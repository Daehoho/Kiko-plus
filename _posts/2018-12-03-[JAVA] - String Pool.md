---
layout: post
title: "[Java] - String Pool"
description: "Java Basic"
date: 2018-12-03
tags: [Java, Immutable, String Pool]
comments: false
share: true
---

Java를 통해 개발 하다보면 가장 많이 쓰는 객체 중 하나가 String 입니다. 하지만 정작 String에 대해서 잘 모르고 사용하는 것 같아 이리저리 찾아보니 생각보다 더 많이 모르고 사용하고 있어 창피함을 느끼면서 이 글을 작성합니다.

---

## String Pool 이란?
```java

	String str1 = "String";
	String str2 = "String";

	System.out.println("str1 == str2 : " + (str1 == str2));
```
위의 코드의 결과에 대해서 추측해보도록 합시다. 위의 코드의 결과와 이유에 대해서 설명하실 수 있는 분이라면 이 글을 보실 필요가 없습니다. 우선 답부터 말씀드리면 "true" 입니다. 
틀리신 분들은 아마 String은 클래스이므로 참조타입이고 각각의 변수가 참조값을 가지고 있으므로 결과가 "false"로 나올 것으로 생각하셨을 것으로 예상됩니다. 물론 저도 알지 못했기 때문에 이 글을 작성하게 되었습니다.

위에서 말한대로 String은 클래스이고 참조타입이 맞습니다. 하지만 String 클래스는 좀 특별한 참조 자료형입니다. new 생성자를 이용해 객체를 생성할 수 있고 위의 예제처럼 리터럴 형식으로 객체를 생성 할 수 있습니다. 하지만 그 두 방법은 방법 뿐만 아니라 객체가 생성되는 메모리 영역에 있어서도 차이점을 보이게 됩니다.

이에 대해서 자세히 알기 위해서는 String Pool이라는 것에 대해 알아야 합니다. String Pool 이란 Java Heap Memory 안에 문자열들을 보관해놓는 저장소입니다. 

![String Pool](https://daehoho.github.io/images/java/string_pool.PNG){: .center-image}
String 객체를 선언하게 되면 위의 그림과 같이 메모리 영역내에 생성되게 됩니다. 

리터럴 형식으로 String 객체를 생성하게되면 String Pool안에 같은 문자열을 가지고 있는지 확인하고 존재하면 해당 참조값을 참조하게 됩니다. 

하지만 new 연산자를 이용하여 String 객체를 생성하게 되면 Heap 공간에 새로운 String 객체를 생성하게 됩니다. 그리고 [intern()](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#intern()) 메소드를 사용하게 되면 해당 객체를 풀에 넣거나 동일한 값을 가진 String Pool에서 다른 String 객체를 참조 할 수 있게 됩니다.

이렇게 String Pool을 사용하게되면 String 객체를 생성하는데 시간이 좀 더 걸리긴 하지만 Java Runtime의 메모리 공간을 매우 많이 절약 할 수 있게 됩니다.

추가적으로 String Pool은 String 이 immutable 객체 이기때문에 사용 할 수 있으며 이것은 [String interning](https://en.wikipedia.org/wiki/String_interning) 개념의 구현체 라고 할수 있습니다. 또한 디자인 패턴적으로 보자면 [Flyweight Pattern](https://ko.wikipedia.org/wiki/%ED%94%8C%EB%9D%BC%EC%9D%B4%EC%9B%A8%EC%9D%B4%ED%8A%B8_%ED%8C%A8%ED%84%B4) 의 예시 중 하나가 될 수 있습니다. 관련된 자세한 내용은 다음 글에서 말씀드리겠습니다.