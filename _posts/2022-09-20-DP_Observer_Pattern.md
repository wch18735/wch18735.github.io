---
title: "[Design Pattern] Observer Pattern (관찰자 패턴)"
excerpt: "observer pattern"
date: 2022-09-19
layout: single
classes: wide
category:
    - design pattern
tag:
    - software engineering
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
header:
    teaser: "/_img/thumb-nail/software-engineering.png"
---

# Observer Pattern

Observer Pattern은 Publish/Subscribe model로도 불리는 패턴으로, 이 관찰자 패턴은 한 개 또는 여러 객체들의 상태 변화를 감지해 다른 객체들에게 전달하기 위해 사용된다. 아래는 짤막한 정리와 관찰자 패턴을 정의한 내용을 정의했다.

- Purpose
    - Lets one or more objects be notified of state changes in other objects within the system
- Use When
    - Loose coupling is needed for communications
    - State changes in one or more objects should trigger behavior in other objects
    - Broadcasting capabilities are required
    - An understanding exists that objects will be blind to the expense of notification


**※ The Observer Pattern Defined**
> The Observer Pattern defines a one-to-many dependency between objects so that when one object change state, all of its dependents are notified and updated automatically

## Problem

관찰자 패턴을 적용해보기 위한 문제 상황을 아래 그림과 같이 가정해보자. 세 개의 센서로부터 측정되는 값을 표시하기 위해 Weather Station과 WeatherData Object가 있다고 가정해보자. WeatherData Object 의 클래스 다이어그램은 우측 상단에 표시되어 있으며 각 세 개의 메소드는 가장 최근 측정된 날씨 정보를 반환한다.

<img src="/_img/2022-09-20/application overview.png">

WeatherData는 세 개의 센서를 가지고 있고, 이들 값이 전달되어 변화를 감지하면 Display Device는 이를 알아차리고 Display Device가 가진 각 모드들에게 정보를 전달하는 것이 요구사항이라고 생각해보자. 만약 WeatherData를 이용하는 Display Device 모드가 current condition, statics, forecast 세 가지가 있을 때, 간단하게 WeatherData를 구현할 수 있다.

```java
public class WeatherData{

    // variables

    public void measurementsChagned(){
        float temp = getTemperature();
        float humidity = getHumidity();
        float pressure = getPressure();

        currentConditionDisplay.update(temp, humidity, pressure);
        statisticsDisplay.update(temp, humidity, pressure);
        forecastDisplay.update(temp, humidity, pressure);
    }

    // other weatherData methods here
}
```

`void measurementsChanged()` 메소드 안에서 각 디바이스 모드 정보를 변경해주는 작업을 수행하고 있다. 그러나 이 부분은 코드는 디바이스 모드가 추가되거나 매개변수 종류가 변할 때마다 함께 수정되어야 하는 번거로움을 가진다. 이렇게 변화를 감지하고 변경된 값을 전달하는 효율적인 구조로 Observer Pattern을 사용할 수 있다.

## Compare to Publishser/Subscriber Model

앞서 Observer Pattern이 어떤 상황에서 사용되는지를 확인했다. 이와 같은 Observer Pattern을 Publisher/Subscriber Model과 비교하면 **Publisher:Subscriber = Subject:Observer**로 나타낼 수 있다. Observer들은 Subject를 구독하거나 구독을 끊을 수 있으며, 해당 Subject의 변경이 일어나면 Subject는 변경된 정보를 전달해준다.

## Sturcture of observer pattern

관찰자 패턴 클래스 다이어그램은 아래와 같다. 여기서 중요한 것은 ConcreteSubject는 ConcreteObserver 정보를 모른다는 것이다. 가지고 있는 정보는 오직 Observer Interface 뿐이기 때문에 다양한 ConcreteObserver들을 사용 할 수 있다. 이러한 패턴은 Model은 View 정보를 철저히 모른 상태로 데이터를 전달해야하는 MVC 패턴에서도 사용된다.

<img src="/_img/2022-09-20/observer pattern class diagram.png">

이러한 패턴 구조를 앞선 문제에 적용해보면 아래와 같이 나타낼 수 있다. Observer들은 **WeatherData::registerObserver()** 를 통해 등록되어 notifyObserver()를 통해 각각의 update()를 호출 할 수 있게 한다.

<img src="/_img/2022-09-20/weather station design.png">

## The Power of Loose Coupling

- Principle of loose coupling
    - Strive for loosely coupled designs between object that interact
- When two objects are loosely coupled, they can interact, but have little knowledge of each other
- Observer Pattern provides an object design where subjects and observers are loosely coupled

## Observer pattern in Java

Java에는 Observer Interface를 지원한다. Observable을 상속해 사용하면 이를 앞서 구현했던 등록, 삭제, 알림 등을 구현하지 않고 사용할 수 있다.

<img src="/_img/2022-09-20/java observable class.png">

- Observable (Publisher) side
    - To send notification
        - Call setChanged()
        - Call either notifyObserver() or notifyObserver(Object arg)

- Observer (Subscriber) side
    - To become an observer
        - Implement java.util.Observer interface
    - To subscribe
        - Call addObserver() on any Observable object
    - To unsubscribe
        - Call deleteObserver()
    - To get updated
        - Implement update(Observable obs, Object arg)

### setChanged()

```java
setChanged() {
    changed = true;
}

notifyObserver(Object arg){
    if(changed){
        for every observer on the list {
            call update(this, arg)
        }
        changed = false;
    }
}

notifyObservers() {
    notifyObservers(null);
}
```

- setChanged() method is there to give you more flexibility
    - Optimize the notifications
    - Example:
        - If the sensor reading is very sensitive, you may not want to update
        - setChanged() allows you to control the notification points

- Other relevant methods in Observable
    - clearChanged()
    - hasChanged()