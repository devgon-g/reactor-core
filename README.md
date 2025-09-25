# Reactor Core

[![https://gitter.im/reactor/reactor에서 채팅에 참여하세요](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/reactor/reactor?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![Reactor Core](https://maven-badges.herokuapp.com/maven-central/io.projectreactor/reactor-core/badge.svg?style=plastic)](https://mvnrepository.com/artifact/io.projectreactor/reactor-core) [![Latest](https://img.shields.io/github/release/reactor/reactor-core/all.svg)]()

[![CI on GHA](https://github.com/reactor/reactor-core/actions/workflows/publish.yml/badge.svg)](https://github.com/reactor/reactor-core/actions/workflows/publish.yml)
[![CodeQL](https://github.com/reactor/reactor-core/actions/workflows/codeql-analysis.yml/badge.svg)](https://github.com/reactor/reactor-core/actions/workflows/codeql-analysis.yml)

JVM을 위한 비차단 [Reactive Streams](https://www.reactive-streams.org/) 기반으로, [Reactive Extensions](https://reactivex.io)에서 영감을 받은 API와 효율적인 이벤트 스트리밍 지원을 모두 제공합니다.

`3.3.x`부터 이 저장소에는 Reactor 코드를 디버깅하는 데 도움을 주는 자바 에이전트 `reactor-tools`도 포함되어 있습니다.

## 다운로드

**Reactor 3을 실행하려면 Java 8 이상이 필요합니다.**

repo.spring.io 또는 Maven Central 저장소(안정 버전만 제공)에서 Gradle을 사용할 수 있습니다.

```groovy
repositories {
    mavenCentral()

    // 스냅샷을 사용하려면 아래 주석을 해제하세요
    // maven { url "https://repo.spring.io/snapshot" }
}

dependencies {
    compile "io.projectreactor:reactor-core:3.8.0-M7"
    testCompile "io.projectreactor:reactor-test:3.8.0-M7"

    // 최신 스냅샷 아티팩트를 사용하려면 다음을 사용하세요
    // compile "io.projectreactor:reactor-core:3.8.0-SNAPSHOT"
    // testCompile "io.projectreactor:reactor-test:3.8.0-SNAPSHOT"

    // Reactor 코드 디버깅을 돕기 위해 `reactor-tools`를 선택적으로 사용할 수 있습니다
    // implementation "io.projectreactor:reactor-tools:3.8.0-M7"
}
```

Maven 사용 방법, 마일스톤 및 스냅샷 버전 다운로드 방법 등 더 자세한 내용은 [레퍼런스 문서](https://projectreactor.io/docs/core/release/reference/docs/index.html#getting)를 확인하세요.

> **Android 지원 참고**: Reactor 3는 공식적으로 Android를 지원하거나 대상으로 삼지 않습니다.
> 하지만 Android SDK 21(Android 5.0) 이상에서는 문제없이 동작합니다. 자세한 내용은 레퍼런스 가이드의 [전체 안내](https://projectreactor.io/docs/core/release/reference/docs/index.html#prerequisites)를 참고하세요.

## 프로젝트 빌드가 어려우신가요?
[Java Multi-Release JAR File](https://openjdk.org/jeps/238) 지원이 도입된 이후에는 클래스패스에서 JDK 8, 9, 21을 모두 사용할 수 있어야 합니다. Gradle Toolchain이 모든 JDK를 자동으로 [감지](https://docs.gradle.org/current/userguide/toolchains.html#sec:auto_detection)하거나 [제공](https://docs.gradle.org/current/userguide/toolchains.html#sec:provisioning)합니다.

하지만 `No matching toolchains found for requested specification: {languageVersion=X, vendor=any, implementation=vendor-specific}`(여기서 `X`는 8, 9 또는 21)와 같은 오류가 표시되면 누락된 JDK를 설치해야 합니다.

### [SDKMAN!](https://sdkman.io/)으로 JDK 설치하기

프로젝트 루트에서 [SDKMAN 환경 초기화](https://sdkman.io/usage#env)를 실행하세요.

```shell
sdk env install
```

필요하다면 JDK 9를 설치합니다.

```shell
sdk install java $(sdk list java | grep -Eo -m1 '9\b\.[ea|0-9]{1,2}\.[0-9]{1,2}-open')
```

필요하다면 JDK 21도 설치합니다.

```shell
sdk install java $(sdk list java | grep -Eo -m1 '21\b\.[ea|0-9]{1,2}\.[0-9]{1,2}-open')
```

설치가 완료되면 프로젝트를 새로고침하고 빌드가 성공하는지 확인하세요.

### JDK를 수동으로 설치하기

운영체제별 수동 설치 방법은 [공식 문서](https://docs.oracle.com/en/java/javase/20/install/overview-jdk-installation.html)에 자세히 정리되어 있습니다.

### 문서 빌드하기

Antora로 문서를 빌드하려면 현재 쉘에서 사용 중인 JDK 버전이 JDK 17 이상과 호환되어야 합니다. 위에서 설명한 대로 JDK 21을 설치하고 현재 버전으로 설정하세요.

그런 다음 Antora 문서는 다음과 같이 빌드할 수 있습니다.

```shell
./gradlew docs
```

문서는 `docs/build/site/index.html`과 `docs/build/distributions/reactor-core-<version>-docs.zip`에 생성됩니다.

생성된 ZIP 파일에 PDF를 포함하려면 `-PforcePdf` 옵션을 지정하세요.

```shell
./gradlew docs -PforcePdf
```

PDF 생성에는 `asciidoctor-pdf` 명령이 PATH에 존재해야 합니다. 예를 들어 macOS에서는 다음과 같이 설치할 수 있습니다.

```shell
brew install asciidoctor
```

## 시작하기

리액티브 프로그래밍이 처음이거나 글로 읽는 것이 지루하시다면 [Reactor Core Hands-on](https://github.com/reactor/lite-rx-api-hands-on)을 직접 체험해 보세요!

RxJava에 익숙하거나 더 자세한 소개를 보고 싶다면 https://www.infoq.com/articles/reactor-by-example 을 참고하세요.

## Flux

기본 흐름 연산자를 갖춘 Reactive Streams Publisher입니다.
- Flux의 정적 팩토리는 임의의 콜백 타입에서 소스를 생성할 수 있습니다.
- 인스턴스 메서드는 각 구독 시점(_Flux#subscribe()_ 등)에 물질화되거나, _Flux#publish_, _Flux#publishNext_와 같은 멀티캐스팅 연산을 제공합니다.

[<img src="https://raw.githubusercontent.com/reactor/reactor-core/v3.1.3.RELEASE/src/docs/marble/flux.png" width="500" style="background-color: white">](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html)

Flux 사용 예시는 다음과 같습니다.

```java
Flux.fromIterable(getSomeLongList())
    .mergeWith(Flux.interval(100))
    .doOnNext(serviceA::someObserver)
    .map(d -> d * 2)
    .take(3)
    .onErrorResume(errorHandler::fallback)
    .doAfterTerminate(serviceM::incrementTerminate)
    .subscribe(System.out::println);
```

## Mono

적절한 연산자와 함께 *0개* 또는 *1개*의 요소만 내보내는 Reactive Streams Publisher입니다.
- Mono의 정적 팩토리는 임의의 콜백 타입에서 *0 또는 1*개의 시퀀스를 결정적으로 생성합니다.
- 인스턴스 메서드는 _Mono#subscribe()_ 또는 _Mono#get()_ 호출 시점에 물질화되는 연산을 구성할 수 있습니다.

[<img src="https://raw.githubusercontent.com/reactor/reactor-core/v3.4.1/reactor-core/src/main/java/reactor/core/publisher/doc-files/marbles/mono.svg" width="500" style="background-color: white">](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Mono.html)

Mono 사용 예시는 다음과 같습니다.

```java
Mono.fromCallable(System::currentTimeMillis)
    .flatMap(time -> Mono.first(serviceA.findRecent(time), serviceB.findRecent(time)))
    .timeout(Duration.ofSeconds(3), errorHandler::fallback)
    .doOnSuccess(r -> serviceM.incrementSuccess())
    .subscribe(System.out::println);
```

Mono를 블로킹 방식으로 사용하는 방법은 다음과 같습니다.

```java
Tuple2<Instant, Instant> nowAndLater = Mono.zip(
    Mono.just(Instant.now()),
    Mono.delay(Duration.ofSeconds(1)).then(Mono.fromCallable(Instant::now)))
  .block();
```

## Schedulers

Reactor는 [Scheduler](https://projectreactor.io/docs/core/release/api/reactor/core/scheduler/Scheduler.html)를 임의 작업 실행을 위한 계약으로 사용합니다. Scheduler는 FIFO 실행과 같은 Reactive Streams 흐름에 필요한 보장을 제공합니다.

프로듀싱 흐름(subscribeOn)이나 소비 흐름(publishOn)에서 쓰레드를 전환하려면 효율적인 [스케줄러](https://projectreactor.io/docs/core/release/api/reactor/core/scheduler/Schedulers.html)를 사용하거나 직접 생성할 수 있습니다.

```java

Mono.fromCallable(() -> System.currentTimeMillis())
        .repeat()
    .publishOn(Schedulers.single())
    .log("foo.bar")
    .flatMap(time ->
        Mono.fromCallable(() -> { Thread.sleep(1000); return time; })
            .subscribeOn(Schedulers.parallel())
    , 8) //maxConcurrency 8
    .subscribe();
```

## ParallelFlux

[ParallelFlux](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/ParallelFlux.html)는 동시에 수행할 수 있는 작업으로 나눌 수 있는 시퀀스에서 CPU를 최대한 활용할 수 있습니다. `ParallelFlux#sequential()`로 다시 `Flux`로 변환하거나, 무순서 조인 또는 `groups()`를 이용한 다양한 병합 전략을 사용할 수 있습니다.

```java
Mono.fromCallable(() -> System.currentTimeMillis())
        .repeat()
    .parallel(8) //parallelism
    .runOn(Schedulers.parallel())
    .doOnNext(d -> System.out.println("I'm on thread " + Thread.currentThread()))
    .subscribe();
```

## 사용자 정의 소스: Flux.create와 FluxSink, Mono.create와 MonoSink

구독자나 프로세서를 외부 컨텍스트에 연결해야 하고, 그 컨텍스트가 단일 스레드로 데이터를 생성한다면 `Flux#create`, `Mono#create`를 사용하세요.

```java
Flux.create(sink -> {
         ActionListener al = e -> {
            sink.next(textField.getText());
         };

         // 취소 지원 없이 사용하는 경우
         button.addActionListener(al);

         // 취소 지원을 추가하는 경우
         sink.onCancel(() -> {
                button.removeListener(al);
         });
    },
    // 오버플로(백프레셔) 처리, 기본값은 BUFFER
    FluxSink.OverflowStrategy.LATEST)
    .timeout(Duration.ofSeconds(3))
    .doOnComplete(() -> System.out.println("completed!"))
    .subscribe(System.out::println);
```

## 백프레셔란?

이러한 기능 대부분은 프로듀서와 컨슈머 간의 처리 속도 차이를 완화하기 위해 내부적으로 제한된 링 버퍼 구현을 사용합니다. 연산자와 프로세서, 기타 표준 리액티브 스트림 컴포넌트는 버퍼에 여유 공간이 있을 때만 데이터를 흘려보내도록 지시받습니다. 즉, 결정적인 용량 모델(제한된 버퍼)을 유지하면서도 쓰기 용량을 초과해서 데이터를 요청하지 않아 블로킹이 발생하지 않습니다. 지루한 부분은 저희와 [Reactive Streams Commons](https://github.com/reactor/reactive-streams-commons)가 함께 연구하며 처리하고 있으니 걱정하지 마세요.

## 이 외의 내용

"연산자 퓨전"(흐름 최적화), 상태 관찰 도구, 사용자 정의 리액티브 컴포넌트를 만들기 위한 도우미, 제한된 큐 생성기, Java 9 Flow/Publisher와 Java 8 CompletableFuture 간 변환기가 제공됩니다. 저장소에는 [`StepVerifier`](https://projectreactor.io/docs/test/release/api/index.html?reactor/test/StepVerifier.html)와 같은 테스트 기능을 제공하는 `reactor-test` 프로젝트도 포함되어 있습니다.

-------------------------------------

## 레퍼런스 가이드
https://projectreactor.io/docs/core/release/reference/docs/index.html

## Javadoc
https://projectreactor.io/docs/core/release/api/

## Flux와 Mono 시작하기
https://github.com/reactor/lite-rx-api-hands-on

## Reactor By Example
https://www.infoq.com/articles/reactor-by-example

## Head-First Spring & Reactor
https://github.com/reactor/head-first-reactive-with-spring-and-reactor/

## Reactor Core 그다음 단계
- JVM 밖으로 확장할 때는 [Reactor Netty](https://github.com/reactor/reactor-netty)의 논블로킹 드라이버를 활용하세요.
- [Reactor Addons](https://github.com/reactor/reactor-addons)는 Reactor 3용 어댑터와 추가 연산자를 제공합니다.

-------------------------------------
_Powered by [Reactive Streams Commons](https://github.com/reactor/reactive-streams-commons)_

_Licensed under [Apache Software License 2.0](https://www.apache.org/licenses/LICENSE-2.0)_

_Sponsored by [VMware](https://tanzu.vmware.com/)_
