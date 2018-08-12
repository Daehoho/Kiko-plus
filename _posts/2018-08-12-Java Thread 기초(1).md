---
layout: post
title: "[Java Basic] - Thread(1)"
description: "Java Baisc Thread  "
date: 2018-08-12
tags: [Java, Thread]
comments: false
share: true
---

Netty 소스 분석을 하면서 Thread에 대한 기초 지식이 부족함을 느껴서 다시 Java 기초부터 다시 다지기로 결심하였다.

## 1. Thread vs Process
보통 쓰레드를 공부하게 되면 프로세스와 무엇이 다른지 헷갈리게 됩니다. 
다음의 블로그 글들은 메모리관점에서 프로세스와 쓰레드를 설명하고 있으며 저 스스로는 쓰레드와 프로세스의 차이점에 대해서 이해하는데 도움을 받을 수 있었습니다. 
https://mooneegee.blogspot.com/2015/01/os-process.html
https://mooneegee.blogspot.com/2015/01/os-thread.html

## 2. Java 에서 Thread 생성 방법
Java에서 Thread를 생성하는 방법은 두가지가 있습니다. Thread 클래스를 이용하는 방법과 Runnable 인터페이스를 사용하는 방법이 있습니다. Thread 클래스 또한 Runnable 인터페이스를 구현한 클래스 입니다. 근데 이렇게 두가지 방법을 제공하는 이유는 Java는 다중 상속을 지원하지 않으므로 이미 다른 클래스를 상속받은 클래스에서 Thread를 생성하기 위해서는 Runnable 인터페이스를 구현하여 생성하면 됩니다.

- Thread 클래스 사용
```java
public class CustomThread extends Thread {
    @Override
    public void run() {
        System.out.println("Extended Thread Class");
    }
}
```

- Runnable 인터페이스 사용
```java
public class CustomRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Implemented Runnable Interface");
    }
}
```

- Thread 생성
```java
public class Main {
    public static void main(String[] args) {
        Main main = new Main();

        main.useRunnable();
        main.useThread();
    }
    
    public void useRunnable() {
        CustomRunnable runnable = new CustomRunnable();
        new Thread(runnable).start();
    }

    public void useThread() {
        CustomThread thread = new CustomThread();
        thread.start()
    }
}
```

위와 같은 코드를 실행하게 되면 쓰레드가 생성되고 실행되게 됩니다.
> Thread가 시작되면 수행되는 메소드는 run() 메소드입니다.  
> Thread를 시작하는 메소드는 start()입니다.  

Thread를 생성하여 실행되는 코드는 순서를 보장하지 않습니다. 예를 들어 위의 코드에서 보면 Runnable 인터페이스를 이용한 코드가 먼저 실행될것 같지만 그렇지 않을 수도 있습니다. 
그 이유는 start메소드의 경우 실행되면 jvm은 start 메소드가 끝날때까지 기다리는것이 아니라 다음 줄의 start메소드를 실행하게 됩니다. 그러므로 순서를 보장하지 않습니다.