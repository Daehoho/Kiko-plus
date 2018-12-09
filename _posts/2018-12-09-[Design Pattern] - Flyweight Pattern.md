---
layout: post
title: "[Design Pattern] - Flyweight Pattern"
description: "Flyweight Pattern"
date: 2018-12-09
tags: [Design Pattern, Flyweight Pattern, String Pool]
comments: false
share: true
---

지난번 [String Pool](https://daehoho.github.io/2018-12-03/JAVA-String-Pool/) 을 살펴 보면서 String Pool이 디자인 패턴적으로 Flyweight Pattern 이라고 하였습니다. 그러면 한번 이러한 Flyweight Pattern에 대해서 알아보도록 하겠습니다.

---

### Flyweight Pattern?
  우선 Flyweight Pattern은 Facade Pattern, Adapter Patter과 이 디자인 패턴에 대해서 아래와 같이 설명하고 있습니다.

 > 동일 하거나 유사한 객체들 사이에 가능한 많은 데이터를 서로 공유하여 메모리 사용량을 최소화 하는 패턴

  조금 더 