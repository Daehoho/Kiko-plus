---
layout: post
title: "[Java] - Immutable 클래스 만들어보기"
description: "Java Basic"
date: 2018-12-15
tags: [Java, Immutable, String Pool, String]
comments: false
share: true
---

String이 Immutable, final인 이유에 대해서 알아보면서 Immutable의 장점에 대해서 알아보았습니다. 그렇다면 이제 Immutable 클래스를 만들어보겠습니다.
다시 한번 정리해보자면 Immutable 클래스는 일단 인스턴스화 되면 그 값은 절대로 변하지 않는 클래스 입니다. 
그렇기 때문에 값이 바뀌는 것에 대해 걱정할 필요가 없어 캐싱 목적으로 유용합니다. 또한 thread-safe 합니다.

---

## Immutable 클래스의 조건

Immutable 클래스를 만들기 위한 몇가지 규칙이 있습니다. 

(1) 해당 클래스가 상속될 수 없도록 final로 선언한다.
(2) 모든 필드들을 private으로 만들어 직접 접근할 수 없도록 한다.
(3) 변수에 대해 setter 메소드를 제공하지 않는다.
(4) 필드들을 final로 선언하여 최초에 한번만 할당할 수 있도록 한다.
(5) 필드들을 초기화 할때 깊은 복사를 수행하도록한다.
(6) getter 메소드안에서 cloning을 통해 객체 참조값이 아닌 복사본을 반환하도록 한다.
   
위와 같은 규칙을 통하여 Immutable을 객체를 생성할 수 있습니다. 일단은 그냥 읽어보고 코드를 통해 각각을 구현하고 그에 대한 이유를 얘기해보도록하겠습니다.

```java
// (1) 클래스가 상속될 수 없도록 final로 선언
public final class MyImmutable {
    //(2) 모든 필드들을 private으로 선언
    private int id;
    private String name;
    private HashMap<String, String> myMap;

    //(4) 필드들을 final로 설정하여 생성자를 통해서 한번만 할당되도록 선언
    public MyImmutable (String id, String name, HashMap<String, String> myMap) {
        this.id = id;
        this.name = name;

        //(5) 깊은 복사를 통해 필드 초기화
        HashMap<String,String> tempMap = new HashMap<>();
        for(String key : myMap.keySet()) {
            tempMap.put(key, myMap.get(key);
        }
        this.myMap = tempMap;
    }

    public int getId() {
        return this.id;
    }

    public int getName() {
        return this.name;
    }
    //(6) 객체 참조값을 반환하지 않고 clonning을 통해 복사본 반환
    public HashMap<String, String> getMyMap() {
        return (HashMap<String, String>)myMap.clone();
    }

    //(3) setter 메소드를 제공하지 않는다.
}
```

코드를 통해 위의 규칙들이 어떻게 구현되는지 확인 할 수 있습니다. 이제 코드와 함께 보면서 각각 규칙들에 대해서 설명해 보겠습니다.

(1) 해당 클래스가 상속될 수 없도록 final로 선언한다.
    > 상속할 수 없도록 만드는 이유는 무분별한 상속을 막아서 위험이 되는 요소를 미리 차단하는 이유가 있습니다.

(2) 모든 필드들을 private으로 만들어 직접 접근할 수 없도록 한다.
    > 위에서 말했듯이 Immutable은 인스턴스화 될 때 할당된 값이 변하지 않는 것이므로 당연히 필드들이 직접 접근하여 값이 수정되는 일이 없어야 합니다.

(3) 변수에 대해 setter 메소드를 제공하지 않는다.
    > (2)와 같은 이유입니다.

(4) 필드들을 final로 선언하여 최초에 한번만 할당할 수 있도록 한다.
    > 이것 또한 (2)에서 말한것처럼 최초에 한번 할당된 값이 변하지 않아야 하므로 값이 변할 수 있는 요소를 애초에 차단하는 역할을 합니다.

(5) 필드들을 초기화 할때 깊은 복사를 수행하도록한다.
(6) getter 메소드안에서 cloning을 통해 객체 참조값이 아닌 복사본을 반환하도록 한다.
    > (5), (6) 에 대해서는 좀 헷갈릴 수 도 있는데 이에 대해서는 테스트 코드를 작성하면서 한번 살펴보겠습니다.
    
    우선 위에서 작성한 코드를 통해 테스트를 진행 해보겠습니다. Immutable 클래스는 생성할때 초기화된 값에서 변경이 일어나지 않아야하는 클래스입니다. 그러므로 그에 맞게 테스트 코드를 작성해보겠습니다.

