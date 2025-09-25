# Highlight.js

[![Build Status](https://travis-ci.org/isagalaev/highlight.js.svg?branch=main)](https://travis-ci.org/isagalaev/highlight.js)

Highlight.js는 JavaScript로 작성된 문법 하이라이터입니다. 브라우저와 서버 모두에서 동작하며, 거의 모든 마크업과 함께 사용할 수 있고 특정 프레임워크에 의존하지 않으며 자동 언어 감지 기능을 제공합니다.

## 시작하기

웹 페이지에서 highlight.js를 사용하기 위한 최소한의 설정은 라이브러리와 스타일 중 하나를 불러오고 [`initHighlightingOnLoad`][1]를 호출하는 것입니다.

```html
<link rel="stylesheet" href="/path/to/styles/default.css">
<script src="/path/to/highlight.pack.js"></script>
<script>hljs.initHighlightingOnLoad();</script>
```

이렇게 하면 `<pre><code>` 태그 안의 코드를 찾아 하이라이트하며, 언어를 자동으로 감지하려 시도합니다. 자동 감지가 잘 동작하지 않는다면 `class` 속성으로 언어를 지정할 수 있습니다.

```html
<pre><code class="html">...</code></pre>
```

지원되는 언어 클래스 목록은 [클래스 레퍼런스][2]에서 확인할 수 있습니다. 클래스 이름 앞에 `language-` 또는 `lang-` 접두사를 붙여도 됩니다.

하이라이팅을 완전히 비활성화하려면 `nohighlight` 클래스를 사용하세요.

```html
<pre><code class="nohighlight">...</code></pre>
```

## 사용자 정의 초기화

highlight.js 초기화를 좀 더 세밀하게 제어하려면 [`highlightBlock`][3]과 [`configure`][4] 함수를 사용하세요. 이를 통해 *무엇을*, *언제* 하이라이트할지 조절할 수 있습니다.

다음은 jQuery를 사용해 [`initHighlightingOnLoad`][1]와 동일한 동작을 구현한 예시입니다.

```javascript
$(document).ready(function() {
  $('pre code').each(function(i, block) {
    hljs.highlightBlock(block);
  });
});
```

`<pre><code>` 대신 원하는 태그를 사용해 코드를 마크업할 수도 있습니다. 줄바꿈을 유지하지 않는 컨테이너를 사용한다면 highlight.js가 `<br>` 태그를 사용하도록 설정해야 합니다.

```javascript
hljs.configure({useBR: true});

$('div.code').each(function(i, block) {
  hljs.highlightBlock(block);
});
```

기타 옵션은 [`configure`][4] 문서를 참고하세요.

## 웹 워커

매우 큰 코드 조각을 처리할 때 브라우저 창이 멈추지 않도록 웹 워커 안에서 하이라이팅을 실행할 수 있습니다.

메인 스크립트 예시는 다음과 같습니다.

```javascript
addEventListener('load', function() {
  var code = document.querySelector('#code');
  var worker = new Worker('worker.js');
  worker.onmessage = function(event) { code.innerHTML = event.data; }
  worker.postMessage(code.textContent);
})
```

`worker.js`는 다음과 같습니다.

```javascript
onmessage = function(event) {
  importScripts('<path>/highlight.pack.js');
  var result = self.hljs.highlightAuto(event.data);
  postMessage(result.value);
}
```

## 라이브러리 받기

highlight.js는 호스팅된 브라우저 스크립트 또는 커스텀 빌드 스크립트, 그리고 서버 모듈 형태로 사용할 수 있습니다. 기본 브라우저 스크립트는 AMD와 CommonJS를 모두 지원하므로 빌드 과정 없이도 RequireJS나 Browserify를 사용할 수 있습니다. 서버 모듈 역시 Browserify와 잘 동작하지만, 서버보다는 브라우저에 특화된 빌드 옵션도 선택할 수 있습니다. 자세한 옵션은 [다운로드 페이지][5]를 참고하세요.

**GitHub에 직접 링크하지 마세요.** 라이브러리는 소스 상태 그대로 사용할 수 없으며 빌드가 필요합니다. 사전 패키징된 옵션이 맞지 않는다면 [빌드 문서][6]를 참고하세요.

**CDN으로 제공되는 패키지에는 모든 언어가 포함되어 있지 않습니다.** 전체를 포함하기엔 용량이 너무 큽니다. ["Common" 섹션][5]에서 원하는 언어를 찾을 수 없다면 수동으로 추가할 수 있습니다.

```html
<script src="//cdnjs.cloudflare.com/ajax/libs/highlight.js/9.4.0/languages/go.min.js"></script>
```

**Almond 사용 시 참고.** 옵티마이저를 사용해 모듈 이름을 지정해야 합니다. 예시는 다음과 같습니다.

```
r.js -o name=hljs paths.hljs=/path/to/highlight out=highlight.js
```

## 라이선스

Highlight.js는 BSD 라이선스로 배포됩니다. 자세한 내용은 [LICENSE][7] 파일을 참고하세요.

## 링크

공식 사이트: <https://highlightjs.org/>

API 및 기타 주제에 대한 자세한 문서는 <https://highlightjs.readthedocs.io/>에서 확인할 수 있습니다.

저자와 기여자 목록은 [AUTHORS.en.txt][8] 파일에 정리되어 있습니다.

[1]: https://highlightjs.readthedocs.io/en/latest/api.html#inithighlightingonload
[2]: https://highlightjs.readthedocs.io/en/latest/css-classes-reference.html
[3]: https://highlightjs.readthedocs.io/en/latest/api.html#highlightblock-block
[4]: https://highlightjs.readthedocs.io/en/latest/api.html#configure-options
[5]: https://highlightjs.org/download/
[6]: https://highlightjs.readthedocs.io/en/latest/building-testing.html
[7]: https://github.com/isagalaev/highlight.js/blob/main/LICENSE
[8]: https://github.com/isagalaev/highlight.js/blob/main/AUTHORS.en.txt
