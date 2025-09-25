# SVG 기여 가이드

대부분의 이미지는 SVG 형식이며 마블 다이어그램을 나타냅니다.
이러한 SVG는 javadoc JAR에 포함되므로 소스 계층 구조 내 `doc-files` 폴더에 위치해야 합니다.

따라서 대부분의 SVG는 `/reactor-core/src/main/java/reactor/core/publisher/doc-files/marbles/`에 있습니다.

SVG 마블 다이어그램의 그래픽 규칙은 최적화되지 않은 원본 SVG인 `/docs/svg/conventions.svg`에 정리되어 있습니다.
이 파일에서 그래픽 요소를 가져와 새 SVG 마블 다이어그램에 추가하면 기여에 도움이 됩니다.

이 규칙 가운데 일부는 독자 관점에서 마블 다이어그램을 읽는 방법을 설명하도록 추출되어 재가공되었습니다.
이러한 `legend-` 접두사의 SVG는 레퍼런스 문서에 특화되어 있습니다.

레퍼런스 문서 전용 이미지는 `/docs/asciidoc/images/` 폴더에 추가할 수 있습니다.
`legend-` SVG처럼 레퍼런스 가이드를 위한 이미지도 최적화해야 하며, 그렇지 않으면 PDF 버전에서 렌더링 문제가 발생합니다.

## 마블 다이어그램 기여하기

새 SVG 마블 다이어그램을 편집하거나 기여할 때는 다음 워크플로를 권장합니다.

 - 표준 SVG 편집기를 사용하세요(크로스 플랫폼 독립 실행형 편집기로는 [`Inkscape`](https://inkscape.org)를 추천합니다).
 - 주요 레이블(예: 박스의 연산자 이름)은 `Tahoma` 글꼴 패밀리를 사용하고 Linux에서는 `Nimbus Sans L`로 폴백하세요.
   - `font-family:'Tahoma','Nimbus Sans L'`
 - 코드처럼 보이게 해야 하는 레이블은 `Lucida Console` 글꼴 패밀리를 사용하고 `Courier New`로 폴백하세요.
   - `font-family:'Lucida Console','Courier New'`
 - 가능하다면 새/편집한 SVG를 최적화하되, 아래 안내처럼 보기 좋은 들여쓰기를 유지하세요.

모든 `*.svg` 마블 다이어그램은 `/reactor-core/src/main/java/reactor/core/publisher/doc-files/marbles/` 폴더에 있습니다.
이 폴더는 `-sources.jar`와 `-javadoc.jar` 아티팩트에도 포함됩니다.

`conventions.svg` 문서는 마블에 반복해서 등장하는 요소(정적 연산자 vs 메서드 연산자, 블로킹, 타임아웃, 람다 등)를 표현하기 위한 그래픽 규칙을 묶어 둡니다. 또한 `/docs/asciidoc/images`에도 더 많은 SVG가 있으며(이 폴더에는 `flux.svg`와 `mono.svg` 마블로 연결되는 심볼릭 링크가 포함됩니다).

### SVG 파일 최적화하기

이미지는 소스와 javadoc JAR에 포함되므로 가능한 한 작은 크기를 유지하고 불필요한 내용을 제거해야 합니다.
따라서 SVG를 편집하거나 새로 만들 때에는 최적화 단계를 거치길 권장합니다.

다만 편집기에서 SVG 소스를 읽기 쉽도록 보기 좋은 들여쓰기를 유지하는 편을 선호합니다.

최적화 과정에서 화살표 머리가 사라지는 문제가 자주 발생하므로 주의 깊게 확인하세요.

권장 최적화 설정은 다음과 같습니다.

 - [svgo](https://github.com/svg/svgo)를 사용하세요(NPM 모듈이며 macOS에서는 Homebrew로 `brew install svgo`로 설치할 수 있습니다).
 - 편집한 파일만 최적화하세요. 여러 파일을 다룰 때는 검증을 위해 임시 폴더에 결과를 내보내는 편이 좋습니다.
 - 리포지터리 루트에서 다음 명령을 실행하면 화살표 머리를 유지하고 보기 좋은 들여쓰기를 유지한 채 SVG를 올바르게 최적화할 수 있습니다.

```sh
svgo --input="reactor-core/src/main/java/reactor/core/publisher/doc-files/marbles/reduce.svg" --multipass --pretty --indent=1 --precision=2 --disable={cleanupIDs,removeNonInheritableGroupAttrs,removeViewBox,convertShapeToPath}
```

TIP: 최적화 전 파일을 git으로 스테이징하고, 최적화 후 렌더링이 올바른지 확인한 다음 문제가 있으면 `git checkout path/to/file.svg`로 되돌릴 수 있습니다.

폴더 전체를 임시 폴더로 최적화하려면 `--folder`와 `--output` 옵션을 사용하세요.

```sh
svgo --folder="reactor-core/src/main/java/reactor/core/publisher/doc-files/marbles/" --multipass --pretty --indent=1 --precision=2 --disable={cleanupIDs,removeNonInheritableGroupAttrs,removeViewBox,convertShapeToPath} --output=/tmp/svg/ --quiet
```

### 최적화 팁

#### 화살표나 다른 요소가 사라질 때
객체의 그룹을 해제해 보세요.
일부 객체는 `conventions.svg` 문서에서 복사/붙여넣기 편의를 위해 그룹화되어 있지만, 실제 사용 시에는 그룹을 풀어야 합니다.

### Inkscape 팁

#### 듀얼 모니터를 사용하는 macOS에서 Inkscape 0.x가 사라지는 문제
안타깝게도 XQuartz 기반 Inkscape는 항상 완벽하게 작동하지 않습니다. 화면에서 사라지는 문제는 좌표계 정렬이 맞지 않아 발생합니다.

macOS `Mission Control` 환경설정에서 `디스플레이마다 별도의 Spaces` 옵션을 비활성화하세요.
이 설정을 변경하면 다시 로그인해야 하며, 메뉴 막대가 이제 도크가 있는 기본 화면에만 표시되는 부작용이 있습니다.

#### Inkscape 0.x에서 문서를 열거나 닫는 속도가 매우 느릴 때
 - 문서를 하나만 남기고 모두 닫은 뒤 도구 대화 상자를 닫고 Inkscape를 다시 시작해 보세요.
 - 환경설정의 `렌더링` 옵션에서 스레드 수, 캐시 크기, 디스플레이 필터 품질을 조정해 보세요.

### 둥근 사각형(버퍼)을 줄이는 방법
사각형을 두 번 클릭하고 왼쪽 위 모서리(네모 핸들)를 선택하세요.
가로로만 이동하려면 Ctrl(또는 Cmd) 키를 누른 채 원하는 위치로 핸들을 옮깁니다.
같은 작업을 오른쪽 아래 핸들에도 반복하세요.
