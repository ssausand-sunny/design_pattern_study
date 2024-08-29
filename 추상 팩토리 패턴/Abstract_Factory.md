# 추상 팩토리 패턴
> 추상 팩토리는 관련 객체들의 구상 클래스들을 지정하지 않고도 관련 객체들의 모음을 생성할 수 있도록 하는 생성패턴이다.

## 추상 팩토리 패턴 예시 (삼성 & 애플)
### 문제
- 비교하기 쉽게 삼성과 애플을 예로 들어보도록 하자
- 해당 제품들은 노트북, 핸드폰, 패드, 워치, 이어폰 등과 같이 관련 제품들로 형성된 제품군이 존재한다.
- 맥북 생태계라는 말이 존재하듯 새로운 개별 제품 객체를 생성했을 때, 이 객체들이 기존의 같은 패밀리 내에 있는 다른 제품 객제들과 일치하는 스타일을 가지도록 해야한다.
- 아이폰을 쓰면서 버즈를 쓰거나, 갤럭시를 쓰면서 애플워치를 찬다면 굉장히 어지러울 것이기 대문이다.

### 해결책
1. 개별적인 인터페이스를 명시적으로 선언하는 방법의 사용
  ![](https://velog.velcdn.com/images/hunnibs/post/fa9a2a0d-29d7-4fc0-9df7-b884d96e2e0f/image.png)
- 위와 같이 같은 객체의 변형을 구현하였다면, 다음은 추상 팩토리 패턴을 적용하는 것이다.

2. 제품 패밀리 내의 모든 개별 제품들의 생성 메서드들을 목록화시키기
   ![](https://velog.velcdn.com/images/hunnibs/post/872e3389-c336-4859-92e6-31430633cd8b/image.png)
- 위와 같이 추상 팩토리 인터페이스를 기반으로 별도의 팩토리 클래스를 생성하여 각 팩토리는 특정 종류의 제품을 반환하게만 한다.
- 삼성 공장에서는 삼성 제품만, 애플 공장에서는 애플 제품만 생성할 수 있듯이 말이다.

**주의할 점**
- 클라이언트 코드는 추상 인터페이스를 통해 팩토리들과 제품들 모두와 함께 작동할 수 있도록 해야한다.
- 그래서 클라이언트 코드는 변형되지 않고 자유자재로 변경할 수 있어야한다.

### 예시 마무리
![](https://velog.velcdn.com/images/hunnibs/post/d8917def-3c32-4ed9-8d61-1498970738ba/image.png)
1. 책을 기준으로 추상 제품들은 Watch interface로 개별 연관 제품들의 집합에 대한 인터페이스를 뜻한다.
2. 구상 제품들은 AppleWatch와 GalaxyWatch와 같이 추상 제품들의 다양한 구현들이다. 각 추상 제품은 주어진 모든 변형에 구현 되어야 한다.
3. 추상 팩토리 인터페이스는 가각의 추상 제품들을 생성하기 위한 메서드 집합으로 ElectricFactory가 이에 해당한다.
4. 구상 팩토리들은 AppleFactory와 SamsungFactory가 해당하며 해당 구상 팩토리에 생성 매서드들은 추상 제품을 반환해야한다.(Watch를 반환하는 것 처럼)
이렇게 설계한다면 클라이언트 코드가 팩토리에서 받은 제품의 특정 변형(구상 제품들)과 결합되지 않고 어떤 팩토리나 제품과 작업할 수 있는 것이다. 

## 간단한 전체 코드
코드는 아무런 로직 없이 껍데기만 대충 만들어봤다. 

```java

// 추상 제품
public interface Watch {
    void display();
    void strap();
}

// 애플 워치
public class AppleWatch implements Watch{
  @Override
  public void display() {

  }

  @Override
  public void strap() {

  }
}

// 갤럭시 워치
public class GalaxyWatch implements Watch{
  @Override
  public void display() {

  }

  @Override
  public void strap() {

  }
}

// 추상 팩토리
public interface ElectricFactory {
  void createPhone();
  void createDesktop();
  Watch createWatch();
  void createEarphone();
  void createPad();
}

// 애플 팩토리
public class AppleFactory implements ElectricFactory{
  @Override
  public void createPhone() {

  }

  @Override
  public void createDesktop() {

  }

  @Override
  public Watch createWatch() {
    return new AppleWatch();
  }

  @Override
  public void createEarphone() {

  }

  @Override
  public void createPad() {

  }
}

// 삼성 팩토리
public class SamsungFactory implements ElectricFactory{
  @Override
  public void createPhone() {

  }

  @Override
  public void createDesktop() {

  }

  @Override
  public Watch createWatch() {
    return new GalaxyWatch();
  }

  @Override
  public void createEarphone() {

  }

  @Override
  public void createPad() {

  }
}

```