```java
    @Test
    public void givenMyImmutable_whenModifyValue_thenSuccess() {
        int id = 10;
        String name = "original";
        HashMap<String, String> map = new HashMap<>();
        map.put("1", "first");
        map.put("2", "second");

        MyImmutable myImmutable  = new MyImmutable(id, name, map);

        int initalMapSize = myImmutable.getMyMap().size();

        map.put("3", "third");

        int afterModifyMapSize = myImmutable.getMyMap().size();

        assertThat(initalMapSize, equalTo(afterModifyMapSize));
    }

    @Test
    public void givenMyImmutable_whenModifyGetterValue_thenSuccess() {
        int id = 10;
        String name = "original";
        HashMap<String, String> map = new HashMap<>();
        map.put("1", "first");
        map.put("2", "second");

        MyImmutable myImmutable  = new MyImmutable(id, name, map);

        int initalMapSize = myImmutable.getMyMap().size();

        map = myImmutable.getMyMap();
        map.put("3", "third");

        int afterModifyMapSize = myImmutable.getMyMap().size();

        assertThat(initalMapSize, equalTo(afterModifyMapSize));
    }
```
위의 두개의 테스트 메소드들은 정상적으로 테스트를 통과하게 됩니다. (5)의 규칙을 통해서 생성자를 통해 깊은 복사를 해주었기때문에 첫번째 테스트 메소드에서 생성한 map과의 연결이 끊어지게 됩니다.
또한 (6)의 규칙에 대한 테스트 코드는 두번째 메소드에 해당하는데 getMyMap() 메소드로 가져온 map의 경우도 clone() 메소드를 통해 참조 값이 아니라 객체를 복사해서 넘겨주기 때문에 myImmutable 객체의 myMap 필드의 값이 변하지 않게 됩니다. 

혹시나 이 과정이 이해가 가지 않는다면 (5),(6) 규칙을 적용하지 않은 아래 코드를 보면서 비교해보면 됩니다.

```java
public class SwallowCopyImmutable {
    private final int id;
    private final String name;
    private final HashMap<String, String> myMap;

    public SwallowCopyImmutable(int id, String name, HashMap<String, String> myMap) {
        this.id = id;
        this.name = name;
        this.myMap = myMap;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public HashMap<String, String> getMyMap() {
        return (HashMap<String, String>)myMap.clone();
    }
}

  @Test
    public void givenSwallowCopyImmutable_whenModifyValue_thenFail() {
        int id = 10;
        String name = "original";
        HashMap<String, String> map = new HashMap<>();
        map.put("1", "first");
        map.put("2", "second");

        SwallowCopyImmutable swallowCopyImmutable  = new SwallowCopyImmutable(id, name, map);

        int initalMapSize = swallowCopyImmutable.getMyMap().size();

        map.put("3", "third");

        int afterModifyMapSize = swallowCopyImmutable.getMyMap().size();

        assertThat(initalMapSize, equalTo(afterModifyMapSize));
    }
    
    @Test
    public void givenSwallowCopyImmutable_whenModifyGetterValue_thenFail() {
        int id = 10;
        String name = "original";
        HashMap<String, String> map = new HashMap<>();
        map.put("1", "first");
        map.put("2", "second");

        SwallowCopyImmutable swallowCopyImmutable  = new SwallowCopyImmutable(id, name, map);

        int initalMapSize = swallowCopyImmutable.getMyMap().size();

        map = swallowCopyImmutable.getMyMap();
        map.put("3", "third");

        int afterModifyMapSize = swallowCopyImmutable.getMyMap().size();

        assertThat(initalMapSize, equalTo(afterModifyMapSize));
    }
```

위의 테스트 코드 같은 경우는 테스트를 통과하지 못하게 되는데 생성자의 얕은 복사로 인해 테스트 코드에서 생성한 map의 참조값으로 연결되어 있어 SwallowCopyImmutable객체의 myMap의 값이 변경되게 됩니다.

또한 getMyMap()의 경우도 참조값을 넘겨주므로 외부에서 값을 변경하게 되면 변경되게 됩니다.

Immutable클래스를 직접 만들어보면서 얕은,깊은 복사등의 개념도 다시 한번 살펴보았습니다.