---
layout: post
title: "[Java] - String 객체가 Immutable, final인 이유"
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

## String이 Immutable, final인 이유

String이 Immutable하고 final로써 사용하는 이유는 아래와 같은 이점이 있기 때문입니다.

### 1. String Pool
 Spring Pool을 통해 다른 String 변수들이 pool 내부의 같은 문자열 값을 참조하면서 Java Runtime은 많은 heap 공간을 절약 할 수 있습니다. 하지만 이렇게 같은 값을 참조하여 공유하기 위해서는 String 클래스가 Immutable 해야만 했습니다.

 이는 Java를 처음 설계 할때 가장 많이 쓰이는 데이터 타입이 String이 될 것이라고 예측하였고, 이에 따라 최적화를 진행하였던 결과입니다.

### 2. Security
 String은 다수의 Java 클래스에서 매개 변수로 널리 사용되고 있으며, 특히 DB, 네트워크 연결 시 중요 정보가 String으로 전달합니다. 그런데 만약 String이 Immutable 하지 않고 final 클래스가 아니라면 심각한 보안 문제를 일으키게 될 것 입니다.

### 3. Thread Safe
 String 객체가 Immutable 함으로써 외부에서 동기화 필요를 없애고 여러 스레드 간에 String을 공유하는 부분이 간단해집니다. 

### 4. Class Loading Mechanism
 Class Loading Mechanism이란 동적으로 클래스를 로딩해주는 방법을 의미합니다. 이에 대해선 나중에 다시 자세히 알아보겠습니다. String은 이러한 Class Loader에서도 매우 자주 사용 됩니다. 하지만 String 객체는 Immutable 하기 때문에 지정한 올바른 클래스들을 로드 할 수 있게 됩니다.

### 5.최적화 및 성능
  클래스가 Immutable 하면 갖는 특징은 클래스가 일단 생성되면 변경이 불가능 하다는 점인데 이로인해 성능 최적화가 가능합니다.  
  예를들어 String의 hash 값에 대해 얘기해보자면 hash를 생성하는 작업은 자원이 많이 드는 작업 입니다.

  그렇기 때문에 String의 경우 객체가 생성될 때 hash값을 생성하지 않고 hashCode 메소드가 실행될때 생성하게 됩니다. String 객체의 경우 Immutable하기 때문에 생성될때 hash값을 생성하지 않고 나중에 생성하더라도 값이 변하지 않기 때문에 hash값을 보장 할수가 있게 됩니다.
  
  또한 String pool에 의해 String 객체가 caching 되기 때문에 한번 생성한 hash값 또한 재 사용 할 수 있게 됩니다.

  아래의 String 클래스의 hashCode() 메소드를 살펴보면 생성된 해쉬코드를 확인하는 코드가 있습니다.

  ```java
      public int hashCode() {
        int h = hash;
        // hash의 초기값이 0 즉, 생성된 해쉬코드가 없으면 생성한다.
        if (h == 0 && value.length > 0) {
            hash = h = isLatin1() ? StringLatin1.hashCode(value)
                                  : StringUTF16.hashCode(value);
        }
        return h;
    }
  ```