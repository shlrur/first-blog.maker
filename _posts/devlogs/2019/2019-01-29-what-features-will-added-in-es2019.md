---
layout:     post
title:      "What features will added in ES2019"
subtitle:   "New Features in JavaScript!!"
categories: develog
tags:       javascript
comments:   true
---

이번 post에서는 2019년에 발표될 **ES2019**에 추가될 예정인 기능들에 대해서 살펴보겠습니다.

<figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/what-features-will-added-in-es2019/article-logo.jpg" alt="new features in es2019">
</figure>

JavaScript는 지난 몇 년 동안 꾸준히 새로운 표준을 발표해왔습니다. 바로 Ecma International의 ECMA-262 기술 규격에 정의된 [ECMAScript](https://www.ecma-international.org/publications/standards/Ecma-262.htm) 인데요, 현재 9번째 버전인 [ES2018](https://www.ecma-international.org/ecma-262/9.0/index.html)까지 나와 있습니다.
그리고 현재 ES2019에 대한 명세가 작성되고 있습니다. 이번 post에서는 ECMAScript가 어떤 과정을 거쳐서 표준 문서를 만드는지 간단히 알아보고, ES2019에 들어갈 수도 있는 기능에 대해서 알아보겠습니다.

---

# Contents

* [The TC39 Process](#the-tc39-process)
* [ES2019 Candidates](#es2019-candidates)
  * [Stage 3 Features](#Stage 3 Features)
    * [globalThis](#globalThis)
    * [import()](#import())
    * [Legacy RegExp features in JavaScript](#Legacy RegExp features in JavaScript)
    * [BigInt](#BigInt)
    * [Array.prototype.{flat,flatMap}](#Array.prototype.{flat,flatMap})
    * [String.prototype.{trimStart,trimEnd}](#String.prototype.{trimStart,trimEnd})
    * [String.prototype.matchAll](#String.prototype.matchAll)
    * [Object.fromEntries](#Object.fromEntries)
    * [Well-formed JSON.stringify](#Well-formed JSON.stringify)
  * [Finished Proposal Features](#Finished Proposal Features)
    * [Optional catch binding](#Optional catch binding)
    * [JSON superset](#JSON superset)
    * [Symbol.prototype.description](#Symbol.prototype.description)
    * [Function.prototype.toString revision](#Function.prototype.toString revision)
* [References](#references)

---

# The TC39 Process

[TC39](https://github.com/tc39)란 ECMA에서 ECMA-262에 대한 표준을 정의하는 기술 위원회(technical committee)입니다. [TC39에서 ECMAScript를 정의하는 절차](https://tc39.github.io/process-document/)는 Stage로 구분하며 다음과 같습니다.

* Stage-0: 밀짚 인형(Strawman)
  * Allow input into the specification
* Stage-1: 제안(Proposal)
  * Make the case for the addition
  * Describe the shape of a solution
  * Identify potential challenges
* Stage-2: 초고(Draft)
  * Precisely describe the syntax and semantics using formal spec language
* Stage-3: 후보(Candidate)
  * Indicate that further refinement will require feedback from implementations and users
* Stage-4: 완료(Finished)
  * Indicate that the addition is ready for inclusion in the formal ECMAScript standard

앞의 Stage(0~4)에서 새로 추가될 것으로 이야기됐던 기능들이 Stage 4까지 오지 않는 한 ECMAScript 표준에 추가된다는 보장은 없습니다. 현재(2019년 1월 29일) [ES2019 proposals](https://github.com/tc39/proposals)를 보면, Stage 4인 Finished Proposal에 들어간 기능도 있고 아직 Stage 3에서 수락을 기다리고 있는 기능도 있습니다. 그러므로 아래에서 살펴볼 기능 중에서 Stage 3에 있는 후보들은 ES2019에 포함되지 않을 수도 있습니다. 하지만, Stage 3까지 논의됐다는 것만으로도 충분히 흥미롭고 중요한 기능입니다.

그리고 ECMAScript에 표준으로 아직 추가되지 않은 기능이지만 몇몇 JavaScript engine(V8, SpiderMonkey 등)에서는 그 기능들을 구현한 경우도 있습니다. 어떤 JavaScript engine에서 제공하는지는 [Can I Use?](https://caniuse.com/) 에서 찾아볼 수 있습니다.

---

# ES2019 Candidates

## Stage 3 Features
Stage 3에 있는 Candidate Feature들은 대부분 완성에 가깝고, 구현 주체나 사용자들로부터 피드백을 좀 더 받아보는 일만이 남은 상태입니다. Stage 3에 들어오기 위해서는 Stage 2의 Draft와는 다르게 빈칸 없이 문법, 동작, 그리고 API까지 모든 부분이 기술되어 있도록 마무리된 명세가 필요합니다.

Stage 3까지 올라온 feature는 이후 구현상 심각한 문제가 발견되지 않는 이상 변경이 허용되지 않습니다. 이 시점에서는 실제로 ECMA-262 표준에 편입시키고자 하는 해당 표준의 명세가 거의 마무리 된 상태여야 합니다.

아래에서는 Stage 3 proposal 중에서 Stage 4로 갈 확률이 높은 feature를 살펴보겠습니다. (test를 위한 pull request가 merge 되었는지를 기준으로 하였습니다)

### globalThis

ECMAScript를 사용해서 코드를 작성할 때, 어느 환경에서나 적용할 수 있도록 _**global object**에 접근하는 것은 어렵습니다._

* Web에서는 **Window** 혹은 **self**, **this** 그리고 **frames**를 통해 _**global object**_ 에 접근 가능합니다.
* node.js에서는 **global** 혹은 **this**로 접근 가능합니다.
* 이 중에서, **this**만이 shell(V8의 d8이나 JavaScriptCore의 jsc같은)에서 사용 가능합니다.
* 엉성한 standalone function call에서 역시 **this**는 작동하지만, module이나 strict mode의 function에서 **this**는 undefined입니다.
* **Function('return this')()** 로 global object에 접근할 수 있지만, Chrome Apps와 같은 CSP setting에서는 접근 불가능합니다.

위와 같은 문제때문에, ECMAScript를 사용하는 환경에 상관없이 global object에 접근하기가 힘듭니다.

이를 해결하기 위해서 제안된 것이 [**globalThis**](https://github.com/tc39/proposal-global)입니다. **globalThis** property는 환경에 상관없이 global object에 접근 가능한 standard way입니다.

**globalThis**가 있다면, 아래와 같이 여러 환경을 고려해서 global object를 가져오는 code가 필요 없어집니다.

```js
function foo() {
	// If we're in a browser, the global namespace is named 'window'. If we're
	// in node, it's named 'global'. If we're in a shell, 'this' might work.
	(typeof window !== "undefined"
		? window
		: (typeof process === 'object' &&
		   typeof require === 'function' &&
		   typeof global === 'object')
			? global
			: this);
}
```

```js
var getGlobal = function () {
	// the only reliable means to get the global object is
	// `Function('return this')()`
	// However, this causes CSP violations in Chrome apps.
	if (typeof self !== 'undefined') { return self; }
	if (typeof window !== 'undefined') { return window; }
	if (typeof global !== 'undefined') { return global; }
	throw new Error('unable to locate global object');
};
```
<br>
<br>

### import()

[import()](https://github.com/tc39/proposal-dynamic-import)는 기존의 _pre-runtime에 import를 하는 정적인 방식_ 이 아닌, **runtime에 import할 수 있는 동적인 방식** 을 제안하고 있습니다.

#### motivation
현재 module import를 위해 사용하는 방식은 정적인 선언(static declarations)입니다. Import 할 module의 식별자로 문자열을 받고, pre-runtime에 "linking" process를 거침으로서 local scope에 binding합니다. 이런 정적인 선언은 90%의 경우에 매우 좋은 방식입니다.<br>
하지만, runtime에 JavaScript application의 일부를 load할 수 있는 것도 바람직한 방법입니다. 그 이유는 다음과 같습니다. 사용자의 언어와 같은 **runtime에만 알 수 있는 요소**들이 있습니다. 그리고 사용되지 않을 코드는 load하지 않기 때문에 **performance 측면**에서도 이유가 될 수 있습니다. 혹은 중요하지 않은 module을 load했을 때 생기는 문제를 피함으로서 견고함에도 이유가 될 수 있습니다.

#### Proposed solution
해당 proposal은 function처럼 동작하는 **import(specifier)**를 제안합니다. **이 함수는 request module에 대한 module namespace object를 가지는 promise를 return합니다. 해당 module namespace object는 fetching, instantiating, 그리고 관련된 다른 module에 대한 evaluating까지 완료한 후 자신의 module로서 제공됩니다.**
import(specifier)의 **specifier**는 string literal뿐만 아니라 backtick 형태도 지원합니다.

#### Example
```html
<!DOCTYPE html>
<nav>
  <a href="books.html" data-entry-module="books">Books</a>
  <a href="movies.html" data-entry-module="movies">Movies</a>
  <a href="video-games.html" data-entry-module="video-games">Video Games</a>
</nav>

<main>Content will load here!</main>

<script>
  const main = document.querySelector("main");
  for (const link of document.querySelectorAll("nav > a")) {
    link.addEventListener("click", e => {
      e.preventDefault();

      import(`./section-modules/${link.dataset.entryModule}.js`)
        .then(module => {
          module.loadPageInto(main);
        })
        .catch(err => {
          main.textContent = err.message;
        });
    });
  }
</script>
```
<br>
<br>

### Legacy RegExp features in JavaScript

[해당 proposal](https://github.com/tc39/proposal-regexp-legacy-features)은 **RegExp.$1와 같은 생성자의 static properties**와 **RegExp.prototype.compile method**가 RegExp에서 deprecated 된 것에 문제를 제기하고 있습니다.

[해당 기능들이 deprecated된 이유는 encapsulation을 위반했기 때문이라는 설명](https://github.com/tc39/proposal-regexp-legacy-features/blob/master/subclass-restriction-motivation.md)을 하며, 해결책을 제안하고 있습니다.


<br>
<br>

### BigInt

[BigInt](https://github.com/tc39/proposal-bigint)는 2<sup>53</sup> 보다 큰 수를 표현하기 위한 새로운 primitive type입니다. (JavaScript에서 기존의 숫자를 표현하는 primitive type인 Number는 2<sup>53</sup>까지만 표현 가능합니다.)

BigInt의 syntax는 다음과 같이 integer의 끝에 'n'을 붙이거나 생성자를 호출합니다.

```js
const theBiggestInt = 9007199254740991n;

const alsoHuge = BigInt(9007199254740991);
// ↪ 9007199254740991n

const hugeButString = BigInt('9007199254740991');
// ↪ 9007199254740991n
```

+, -, *, ** 그리고 % 같은 연산들 역시 가능합니다. 그 외에 comparison, conditional 등등의 여러 usecase와 gotcha에 대한 설명은 [여기](https://github.com/tc39/proposal-bigint)를 참조하세요.

<br>
<br>

### Array.prototype.{flat,flatMap}
> 특히 finished proposal로 넘어갈 확률이 높은 proposal입니다.
[해당 proposal](https://github.com/tc39/proposal-flatMap)은 Array.prototype.flat과 Array.prototype.flatMap을 ECMAScript에 추가하려 합니다.

flat()은 여러 depth를 가지는 array를 한 depth씩 혹은 여러 depth씩 평평하게(flatten) 합니다.

```js
const nestedArr = [1, [2, [3, [4]]]];

const flat1Depth = nestedArr.flat(1); // == nestedArr.flat(); [1, 2, [3, [4]]]
const flat2Depth = nestedArr.flat(2); // [1, 2, 3, [4]]
const flat3Depth = nestedArr.flat(3); // [1, 2, 3, 4]
const flatMoreDepth = nestedArr.flat(10000); // [1, 2, 3, 4]
```

flatMap()은 map()과 유사하게 array를 return합니다. 하지만 flatMap은 return되는 array를 한 depth 평평하게 합니다.
```js
const example1 = [2, 3, 4].flatMap((x) => [x, x * 2]);
// example1 === [2, 4, 3, 6, 4, 8]

const example2_1 = ['I am', 'not a', 'boy'].map((d) => {return d.split(' ')});
// example2_1 === [["I", "am"], ["not", "a"], ["boy"]]
const example2_2 = ['I am', 'not a', 'boy'].flatMap((d) => {return d.split(' ')});
// example2_2 === ["I", "am", "not", "a", "boy"]
```

<br>
<br>

### String.prototype.{trimStart,trimEnd}

String type은 trim()이라는, 양쪽의 whitespace를 제거하는 method를 표준으로 가지고 있습니다. [해당 proposal](https://github.com/tc39/proposal-string-left-right-trim)은 string의 양쪽이 아닌 한쪽의 whitespace만 제거하는 trimStart()와 trimEnd()라는 method를 제안합니다.

재미있는 점은, 해당 method는 표준에 제정되기 전에 [여러 browser에서 이미 구현](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/trimEnd#Browser_compatibility)되어 있습니다. Browser에서 ECMAScript로 기능을 넣게하는 경우 중 하나입니다.

```js
const first = "      My favorite sport ";
const second = "is kendo.        ";
console.log(first.trimStart() + second.trimEnd()) // "My favorite sport is kendo."
```

기능의 추가 외에도, naming/aliasing 도 제안하고 있습니다. ES2017에서 표준으로 추가된 **padStart/padEnd**와 일관성을 유지하기 위해서 **trimLeft/trimRight** 대신 **trimStart/trimEnd**를 제안하고 있습니다.

<br>
<br>

### String.prototype.matchAll

[해당 proposal은 **String.prototype.exec**이 단일 결과값을 return하는 것을 보완하여, 모든 결과값을 return하는 **String.prototype.matchAll**을 제안하고 있습니다.](https://github.com/tc39/proposal-string-matchall)

기존의 [String.prototype.exec은 단일 결과값을 이러한 format](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec#%EC%84%A4%EB%AA%85)으로 보여줍니다. exec()를 사용해서 모든 만족하는 결과를 얻기 위해서는 loop를 사용해야 합니다. 이런 과정을 거쳐서 주어진 string에서 정규표현식과 일치하는 모든 부분을 찾는 logic은 아래의 코드와 같습니다.

```js
var regex = /t(e)(st(\d?))/g;
var string = 'test1test2';

var matches = [];
var lastIndexes = {};
var match;
lastIndexes[regex.lastIndex] = true;
while (match = regex.exec(string)) {
	lastIndexes[regex.lastIndex] = true;
	matches.push(match);
	// example: ['test1', 'e', 'st1', '1'] with properties `index` and `input`
}
matches; /* gives exactly what i want, but uses a loop,
		* and mutates the regex's `lastIndex` property */
lastIndexes; /* ideally should give { 0: true } but instead
		* will have a value for each mutation of lastIndex */
```

위의 코드는 **matches** 변수에 원하는 결과가 들어있지만, loop를 사용했고, regex의 lastIndex property를 변경했다는 단점이 있습니다.

아래의 코드는 String.prototype.exec가 아닌 **String.prototype.replace**의 두 번째 parameter에 function을 사용한 경우입니다.

```js
var regex = /t(e)(st(\d?))/g;
var string = 'test1test2';

var matches = [];
string.replace(regex, function () {
	var match = Array.prototype.slice.call(arguments, 0, -2);
	match.input = arguments[arguments.length - 1];
	match.index = arguments[arguments.length - 2];
	matches.push(match);
	// example: ['test1', 'e', 'st1', '1'] with properties `index` and `input`
});
matches; /* gives exactly what i want, but abuses `replace`,
	  * mutates the regex's `lastIndex` property,
	  * and requires manual construction of `match` */
```

위의 코드는 앞의 코드가 가지는 단점을 보완했지만, replace 함수를 남용하였고, 역시 regex의 lastIndex property를 변경했다는 단점이 있습니다. 그리고 match 변수를 직접 작성해야 합니다.

이러한 단점들이 있기 때문에, 주어진 string에서 정규표현식과 일치하는 모든 결과값을 return하는 **String.prototype.matchAll** 을 표준에 포함하길 제안하고 있습니다.

<br>
<br>

### Object.fromEntries
> 특히 finished proposal로 넘어갈 확률이 높은 proposal입니다.

<br>
<br>

### Well-formed JSON.stringify


## Finished Proposal Features
마지막 Stage 4는 모든 단계를 거치고 마침내 제안이 수락되고 다음 표준에 포함되어 발표되기만을 기다리는 단계입니다. Stage 3의 proposal이 ECMA-262의 unit test suit인 Test262에 관련 테스트가 작성되고, 최소 2개 이상의 구현이 제공되는 등의 까다로운 추가 조건을 모두 만족하면 마침내 Stage 4로 올라올 수 있습니다.

Stage 4까지 올라온 proposal은 별다른 이변이 없는 이상 다가오는 새 표준에 포함되어 발표됩니다. 2015년을 기점으로 매년 6월 새로운 ECMAScript 표준이 발표되는데, 당해 3월 전까지 Stage 4를 달성하고 3월 회의에서 최종 승인된 제안들이 새 표준에 포함됩니다.

현재 ES2019에 포함될 것으로 예상되는, finished proposal은 다음과 같습니다.

### Optional catch binding

<br>
<br>

### JSON superset

<br>
<br>

### Symbol.prototype.description

<br>
<br>

### Function.prototype.toString revision


---

# References

* [ECMAScript](https://www.ecma-international.org/publications/standards/Ecma-262.htm)
* [ES2018](https://www.ecma-international.org/ecma-262/9.0/index.html)
* [TC39](https://github.com/tc39)
* [The TC39 Process](https://tc39.github.io/process-document/)
* [ECMAScript Proposals](https://github.com/tc39/proposals)
* [ECMAScript와 TC39](https://ahnheejong.name/articles/ecmascript-tc39/)
* [globalThis](https://github.com/tc39/proposal-global)
* [import()](https://github.com/tc39/proposal-dynamic-import)
* [Legacy RegExp features in JavaScript](https://github.com/tc39/proposal-regexp-legacy-features)
* [Why disable legacy RegExp features for proper subclasses of RegExp?](https://github.com/tc39/proposal-regexp-legacy-features/blob/master/subclass-restriction-motivation.md)
* [BigInt: Arbitrary precision integers in JavaScript](https://github.com/tc39/proposal-bigint)
* [Array.prototype.{flat,flatMap}](https://github.com/tc39/proposal-flatMap)
* [String.prototype.{trimStart,trimEnd}](https://github.com/tc39/proposal-string-left-right-trim)
* [String.prototype.matchAll](https://github.com/ljharb/String.prototype.matchAll)