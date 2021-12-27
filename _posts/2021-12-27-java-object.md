---
title: "[Java] 자바 객체지향 관련 용어 정리"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-12-27T21:34:30+09:00
categories:
  - Java
tags:
  - Java
permalink: /java/object-terms/
---
---

Java를 처음 공부해보면서 객체 관련 내용들을 용어 중심으로 정리해 보았습니다.
<!--more-->

## 클래스(Class)
클래스는 객체를 만들기 위한 청사진입니다.

객체를 생성하기 위해서는 클래스를 만들고 클래스의 생성자를 호출해야합니다.

```java
public class Animal {
    String name;
    private int age;
    
    public void setNameAge(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public int getAge() {
        return age; 
    }
}
```
하나의 .java 파일에는 같은 이름의 public class 하나만이 정의될 수 있습니다.

프로그램은 main 메서드로부터 시작됩니다.

## 상속
class를 선언할 때에 extends 를 사용하여 다른 클래스를 상속 받을 수 있습니다.

상속을 하면 부모 클래스의 모든 변수와 메서드를 그대로 가져옵니다. (static 제외)

```java
public class Dog extends Animal {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.setNameAge("James", 2);
        System.out.println(dog.name);
        System.out.println(dog.getAge());
    }
}
```
하나의 클래스는 하나의 클래스만 상속 받을 수 있습니다. (복잡해지지 않도록)

부모 클래스에서 private으로 선언된 요소들도 상속이 되지만 접근은 불가능합니다.

## 접근 제어자
외부에서 접근을 얼마나 허용할 지를 접근 제어자를 통해 제어합니다.

![Access Modifier](https://t1.daumcdn.net/cfile/blog/99508B3D5E54AD1536)

{% include ad-contents.html %}

## 인터페이스(Interface)
인터페이스는 클래스와 비슷하나, 메서드를 선언만 하고 구현은 인터페이스를 사용한 클래스에서 직접 하도록 강제합니다.

인터페이스는 implements 로 사용할 수 있습니다.

```java
public interface Predator {
    public String getFood();
}
```

인터페이스에 변수를 선언할 수 있지만, final로 선언되기 때문에 초기화를 해주어야 합니다.

하나의 클래스가 여러 개의 인터페이스를 사용할 수 있습니다.

여러 개의 인터패이스를 상속 받을 수 있습니다.

## 추상 클래스(Abstract Class)

추상 클래스는 클래스와 인터페이스의 기능을 모두 가지고 있습니다.

```java
public abstract class Predator extends Animal {
    public abstract String getFood();

    public boolean isPredator() {
        return true;
    }
}
```

abstract 키워드를 사용하며, 선언만 해 놓을 메서드에도 abstract를 붙입니다.

추상 클래스와 인터페이스 중에서 어떤 것을 사용할지 잘 생각해 보아야 합니다.

## 다형성
객체를 선언할 때에 여러 타입들(클래스, 부모 클래스, 인터페이스)에서 하나를 선택하여야 합니다.

어떤 것을 선택하든 인스턴스는 잘 생성됩니다.

```java
// 둘 다 가능!
Dog dog = new Dog();
Animal dog = new Dog();
```

그러나 메서드를 호출하기 위해서는 메서드가 선언되어 있다고 보장되는 타입을 선택해야 합니다.

(Dog에 선언되어 있는 메서드를 호출하기 위해서는 Animal이 아니라 Dog로 선언해야 합니다)

## Reference
* [점프 투 자바](hhttps://wikidocs.net/book/31){:target="_blank"}
