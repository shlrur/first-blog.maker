---
layout:     post
title:      "[webpack] Why using webpack"
subtitle:   "Modules"
categories: develog
tags:       api
comments:   true
---

기존의 React Boilerplate들은 여러 종류의 package를 사용하지만, 그중에서 webpack과 babel은 거의 필수로 사용하고 있습니다. 이번 포스트에서는 필수 third party API 중 Frontend에서 bundling을 위해 가장 많이 사용하는 **Webpack**이 필요한 이유에 대해서 알아보려 합니다.

> webpack is a static module bundler for modern JavaScript applications.

Webpack 공식 홈페이지의 concept을 설명하는 제일 첫 줄에 있는 문구입니다. 중요한 문장이니만큼 **module**과 **Bundle**에 대해서 알아봐야겠습니다.

# Contents

* [Module](#module)
* [Bundle](#bundle)
  * [OLD way...](#old-way)
  * [Static File Bundling](#static-file-bundling)
* [Webpack](#webpack)
  * [Concepts](#concepts)
  * [Entry](#entry)
  * [Output](#output)
  * [Loaders](#loaders)
  * [Plugins](#plugins)
  * [Mode](#mode)
  * [Browser Compatibility](#browser-compatibility)
* [Conclusion](#conclusion)

---

# Module

**Modular Programming**에서, 개발자들은 프로그램을 **module**이라고 부르는 별도의 기능 덩어리(_chunks of functionality_)로 분할합니다.([Low Coupling High Cohesion](https://stackoverflow.com/questions/14000762/what-does-low-in-coupling-and-high-in-cohesion-mean)) 각 module은 전체 프로그램에 비하면 작기 때문에, 검증, 디버깅, 그리고 테스트하는데 비교적 적은 노력이 필요합니다. 그리고, 잘 만들어진 module은 OOP의 요소인 abstraction과 encapsulation을 제공하기 때문에, 전체 프로그램에서 일관된 디자인과 함께 명확한 목적을 가지고 사용됩니다.

Node.js는 처음부터 modular programming을 지원했지만, 현재 web 표준인 ES6의 module은 아직 모든 브라우저에서 지원하고 있지 않습니다. 때문에, third party library를 사용해서 bundling을 해야 합니다. 현재 Web에서 **Modular JavaScript**를 지원하는 여러 third party library들이 존재하며 각각 이점과 한계를 가지고 있습니다.~~만, 여기서는 webpack에 대해서 알아보려 합니다.~~

---

# Bundle

JavaScript에서의 Modular Programming을 위해서는 일반적으로 하나의 module 당 하나의 파일이 사용됩니다. 그러면 여러 파일이 생성되는데, 이 파일들을 사용하기 위해서는 어떤 형태로든 명시를 해 줘야 합니다. 

## OLD way...

<figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/why_using_webpack/0_old-way.png" alt="">
    <figcaption>old way</figcaption>
</figure>

위의 이미지는 우리가 예전에 혹은 간단한 site에서 사용하는 방식입니다.

위의 방식은 세 가지 큰 단점이 있습니다.

첫 번째는 너무 많은 global 변수들 때문에 global namespace가 엉망진창이 된다는 것입니다. 그리고 Library들이 load 되는 순서가 엉켜버리지 않게 조심해야 합니다. jQuery, React, Angular 혹은 lodash같은 third party library들을 먼저 load 한 후에 직접 작성한 component나 service들을 load 해야 합니다.

두 번째는 각 library의 dependency들을 볼 수 없다는 것입니다. 때문에, 특정 코드가 실행될 때, 그 코드에서 사용하는 API의 script 역시 _이미_ load 되었다고 가정하는 수밖에 없습니다. 코드들이 추가될수록, 특정 API를 쓰기 위해서 context를 전환해야 할 수도 있고, HTML에 script 코드가 추가로 필요할 수도 있습니다. 그리고 이 script들이 잘 로드되는지는 직접 실행해서 화면을 보는 수밖에 없습니다...

세 번째는 페이지를 로딩할 때 많은 script 파일의 다운로드로 인해서 네트워크에 bottleneck이 걸릴 수 있다는 점입니다. 

## Static File Bundling

<figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/why_using_webpack/1_static-bundling.png" alt="">
    <figcaption>bundle your anything</figcaption>
</figure>

Web app들의 증가와 현대적인 frontend framework들 덕분에 JavaScript 개발 방식이 변화되기 시작하면서, static file bundling이라는 방식이 나타났습니다.

현재의 bundling은 여러 tool에 의해 frontend에서 수행되지만, backend에서 수행되던 때도 있었습니다. ASP.NET 5 이전 버전의 Microsoft.AspNet.Optimization package 같은 경우가 하나의 예입니다. 하지만 결국에는 JavaScript를 사용하게 되었습니다.

---

# Webpack

## Concepts

다시 webpack으로 돌아오겠습니다.

> webpack is a static module bundler for modern JavaScript applications.

Webpack은 한마디로 **static module bundler**입니다. Webpack을 frontend 프로그램에 적용했을 때 내부에서 일어나는 일을 아주 간단하게 설명해보자면,

1. 프로그램에서 사용하는 모든 module의 연관성을 나타내는 _dependency graph_ 를 작성합니다. (이 그래프는 간단히 설명하자면 바로 위의 그림인 'bundle your anything' 의 왼쪽 graph라고 보시면 됩니다)
2. _Dependency graph_ 에 따라서 모든 module을 적은 수의 파일로 bundle하고, 이 bundle 된 파일이 브라우저에서 load 되어 사용됩니다. (보통 하나의 파일로 만듭니다)

정말 간단합니다. 하지만 이 간단한 과정을 자신의 frontend project에 반영하기 위해서는 다음의 **Core Concept**를 알아야 합니다.

* [Entry](#entry)
* [Output](#output)
* [Loaders](#loaders)
* [Plugins](#plugins)
* [Mode](#mode)
* [Browser Compatibility](#browser-compatibility)

하나씩 알아보겠습니다.

## Entry

Entry point는 간단히 말해서 dependency graph가 시작되는 지점... 즉, root node를 뜻합니다. Webpack은 entry point부터 시작해서 dependency가 있는 module들을 reclusive 하게 탐색해 나갑니다.

기본값은 <kbd>./src/index.js</kbd> 이지만, 직접 설정할 수도 있고, 물론 여러 개의 entry point를 설정할 수도 있습니다. 그러면, 이 값은 어떻게 설정하며, 이 entry point의 설정이 필요한 이유에 대해서도 알아보겠습니다.

### Single Entry (Shorthand) Syntax

**webpack.config.js**
```js
/*** SINGLE ENTRY SYNTAX ***/
module.exports = {
  entry: './path/to/my/entry/file.js'
};
// OR
module.exports = {
  entry: {
    main: './path/to/my/entry/file.js'
  }
};

/*** Object ENTRY SYNTAX ***/
module.exports = {
  entry: {
    app: './src/app.js',
    adminApp: './src/adminApp.js'
  }
};
```

위의 코드는 entry point를 single 혹은 object를 사용한 multi로 설정하는 방식을 보여주고 있습니다. 위에서 눈여겨볼 방식은 **Object entry syntax** 입니다. 프로그램의 확장성을 위한 방식인데, 이와 같은 방식이 왜 필요할까요?

Web application은 최근 AJAX나 여러 기술로 인해서 single-page로 제작되기도 하지만, performance나 여러 이유에 의해서(굳이 single-page로 제작할 필요가 없음) multi-page로 제작되는 경우도 있습니다. Multi-page application인 경우, **page마다 server에서는 새로운 HTML 문서를 전송해 줍니다.** 그리고 이 HTML 문서는 여러 assets를 새로 내려받습니다. 이때, multi entry point를 설정한 이유를 알 수 있습니다. **특정 단위 page마다 다른 entry point를 설정한다면, 현재 page에서 필요하지 않은 module은 load 되지 않기 때문에, 여러 이점을 가져갈 수 있는 것입니다.**

<kbd>optimization.splitChunks</kbd>는 각 page에서 공유할 수 있는 code를 bundle로 만들어 줍니다. Multi-page application에서는 각 entry point에서 공유하는 module이 많기 때문에, 이 기술을 사용한다면 얻을 수 있는 이점이 많습니다. SplitChunks에 대한 자세한 설명은 [여기](https://medium.com/webpack/webpack-4-code-splitting-chunk-graph-and-the-splitchunks-optimization-be739a861366)와 [여기](https://webpack.js.org/plugins/split-chunks-plugin/)를 참조해주세요.

## Output

**Output** 은 _bundle_ 파일이 생성될 위치와 이름을 설정하는 property입니다.

**webpack.config.js**
```js
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  }
};
```

Main output 파일의 기본값은 <kbd>./dist/main.js</kbd>입니다. path의 기본값은 <kbd>./dist</kbd>입니다.

앞서 살펴본 entry point가 여러 개일 때는 어떻게 될까요?

**webpack.config.js**
```js
module.exports = {
  entry: {
    app: './src/app.js',
    search: './src/search.js'
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist'
  }
};

// writes to disk: ./dist/app.js, ./dist/search.js
```

Multi entry point인 경우에는 output의 filename에 [substitution](https://webpack.js.org/configuration/output#output-filename)을 사용해야 합니다.

그 외에 output의 여러 option 중에서 눈여겨볼 만한 것이 있습니다. 바로 _publicPath_ 입니다. 이 옵션은 on-demand-loading이나 이미지 같은 external resource를 loading 할 때 필요합니다. 해당 값이 제대로 설정되어있지 않으면, 404에러를 받을 수 있습니다... 이 옵션에 대한 자세한 설명을 원하시면 [여기](https://webpack.js.org/configuration/output/#output-publicpath)를 참고해주세요.

## Loaders

TypeScript라는 언어가 있습니다. JavaScript와 달리 variable에 type을 선언할 수 있는 특징을 가지는 언어입니다. 하지만 이 언어는 browser에서 바로 처리할 수 없습니다. 이렇게, Browser 혹은 webpack에서 처리할 수 없는 source code를 가지는 module을 JavaScript 코드 또는 data URI로서의 inline image 등으로 변환시키는 것이 Loader의 역할입니다. 그 때문에 module에서 .CSS 파일을 import 하는 것도 가능합니다 :)

Loader를 사용하는 방법은 3가지가 있습니다.

* Configuration: webpack.config.js 파일에서 명시하는, 추천하는 방식입니다.
* Inline: <kbd>import</kbd> 구문마다 명시하는 방법입니다.
* CLI: Shell command에서 지정하는 방식입니다.

이 포스트에서는 Configuration 방식만 알아보도록 하겠습니다. 그 이유는, configuration에 loader의 설정을 명시하는 방법이 loader를 한눈에 보기에 가장 좋고 유지보수에 좋기 때문입니다. 각 loader에 대한 전체적인 overview를 한눈에 볼 수 있는 장점이 있습니다.

### Configuration of Loaders

Webpack configuration에서 loader에 대한 설정을 하기 위해서는 <kbd>module.rules</kbd>를 건드려야 합니다. 아래의 예제 코드를 보겠습니다.

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: 'style-loader' },
          {
            loader: 'css-loader',
            options: {
              modules: true
            }
          },
          { loader: 'sass-loader' }
        ]
      }
    ]
  }
};
```

Loader는 마지막 ues부터 평가/실행을 시작합니다. 위의 코드를 보면, _sass-loader_ 부터 시작해서 _css-loader_ 그리고 _style-loader_ 로 끝이납니다. 

Loader에 대한 개념을 얕게 훑은 후 example도 봤으니, 이제 Loader의 특징에 대해서 알아보겠습니다.

### Loaders Features

* **Loader는 chain 형식으로 설정될 수 있습니다.** Chain 구조 내에서는 loader들이 연결되어있고, 각 loader의 output은 다음 loader의 input으로 들어갑니다. 마지막 loader는 아마 JavaScript 코드를 출력할 것입니다.
* Synchronous와 Asynchronous 모두 가능합니다.
* Loader는 **Node.js**에서 실행되므로, Node.js에서 가능한 모든 것을 할 수 있습니다.
* 각 Loader의 설정은 <kbd>options</kbd> 객체로 구성할 수 있습니다. (<kbd>query</kbd> parameter는 가능하긴 하지만 deprecated 된 방식입니다.)
* Normal modules can export a loader in addition to the normal main via package.json with the loader field.
* Plugin은 loader에 더 많은 기능을 제공할 수 있습니다.
* 로더는 추가의 임의 파일을 내보낼 수 있습니다.

Loader는 전처리 function(즉, loader)을 통해서 JavaScript 생태계에 활력을 불어넣습니다. 이를 통해서 compression, packaging, 그리고 language translation 같은 fine-grained logic을 유연하게 사용할 수 있습니다.

## Plugins

Loader가 특정 type의 module을 변환하는 역할을 가진다면, **plugin**은 _bundle 최적화_, _asset 관리_ 그리고 _환경변수 injection_ 같은 광범위한 작업을 수행합니다. Plugin의 목적은 loader가 할 수 없는 다른 어떤 작업을 하는 것입니다.

### Anotomy of Plugins

가져다 쓰는 plugin의 내부는 어떤지 잠시 살펴보겠습니다.

```js
// ConsoleLogOnBuildWebpackPlugin.js
const pluginName = 'ConsoleLogOnBuildWebpackPlugin';

class ConsoleLogOnBuildWebpackPlugin {
  apply(compiler) {
    compiler.hooks.run.tap(pluginName, compilation => {
      console.log('The webpack build process is starting!!!');
    });
  }
}
```

위의 코드는 **ConsoleLogOnBuildWebpackPlugin** 이라는 plugin입니다. Webpack plugin은 <kbd>apply</kbd>라는 함수를 가지는 JavaScript object입니다.(class지만 object입니다...) 이 <kbd>apply</kbd> 함수는 webpack compiler에서 호출되며, compliation lifecycle 중에 언제라도 호출될 수 있습니다. 한 가지 유의해야 할 사항은, <kbd>compiler.hooks.run.tap</kbd>함수의 첫 번째 parameter는 plugin 이름의 upper camel case형이어야 하며, 이는 모든 hook에서 사용될 수 있도록 상수를 사용하는 것이 좋습니다.

### Usage of Plugins

```js
// webpack.config.js
const HtmlWebpackPlugin = require('html-webpack-plugin'); //installed via npm
const webpack = require('webpack'); //to access built-in plugins
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    filename: 'my-first-webpack.bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        use: 'babel-loader'
      }
    ]
  },
  plugins: [
    new webpack.ProgressPlugin(),
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
};
```

Plugin은 arguments/options를 가질 수 있으므로, <kbd>new</kbd>를 사용해서 instance를 전달해야 합니다.

## Mode

**Mode**는 <kbd>development</kbd>, <kbd>production</kbd> 그리고 <kbd>none</kbd>으로 설정할 수 있는데, 각각의 값에 따라서 내부적으로 최적화되게 되어있습니다. Default는 <kbd>production</kbd>입니다.

```js
module.exports = {
  mode: 'production'
};
```

<kbd>production</kbd> mode를 사용하려면 위의 코드처럼 사용하시면 됩니다.

그럼, 각각의 mode에 대해서 알아보겠습니다.

### development Mode

* <kbd>DefinePlugin</kbd>의 <kbd>process.env.NODE_ENV</kbd>값을 <kbd>development</kbd>로 설정합니다.
* <kbd>NamedChunksPlugin</kbd>과 <kbd>NamedModulesPlugin</kbd>이 가능합니다.

```js
// webpack.development.config.js
module.exports = {
  mode: 'development'
//devtool: 'eval',
//cache: true,
//performance: {
//  hints: false
//},
//output: {
//  pathinfo: true
//},
//optimization: {
//  namedModules: true,
//  namedChunks: true,
//  nodeEnv: 'development',
//  flagIncludedChunks: false,
//  occurrenceOrder: false,
//  sideEffects: false,
//  usedExports: false,
//  concatenateModules: false,
//  splitChunks: {
//    hidePathInfo: false,
//    minSize: 10000,
//    maxAsyncRequests: Infinity,
//    maxInitialRequests: Infinity,
//  },
//  noEmitOnErrors: false,
//  checkWasmTypes: false,
//  minimize: false,
//},
//plugins: [
//  new webpack.NamedModulesPlugin(),
//  new webpack.NamedChunksPlugin(),
//  new webpack.DefinePlugin({ "process.env.NODE_ENV": JSON.stringify("development") }),
//]
}
```

### production Mode

* <kbd>DefinePlugin</kbd>의 <kbd>process.env.NODE_ENV</kbd>값을 <kbd>production</kbd>으로 설정합니다.
* <kbd>FlagDependencyUsagePlugin</kbd>, <kbd>FlagIncludedChunksPlugin</kbd>, <kbd>ModuleConcatenationPlugin</kbd>, <kbd>NoEmitOnErrorsPlugin</kbd>, <kbd>OccurrenceOrderPlugin</kbd>, <kbd>SideEffectsFlagPlugin </kbd> 그리고 <kbd>UglifyJsPlugin</kbd>이 가능합니다.

```js
// webpack.production.config.js
module.exports = {
  mode: 'production',
//performance: {
//  hints: 'warning'
//},
//output: {
//  pathinfo: false
//},
//optimization: {
//  namedModules: false,
//  namedChunks: false,
//  nodeEnv: 'production',
//  flagIncludedChunks: true,
//  occurrenceOrder: true,
//  sideEffects: true,
//  usedExports: true,
//  concatenateModules: true,
//  splitChunks: {
//    hidePathInfo: true,
//    minSize: 30000,
//    maxAsyncRequests: 5,
//    maxInitialRequests: 3,
//  },
//  noEmitOnErrors: true,
//  checkWasmTypes: true,
//  minimize: true,
//},
//plugins: [
//  new UglifyJsPlugin(/* ... */),
//  new webpack.DefinePlugin({ "process.env.NODE_ENV": JSON.stringify("production") }),
//  new webpack.optimize.ModuleConcatenationPlugin(),
//  new webpack.NoEmitOnErrorsPlugin()
//]
}
```

### none Mode

* Default 최적화 옵션이 적용됩니다.

```js
// webpack.custom.config.js
module.exports = {
  mode: 'none',
//performance: {
// hints: false
//},
//optimization: {
//  flagIncludedChunks: false,
//  occurrenceOrder: false,
//  sideEffects: false,
//  usedExports: false,
//  concatenateModules: false,
//  splitChunks: {
//    hidePathInfo: false,
//    minSize: 10000,
//    maxAsyncRequests: Infinity,
//    maxInitialRequests: Infinity,
//  },
//  noEmitOnErrors: false,
//  checkWasmTypes: false,
//  minimize: false,
//},
//plugins: []
}
```

### Tip of mode

Mode에 따라서 webpack config의 설정값을 바꾸고 싶은 경우에는, 아래의 코드와 같이 object가 아닌 function을 export 하면 됩니다.

```js
var config = {
  entry: './app.js'
  //...
};

module.exports = (env, argv) => {

  if (argv.mode === 'development') {
    config.devtool = 'source-map';
  }

  if (argv.mode === 'production') {
    //...
  }

  return config;
};
```

## Browser Compatibility

Webpack은 [ES5를 준수하는](https://kangax.github.io/compat-table/es5/) 모든 browser를 지원합니다. 구식의 browser에서도 작동하길 원한다면, [pollyfill을 로드](https://webpack.js.org/guides/shimming/)해야 합니다.

---

# Conclusion

지금까지 webpack을 사용해야 하는 이유와 함께 큼직큼직한 개념들에 대해서 간단히 살펴봤습니다. 대략 정리해보자면, webpack은 frontend application을 module 별로 정리해서 bundling 하는 **static module bundler**입니다. 하나 혹은 여러 개의 entry point를 설정할 수 있고, 그에 따른 bundle file의 이름과 경로도 설정할 수 있습니다. Bundling 하는 과정에서 browser가 인식하지 못하는 type의 module을 발견하면 이를 적절한 code로 변환할 수 있으며, 최적화나 assets 관리 그리고 환경변수 주입도 가능합니다. 그리고 development/production/none 중에서 mode를 선택해서 내부적으로 최적화를 수행할 수도 있습니다.

이 포스트가 새로운 Frontend project를 시작하기에 앞서 webpack에 대한 개념을 정리하는 데 도움이 되길 바랍니다.

---

# References

* [webpack's concepts](https://webpack.js.org/concepts/)
* [Modern approach of JavaScript bundling with Webpack](https://medium.com/@andrejsabrickis/modern-approach-of-javascript-bundling-with-webpack-3b7b3e5f4e7)
* [SplitChunksPlugin](https://webpack.js.org/plugins/split-chunks-plugin/)
* [webpack 4: Code Splitting, chunk graph and the splitChunks optimization](https://medium.com/webpack/webpack-4-code-splitting-chunk-graph-and-the-splitchunks-optimization-be739a861366)