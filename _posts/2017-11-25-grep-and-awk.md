---
layout: post
title: "2017/11/25 grep 과 awk 활용"
---

# grep 과 awk

운영하고 있는 서버의 로그를 볼 필요가 있을 때, 보통 grep 으로 로그 파일에서 필요한 정보를 검색하곤 한다. 그러나 grep으로 원하는 정보를 찾기가 힘들 경우가 있는데, 그럴때는 grep과 awk 조합을 이용하면 좋다.
grep과 awk를 조합해서 필요한 정보를 검색하는 방법에 대해서 알아보자.

## grep
grep 은 파일의 라인에서 주어진 패턴을 검색하는 명령어이다. 해당 패턴과 일치되는 라인이 있다면 표준 출력으로 출력한다. 
입력되는 라인의 길이는 메모리가 허용하는 한도내에서 제한이 없다.

* 기본 사용법
```
grep options pattern input_file_names
```

## awk
awk 는 패턴 검색 및 처리 스크립트 언어이다. 주로 필드 단위로 패턴을 검색하고 조작한다. 주어진 패턴과 라인이 매치된다면 awk는 해당 라인에 대해 정의된 행동을 수행한다.

* 기본 사용법
```
awk `program` input_file_names
```

* 프로그램이 길다면 다음과 같이 이용하기도 한다

```
awk -f program_file input_file_names
```

## grep 과 awk 조합하기

