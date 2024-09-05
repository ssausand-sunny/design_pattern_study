# 싱글톤

---
### 정의

싱글턴은 클래스에 인스턴스가 하나만 있도록 하면서 이
인스턴스에 대한 전역 접근(액세스) 지점을 제공하는 생성
디자인패턴입니다.


---

### 문제?

싱글턴패턴은 한번에 두가지의 문제를 동시에 해결함으로써 단일책임원칙을 위반합니다.

싱글톤 객체는 2가지의 두가지의 책임을 가지기 때문이라고 하네요..
1. 객체 생성과 관련된 책임
2. 비즈니스 로직

--- 
### 구현 방법

![img](https://github.com/user-attachments/assets/d27c6f63-394f-4e72-92ce-bd280755dbbc)


1. 일반적인 방법
```java
public class Singleton{
    private static Singleton instance;
    private Singleton(){

    }
    public static Singleton getInstance(){
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```
2. 멀티스레딩 환경
- 멀티스레딩 환경에서 두개이상의 객체가 생성될 가능성이 있다. 따라서, 그에 대한 처리를 ㅎ ㅐ야 한다.
```java
public class Singleton{
    // volatile 은 모든 스레드에서 최신값을 사용한다는 보장이라고 함
    private static volatile Singleton instance;
    private Singleton(){

    }
    public static Singleton getInstance(){
        if (instance == null) {
            synchronized (Singleton.class) {
                // 락을 잡는 동안 초기화 되어있는지 확인
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}

```
3. 직렬화 역직렬화 문제
- 싱글톤 객체를 직렬화하고 역 직렬화 하는 경우 새로운 객체가 생성된다고 한다..
```java
public class Singleton implements Serializable {
    // volatile 은 모든 스레드에서 최신값을 사용한다는 보장이라고 함
    private static volatile Singleton instance;
    private static final long serialVersionUID = 1L;

    private Singleton(){

    }
    public static Singleton getInstance(){
        if (instance == null) {
            synchronized (Singleton.class) {
                // 락을 잡는 동안 초기화 되어있는지 확인 (Double Check)
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
    // 이 함수를 작성안하면 직렬화 했을시 새로운 객체가 생성되며 싱글톤이 깨짐
    private Object readResolve() throws ObjectStreamException {
        return instance;
    }
}

```
4. enum 사용
- enum 은 기본적으로 싱글톤 방식으로 생성된다 하지만, 여러 프레임워크에서 지원하지 않으니 굳이.. 싶기도 하다
```java
public enum EnumSingleton {
    SINGLETON;
    public void someFunction(){
        System.out.println("enum some function");
    }
}
```
---

### 주 사용처
디스크 연결, 네트워크 통신, DBCP 커넥션풀, 스레드풀, 캐시, 로그 기록 객체 등에 이용된다.
