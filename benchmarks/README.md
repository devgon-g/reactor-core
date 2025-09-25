## JMH 작업 사용 방법

특정 벤치마크만 실행합니다(와일드카드는 앞뒤에 자동으로 추가됩니다).
```
../gradlew jmh --include="(BenchmarkPrimary|OtherBench)"
```
와일드카드를 직접 지정하려면 전체 정규식을 전달하세요.
```
../gradlew jmh --fullInclude=.*MyBenchmark.*
```

추가 프로파일러를 지정합니다.
```
../gradlew jmh --profilers="gc,stack"
```

주요 프로파일러(전체 목록은 `jmhProfilers` 작업을 실행하세요).
- comp - JIT 컴파일 횟수, 반복 횟수 조정에 유용
- stack - 가장 많은 시간을 사용하는 메서드
- gc - 가비지 컬렉션 통계 출력
- hs_thr - 스레드 사용량

리포트 형식을 JSON에서 [CSV, JSON, NONE, SCSV, TEXT] 중 하나로 변경합니다.
```
./gradlew jmh --format=csv
```

JVM 인자를 지정합니다.
```
../gradlew jmh --jvmArgs="-Dtest.cluster=local"
```

검증 모드로 실행합니다(최소한의 fork/워밍업/측정 반복으로 벤치마크 실행).
```
../gradlew jmh --verify=true
```

## 기준선과 비교하기
현재 변경분과 "기준선"을 각각 한 번씩 실행하려면 최신 릴리스를 사용하는 `jmhBaseline` 작업을 추가로 실행하세요.
```
../gradlew jmh jmhBaseline --include=MyBenchmark
```

## 참고 자료
- http://tutorials.jenkov.com/java-performance/jmh.html (입문용)
- https://github.com/openjdk/jmh/tree/master/jmh-samples/src/main/java/org/openjdk/jmh/samples (샘플)
