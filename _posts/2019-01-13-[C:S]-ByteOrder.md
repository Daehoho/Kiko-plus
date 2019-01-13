---
layout: post
title: "[C/S] - Byte Order"
description: "C/S"
date: 2019-01-13
tags: [ByteOrder]
comments: false
share: true
---

---
빅 엔디안(Big-endian)과 리틀 엔디안(Little-endian)은 컴퓨터 메모리에 저장된 바이트 오더(Byte Order)를 설명하는 용어이다.

빅 엔디안(Big-endian)은 주로 UNIX 시스템인 RISC 프로세서 계열에서 사용한다. 메모리에 데이터를 저장할때 낮은 주소에서 높은 주소로 상위 byte부터 하위 byte까지 저장하는 방식이다.

![빅 엔디언(위키피디아 출처)](https://daehoho.github.io/images/big-endian.png){: .center-image}

리틀 엔디안(Little-endian)은 인텔(Intel) 프로세스 계열에서 사용하는 바이트 오더 이며 낮은 주소에서 높은주소로 하위 byte부터 상위 byte까지 저장하는 방식이다.

![리틀 엔디언(위키피디아 출처)](https://daehoho.github.io/images/little-endian.png){: .center-image}

두 방법 중 어느 한 쪽이 다른 쪽과 비교해 압도적으로 좋거나 나쁘지는 않다고 알려져 있다. 


|종류|0x1234의 표현|0x12345678의 표현|
|:---|:---|:---|
|빅 엔디언|[12 34]|[12 34 56 78]|
|리틀 엔디언|[34 12]|[78 56 34 12]|


장단점

빅 엔디언은 소프트웨어 디버그를 편하게 해줄 수 있다. 사람이 숫자를 읽고 쓰는 방법과 같기 때문이다. 반대로 리틀 엔디어는 메모리에 저장된 값의 하위 바이트들만 사용할때 별도의 계싼이 필요 없다는 장점이 있다. 예를 들어, 32비트 숫자인 0x2A는 리틀 엔디언으로 표현하면 [2A 00 00 00]이 되는데, 이 표현에서 앞의 두 바이트 또는 한 바이트만 떼어내면 하위 16비트 또는 8비트를 바로 얻을 수 있다.

