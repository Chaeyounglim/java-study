# Item1. 생성자 대신 static 팩토리 메소드를 고려해라

```java
public static Boolean valueOf(boolean b){
    return b ? Boolean.TRUE : Boolean.FALSE;
}
```

public 생성자를 사용해서 객체를 생성하는 전동적인 방법 말고,
이와 같이 public static 팩토리 메소드를 사용해서 해당 클래스의 인스턴스를 만드는 방법도 있다.

이런 방법에는 각각 장단점이 있는데 다음과 같다.

<br/>


## 장점 1. 이름을 지어 반환될 객체의 특성을 쉽게 묘사할 수 있다.
```java
public class Foo{
    String name;
    String address;
    public Foo() {}

    public Foo(String name){this.name = name;}

    public static Foo withName(String name){
        return new Foo(name);
    }

    public static Foo withAddress(String address){
        Foo foo = new Foo();
        foo.address = address;
        return foo;
    }
    
    public static void main(String[] args){
        Foo foo = new Foo("sally"); // 생성자 메소드
        Foo foo = Foo.withName("sally");  // 정적 메소드
        Foo foo = Foo.withAddress("seoul");  // 정적 메소드
    }
}
```
한 클래스에 시그니처가 같은 생성자가 여러 개 필요할 경우에는, 생성자를 정적 메소드로 변경하여 차이가 드러나는 메소드 명으로 지어주자.

<br/>



## 장점2. 호출될 때마다 인스턴스를 새로 생성하지 않아도 된다.
```java
public class Foo{
    String name;
    String address;
    public Foo() {}
    
    private static final Foo STUFF = new Foo();

    public static Foo getFoo(){
        return STUFF;
    }

    public static void main(String[] args){
        Foo foo = Foo.getFoo();
    }
}
```
불변 클래스는 인스턴스를 미리 만들어 놓거나 새로 생성한 인스턴스를 캐싱하여 재활용하는 식으로 불필요한 객체 생성을 피할 수 있다.

반복되는 요청에 같은 객체를 반환하는 식으로 정적 팩토리 방식의 클래스는 언제 어느 인스턴스를 살아 있게 할지 청저히 통제할 수 있다.

이러한 클래스를 "인스턴스 통제 클래스" 라 한다.
인스턴스를 통제하면 클래스를 싱글턴으로 만들 수도, 인스턴스화 불가로 만들 수도 있다.

<br/>



## 장점3. 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.

클래스에서 만들어 줄 객체의 클래스를 선택하는 유연함이 있다. 리턴 타입의 인스턴스를 만들어줘도 되니, 리턴 타입은 인터페이스로 지정하고 그 인터페이스의 구현체는 API로 노출시키지 않지만 그 구현체의 인스턴스를 만들어 줄 수 있다는 말이다.
`java.util.Collections` 가 그 예에 해당한다.


<br/>


## 장점4. 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.

반환 타입의 하위 타입이기만 하면 어떤 클래스의 객체를 반환하든 상관없다.
가령, `EnumSet` 클래스는 public 생성자 없이 옹직 정적 팩토리만 제공하는데, OpenJDK에서는 원소의 수에 따라 두 가지 하위 클래스 `RegularEnumSet` 와 ` JumboEnumSet` 중 하나의 인스턴스를 반환한다.

클라이언트는 팩토리가 건네주는 객체가 어느 클래스의 인스턴스인지 알 수도, 알 필요도 없다. EnumSet의 하위 클래스이기만 하면 되는 것이다.

* 공개될 필요가 없는 클래스의 공통 기능은 private 메서드로 따로 추출하여 선언하는 것이 좋다.

<br/>

## 장점5. 정적 팩토리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.


<br/>

## 단점1. 상속을 하려면 public이나 protected 생성자가 필요하니 정적 팩토리 메소드만 제공하면 하위 클래스를 만들 수 없다.


<br/>

## 단점2. 정적 팩토리 메소드는 프로그래머가 찾기 어렵다.


해당 클래스가 제공해주는 중요한 static 메서드는 클래스의 상단에 주석으로 기재하여 알려주어야 한다.
