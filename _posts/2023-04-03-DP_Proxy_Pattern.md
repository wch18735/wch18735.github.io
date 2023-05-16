---
title: "[Design Pattern] Proxy Pattern (프록시 패턴)"
excerpt: "proxy pattern"
date: 2023-04-03
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

# Proxy Pattern

Proxy Pattern은 객체 지향 디자인 패턴 중 하나로, 객체의 중간자 역할을 하는 중간 객체(Proxy)를 사용해 **객체에 접근을 제어**하는 패턴이다. Proxy 객체는 실제 객체의 인터페이스를 구현하며, 클라이언트에서는 Proxy 객체를 통해 실제 객체에 접근한다. 클라이언트는 Proxy 객체와 실제 객체를 구분하지 않고, 인터페이스에서 제공하는 메소드를 호출해 사용한다.

Proxy Pattern은 보안, 성능, 접근 제어 등 다양한 상황에서 사용될 수 있다. 가령, 네트워크를 통해 접근하는 원격 객체의 경우, 원격 객체에 직접 접근하는 것보다 Proxy 객체를 사용해 네트워크 부하를 줄일 수 있다.

예를 들어, 이미지 로딩의 경우 이미지를 로딩할 때, 이미지가 매우 큰 경우 이미지 로딩에 시간이 매우 오래 걸리는 문제가 발생한다. Proxy Pattern을 이용해 이미지를 로딩하면 캐싱을 사용해 조금 더 빠르게 대응할 수 있다.

1. 클라이언트에서 이미지 로딩을 요청합니다.
2. Proxy 객체에서는 이미지 파일의 URL을 전송받습니다.
3. Proxy 객체는 이미지를 캐싱하고 있는지 확인합니다. 이미지가 캐시되어 있다면, 캐시된 이미지를 사용합니다.
4. 이미지가 캐시되어 있지 않은 경우, Proxy 객체는 이미지를 로딩합니다.
5. Proxy 객체는 이미지를 캐싱합니다.
6. Proxy 객체는 이미지를 클라이언트에게 전송합니다.
7. 클라이언트는 이미지를 화면에 출력합니다.

아울러, 데이터베이스 연결과 같은 부분에서도 Connection Pool이나 조회된 데이터를 Proxy가 가지고 있다면 고비용 작업 중복 수행에 따른 소프트웨어 품질 저하를 방지할 수 있다.

## Example

아래 코드에서는 ImageLoader 인터페이스를 선언하고, 이를 구현하는 RealImageLoader 클래스와 ProxyImageLoader 클래스를 만든다. RealImageLoader 클래스는 이미지를 로딩하여 화면에 출력하는 역할을 수행하며, ProxyImageLoader 클래스는 RealImageLoader 클래스를 대신하여 이미지를 로딩하는 역할을 수행한다. 

```java
// ImageLoader 인터페이스
public interface ImageLoader {
    void displayImage();
}

// RealImageLoader 클래스
public class RealImageLoader implements ImageLoader {
    private String filename;

    public RealImageLoader(String filename) {
        this.filename = filename;
        loadImageFromDisk();
    }

    private void loadImageFromDisk() {
        System.out.println("Loading image " + filename + " from disk...");
        // 디스크에서 이미지 파일을 로딩하는 코드
    }

    @Override
    public void displayImage() {
        System.out.println("Displaying image " + filename);
        // 이미지를 화면에 출력하는 코드
    }
}

// ProxyImageLoader 클래스
public class ProxyImageLoader implements ImageLoader {
    private String filename;
    private RealImageLoader imageLoader;

    public ProxyImageLoader(String filename) {
        this.filename = filename;
    }

    @Override
    public void displayImage() {
        if (imageLoader == null) {
            imageLoader = new RealImageLoader(filename);
        }
        imageLoader.displayImage();
    }
}
```

작성한 코드를 테스트하는 클라이언트에서는 ProxyImageLoader 클래스를 사용하여 이미지를 로딩하며, 이미지가 캐시되어 있다면 캐시된 이미지를 사용하여 출력한다.

```java
// 클라이언트 코드
public class Client {
    public static void main(String[] args) {
        // ProxyImageLoader를 사용하여 이미지를 로딩
        ImageLoader imageLoader = new ProxyImageLoader("image.jpg");
        // 이미지를 화면에 출력
        imageLoader.displayImage();
        // 이미지를 다시 로딩하지 않고, 캐시된 이미지를 사용하여 출력
        imageLoader.displayImage();
    }
}
```