---
layout:     post
title:      "What features will added in ES2019"
subtitle:   "New Features in JavaScript!!"
categories: develog
tags:       javascript
comments:   true
---

<figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/what-features-will-added-in-es2019/article-logo.jpg" alt="new features in es2019">
</figure>

이번 post에서는 2019년에 발표될 **ES2019**에 추가될 예정인 기능들에 대해서 살펴보겠습니다.

JavaScript는 지난 몇 년 동안 꾸준히 새로운 표준을 발표해왔습니다. 바로 Ecma International의 ECMA-262 기술 규격에 정의된 [ECMAScript](https://www.ecma-international.org/publications/standards/Ecma-262.htm) 인데요, 현재 9번째 버전인 [ES2018](https://www.ecma-international.org/ecma-262/9.0/index.html)까지 나와 있습니다.
그리고 현재 ES2019에 대한 명세가 작성되고 있습니다. 이번 post에서는 ECMAScript가 어떤 과정을 거쳐서 표준 문서를 만드는지 간단히 알아보고, ES2019에 들어갈 수도 있는 기능에 대해서 알아보겠습니다.

---

# Contents

* [The TC39 Process](#the-tc39-process)
* [ES2019 Candidates](#es2019-candidates)
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

앞의 Stage(0~4)에서 새로 추가 될 것으로 이야기 됐던 기능들이 Stage 4까지 오지 않는 한 ECMAScript 표준에 추가된다는 보장은 없습니다. 현재(2019년 1월 29일) [ES2019 proposals](https://github.com/tc39/proposals)를 보면, Stage 4인 Finished Proposal에 들어간 기능도 있고 아직 Stage 3에서 수락을 기다리고 있는 기능도 있습니다. 그러므로 아래에서 살펴 볼 기능 중에서 Stage 3에 있는 후보들은 ES2019에 포함되지 않을 수도 있습니다. 하지만, Stage 3까지 논의됐다는 것 만으로도 충분히 흥미롭고 중요한 기능입니다.

그리고 ECMAScript에 표준으로 아직 추가되지 않은 기능이지만 몇몇 JavaScript engine(V8, SpiderMonkey등)에서는 그 기능들을 구현한 경우도 있습니다. 어떤 JavaScript engine에서 제공하는지는 [Can I Use?](https://caniuse.com/) 에서 찾아볼 수 있습니다.

---

# ES2019 Candidates

## Stage 3 Features
Stage 3에 있는 Candidate Feature들은 대부분 완성에 가깝고, 구현 주체나 사용자들로부터 피드백을 좀 더 받아보는 일만이 남은 상태입니다. Stage 3에 들어오기 위해서는 Stage 2의 Draft와는 다르게 빈칸없이 문법, 동작, 그리고 API까지 모든 부분이 기술되어 있도록 마무리 된 명세가 필요합니다.

Stage 3까지 올라온 feature는 이후 구현상 심각한 문제가 발견되지 않는 이상 변경이 허용되지 않습니다. 이 시점에서는 실제로 ECMA-262 표준에 편입시키고자 하는 해당 표준의 명세가 거의 마무리 된 상태여야 합니다.

아래에서는 Stage 3 proposal 중에서 Stage 4로 갈 확률이 높은 feature를 살펴보겠습니다.(test를 위한 pull request가 merge 되었는지를 기준으로 하였습니다)

### globalThis
### import()
### Legacy RegExp features in JavaScript
### BigInt
### Array.prototype.{flat,flatMap}
### String.prototype.{trimStart,trimEnd}
### String.prototype.matchAll
### Object.fromEntries
### Well-formed JSON.stringify

## Finished Proposal Features
마지막 Stage 4는 모든 단계를 거치고 마침내 제안이 수락되고 다음 표준에 포함되어 발표되기만을 기다리는 단계입니다. Stage 3의 proposal이 ECMA-262의 unit test suit인 Test262에 관련 테스트가 작성되고, 최소 2개 이상의 구현이 제공되는 등의 까다로운 추가 조건을 모두 만족하면 마침내 Stage 4로 올라올 수 있습니다.

Stage 4까지 올라온 프로포절은 별다른 이변이 없는 이상 다가오는 새 표준에 포함되어 발표됩니다. 2015년을 기점으로 매년 6월 새로운 ECMAScript 표준이 발표되는데, 당해 3월 전까지 Stage 4를 달성하고 3월 회의에서 최종 승인된 제안들이 새 표준에 포함됩니다.

현재 ES2019에 포함될것으로 예상되는, finished proposal은 다음과 같습니다.

### Optional catch binding
### JSON superset
### Symbol.prototype.description
### Function.prototype.toString revision

---

# References

* [ECMAScript](https://www.ecma-international.org/publications/standards/Ecma-262.htm)
* [ES2018](https://www.ecma-international.org/ecma-262/9.0/index.html)
* [TC39](https://github.com/tc39)
* [The TC39 Process](https://tc39.github.io/process-document/)
* [ECMAScript Proposals](https://github.com/tc39/proposals)
* [ECMAScript와 TC39](https://ahnheejong.name/articles/ecmascript-tc39/)