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
  우선 Flyweight Pattern은 Facade Pattern, Adapter Pattern과 같이 구조적 디자인 패턴입니다. 패턴에 대한 설명은 아래와 같습니다.

- 패턴의 목적
  > 많은 수의 객체를 효율적으로 제공하기 위해 공유 개념을 사용

- 패턴의 등장 배경
  > 동일한 클래스에 대해 많은 객체를 생성해야 할 때 메모리 부하를 줄일 수 있기 위해 등장

- 사용 조건
  > 어플리케이션이 정말 많은 객체를 사용할 때
  > 저장비용이 높을 때

정리 하자면 Spring Pool에서 살펴 봤듯이 이미 생성된 문자열을 재사용 함으로써 메모리 공간을 절약할 수 있듯이 많은 객체를 생성해야할 때 해당하는 객체가 존재하면 그 객체를 가져다 쓰고 없으면 생성하는 패턴이라고 할 수 있습니다.
 
### 구현
구현 방법에 따라 interface를 정의하는 등등 좀 더 우아하게(?) 작성 할 수 있지만 우선 개념을 이해를 위해 간단하게 작성 해보았습니다. 우선 아래의 코드를 보도록 하죠.


```java 
public class Person {
    private String name;

    public Person(String data) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}

public class PersonFactory {
    Map<String, Person> pool;

    public PersonFactory() {
        pool = new TreeMap<>();
    }

    public getPerson(String name) {
        Person person = pool.get(name);

        if(person == null)  {
            person = new Person(name);
            pool.put(name, person);
        } 

        return person;
    }
}
```
코드에서 보는것 처럼 Person 클래스와 PersonFactory 클래스가 존재합니다.

PersonFactory의 getPerson 메소드를 보면 name으로 pool에서 조회하여 존재하면 해당 객체를 반환하고 존재하지 않으면 Person 객체를 생성하고 pool에 담고 반환해줍니다.

매우 간단하게 구현하였지만 이러한 방법을 통해 메모리 사용량을 줄일 수 있으며 생각보다 많은 곳에서 이러한 패턴이 쓰이고 있습니다. 

특히 Java에서 primitive 타입을 박싱 해주는 Number 객체들의 valueOf 메소드도 이러한 방법을 사용하고 있는데 잠깐 확인해보도록 합시다.

```java
    public static Integer valueOf(int i) {
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }

        private static class IntegerCache {
        static final int low = -128;
        static final int high;
        static final Integer cache[];

        static {
            // high value may be configured by property
            int h = 127;
            String integerCacheHighPropValue =
                sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
            if (integerCacheHighPropValue != null) {
                try {
                    int i = parseInt(integerCacheHighPropValue);
                    i = Math.max(i, 127);
                    // Maximum array size is Integer.MAX_VALUE
                    h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
                } catch( NumberFormatException nfe) {
                    // If the property cannot be parsed into an int, ignore it.
                }
            }
            high = h;

            cache = new Integer[(high - low) + 1];
            int j = low;
            for(int k = 0; k < cache.length; k++)
                cache[k] = new Integer(j++);

            // range [-128, 127] must be interned (JLS7 5.1.7)
            assert IntegerCache.high >= 127;
        }

        private IntegerCache() {}
    }
```

위의 코드는 Integer 클래스와 그의 내부클래스인 IntegerCache 클래스의 소스입니다.

IntegerCache는 static으로 선언되어 먼저 메모리에 생성되어 있을 것이고 이를 통해 특정 범위 안에 있는 값들은 캐싱 해놓고 사용하는 방법을 사용하고 있습니다.

좀 더 세부적인 내용도 있고 멀티스레딩에 대응 할 수 있는 코드도 작성 해보고 싶지만 일단 오늘은 여기 까지 하고 다음 번에 더 자세하게 작성하여 올리도록 하겠습니다.