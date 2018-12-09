---
layout: post
title: "[Java] - Java Memory"
description: "Java Basic"
date: 2018-12-03
tags: [Java, Immutable, String Pool, Java Memory]
comments: false
share: true
---


---
## Java Heap Space
Java Heap 공간은 Java Runtime에 의해 JRE 클래스들과 객체들을 메모리에 할당하기 위해서 사용됩니다. 
우리가 어떤 객체를 생성할때 항상 Heap 공간 안에 생성됩니다.

G.C(Garbage Collection)는 heap memory 위에서 더 이상 참조가 존재하지 않는 객체들이 사용한 메모리를 해제하기 위해 동작하게 됩니다. heap 공간에 생성된 객체는 global 하게 접근이 가능하며 이는 어플리케이션 내부 어디서든지 참조가 가능하다는 것을 의미합니다.

## Java Stack Memory
Java Stack 메모리는 thread의 실행을 위해 사용 됩니다. 