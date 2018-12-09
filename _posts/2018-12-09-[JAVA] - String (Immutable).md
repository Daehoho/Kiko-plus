---
layout: post
title: "[Java] - String (Immutable)"
description: "Java Basic"
date: 2018-12-09
tags: [Java, Immutable, String Pool, String]
comments: false
share: true
---

이전 글 [String Pool](https://daehoho.github.io/2018-12-03/JAVA-String-Pool/) 에서 String Pool의 간략적인 내용에 대해서 알아 보았습니다. 또한 String Pool이라는것은 String 객체가 Immutable 객체 이기 때문에 가능하다고 말씀드렸는데 이에 대해서 알아보고 왜 String 객체는 Immutable한가에 대해서 얘기해보도록 하겠습니다.

---

## Immutable?

우선 Immutable에 대해서 간단하게 알아보고 가겠습니다. 

 > * Immutable 객체 : 생성 후 변경 불가능한 객체

위에서 말했듯이 생성 후 변경이 불가능하므로 setter 메소드도 존재하지 않고 멤버 변수 또한 변경 할 수 없습니다. 대표적인 Immutable 클래스로는 String, Boolean, Integer, Float, Long등이 있습니다.

이렇게 생성 후 변경이 불가능함으로써 갖는 장점은 멀티스레드 환경에서 동기화 처리 없이 객체를 공유하여 사용할 수 있다는 점입니다. 하지만 계속해서 새로운 객체가 필요하게 됨으로써 성능 저하를 일으킬 수 있다는 단점이 있습니다.

## String객체가 Immutable임으로써 갖는 이점

### 1. String Pool
 Spring Pool을 통해 다른 String 변수들이 pool 내부의 같은 문자열 값을 참조하면서 Java Runtime은 많은 heap 공간을 절약 할 수 있습니다. 이러한 String Pool은 String이 Immutable이기 때문에 가능한 부분입니다.

 만약 String이 Immutable 하지 않다면 어떤 변수가 값을 바꾸게 되면 다른 변수의 값도 변하게 되므로 String interning이 불가능 할 것입니다.
 
### 2. Security
 String이 immutable 하지 않다면 어플리케이션에게 심각한 보안 위협이 있을 수 있습니다.

 예를 들어 DB 와 연결시 연결 정보(username, password)가 String으로 전달되게 됩니다. String은 immutable 하기 때문에 그 값을 변경할 수 없게 됩니다.

### 3. Thread Safe
 String 객체는 Immutable 하기 때문에 서로 다른 Thread에서 접근하더라도 그 값을 보장 해 줄 수 있습니다.