# 어댑터 패턴
> 호환되지 않는 인터페이스를 가진 객체들이 협업할 수 있도록 하기 위한 패턴

## 어댑터 패턴 특징
### 예시
- 예를 들어, 해외여행을 가면 우리가 가진 장비들은 220V로 충전을 해야하지만 국가 별로 지원하는 전압은 다를 수 있다.
- 이 때, 어댑터를 사용해서 호환되지 않는 것들끼리도 사용할 수 있도록 하는 것과 동일한 방식이다.   


- 동일하게 프로그램을 보면 이미 사용하고 있는 써드파티 라이브러리가 있지만 새로운 기능을 추가하기 위해 새로운 라이브러리를 적용하고 싶다고 가정해보자.
- 그냥 끼울 수 있다면 최고겠지만 그렇지 못하다면 우리는 노가다를 통해 어거지로 새로운 라이브러리 기능을 우겨넣어야한다.
- 미련하게 우겨넣지 말고 지성인답게 해결할 수 있는 방법이 바로 어댑터 패턴의 적용이다!

### 해결책
1. 어댑터는 기존에 있던 객체와 호환되는 **인터페이스**를 받는다.
2. 해당 인터페이스를 사용해서 기존 객체에서 어댑터 내에 새로운 메서드를 가져온다.
3. 호출을 수신하면 기존 객체에 어댑터가 변환해서 해당 객체에 전달해준다.

## 어댑터 패턴 종류
- 어댑터 패턴은 2가지로 나뉜다. 객체와 클래스 어댑터.
- 주요 차이는 객체를 새롭게 만드느냐 혹은 만들지 않고 상속을 활용해서 메소드를 구현하느냐이다.
### 객체
![객체 다이어그램](/img/병헌/adapter_object_diagram.png)

```java

// client 메소드
public class Client {
    public static void main(String[] args) {
        Adapter adapter = new Adapter(new Service());

        adapter.method("https://dev-qhyun.tistory.com/");
    }
}

// client와 작업하기 위해 클래스들이 따라야하는 프로토콜
public interface ClientInterface {
    void method(String data);
}

/*
 클라이언트와 서비스 양쪽에서 동작할 수 있는 코드
 새로운 메서드를 클라이언트에서 사용할 수 있도록 도와준다.
 */
public class Adapter implements ClientInterface{
    Service adaptee; // 사용하고 싶은 객체를 선언

    public Adapter(Service adaptee) {
        this.adaptee = adaptee;
    }

    @Override
    public void method(String data) {
        adaptee.specialMethod(data);  // adapter를 통해 Service 호출
    }
}

// 기존 시스템을 어댑터를 이용해 들어가려는 쪽
public class Service {
    void specialMethod(String data){
        System.out.print("남의 블로그 홍보 " + data);
    }
}

```

### 클래스
![클래스 다이어그램](/img/병헌/adapter_class_diagram.png)

```java

public class Client {
    public static void main(String[] args) {
        Adapter adapter = new Adapter();  // 객체는 adapter만 새로 생성한다.

        adapter.method("https://dev-qhyun.tistory.com/");
    }
}

public class Adapter extends Service implements ClientInterface {
    @Override
    public void method(String data) {
        specialMethod(data);
    }
}

```

- 클라이언트 호출 부분과 어댑터 코드의 변경만 있었을 뿐이다. 
- 책에서 해당 방식은 다중 상속이 가능한 프로그래밍 언어에서 사용이 가능하다고 설명이 되어있고 클래스로 구현되어있었으나, 비교를 위해서 interface를 활용해서 코드를 작성했다.
- 객체 생성을 따로 하지 않아도 된다는 점이 메모리 상 약간의 이점이 있는 것 같다.

## 패턴 적용
### 시기
- 기존 클래스를 활용하고싶지만 다른 코드들과 호환되지 않는 경우
- 부모 클래스에 추가할 수 없는 여러 기존 자식 클래스를 재사용하기 위해 사용

### 장단점
1. 단일 책임 원칙 준수
2. 개방/폐쇄의 원칙 준수
