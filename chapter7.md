## 1. 상속(inheritance)

#### 1.1 상속의 정의와 장점

* 상속이란, 기존의 클래스를 재사용하여 새로운 클래스를 작성하는 것이다.
  - 재사용성 높일 수 있다.
  - 코드 중복 제거
  - 생산성과 유지보수에 크게 기여

``` java
class Child extends Parent {
  //...
}
```

* `Child` : 새로 작성하려는 클래스의 이름(상속 받는 것)
* `Parent` : 기존 클래스의 이름(상속 해주는 것)
* `extends` : 상속받기 위해 필요한 키워드

* **조상 클래스** : 부모(parent)클래스, 상위(super)클래스, 기반(base)클래스
* **자손 클래스** : 자식(child)클래스, 하위(sub)클래스, 파생된(derived)클래스

![상속관계도](https://raw.githubusercontent.com/shldhee/java-summary/master/images/7-1.png)

_상속관계를 그림으로 표현한 것으로 **상속계층도(class hierarchy)**라고 한다._

* 화살표 방향 있는 쪽이 상속해주는 클래스(조상 클래스)

_**다이어그램**__

![다이어그램](https://raw.githubusercontent.com/shldhee/java-summary/master/images/7-2.png)

``` java
class Parent {
  int age;
}

class Child extends Parent { }
```

![다이어그램](https://raw.githubusercontent.com/shldhee/java-summary/master/images/7-3.png)

* `Parent` 클래스에 있는 정수형 변수 인 `age`가 상속받은 `Child`클래스에 추가되는 것과 같은 효과

``` java
class Parent {
  int age;
}

class Child extends Parent {
  void play() {
    System.out.println("놀자~");
  }
}
```

![다이어그램](https://raw.githubusercontent.com/shldhee/java-summary/master/images/7-4.png)

* `Child` 자식 클래스에 추가한 `play()` 메서드는 `Parent` 조상 클래스에는 영향을 받지 않아 존재하지 않는다.

* **생성자와 초기화 블럭은 상속되지 않는다. 멤버만 상속된다.**
* **자손 클래스의 멤버 개수는 조상 클래스보다 항상 같거나 많다.**

``` java
class Parent {
  int age;
}
class Child extends Parent { }
class Child2 extends Parent { }
class GrandChild extends Child { }
```

![상속관계도](https://raw.githubusercontent.com/shldhee/java-summary/master/images/7-5.png)
![다이어그램](https://raw.githubusercontent.com/shldhee/java-summary/master/images/7-6.png)

* `Child`와 `Child2`는 아무런 관계가 없다.
* `GrandChild`는 `Child`를 상속받아 상속관계이다.
* `Parent`의 정수형 변수 `age`는 모두 사용 가능하다.

_TV예제__
``` java
class Tv {
    boolean power;
    int channel;

    void power() { power = !power; }
    void channelUp() { ++channel; }
    void channelDown() { --channel; }
}

class CaptionTv extends Tv {
    boolean caption;
    void displayCaption (String text) {
        if (caption) {
            System.out.println(text);
        }
    }
}
public class CaptionTvTest {
    public static void main(String[] args) {
        CaptionTv ctv = new CaptionTv();
        ctv.channel = 10;
        ctv.channelUp();
        System.out.println(ctv.channel);
        ctv.displayCaption("Hello, World");
        ctv.caption = true;
        ctv.displayCaption("Hello, World");
    }
}
```

* `TV`클래스로부터 상속받은 `CaptionTV`클래스 작성
* `CaptionTV`클래스(자손클래스)의 인스턴스를 생성하면 상속에 따라 조상클래스의 멤버들을 사용할 수 있다.

![다이어그램](https://raw.githubusercontent.com/shldhee/java-summary/master/images/7-7.png)

#### 1.2 클래스간의 관계 - 포함관계

* 포함(Composite)관계는 한 클래스의 멤버변수로 다른 클래스 타입의 참조변수를 선언하는 것을 뜻한다.

``` java
class Circle {
  int x;
  int y;
  int r;
}

class Point {
  int x; // 원점의 x좌표
  int y; // 원점의 y좌표
}

class Circle {
  Point c = new Point(); // 원점좌표
  int r;
}
```

* 재사용시 간결하고 손쉽게 클래스 작성 가능

``` java
class Car {
  Engine e = new Engine(); //엔전
  Door[] d = new Door[4]; // 문, 문의 개수를 넷으로 가정하고 배열로 처리했다.
}
```

* `Car`클래스의 단위구성요소인 `Engine, Door`를 멤버변수로 선언하여 포함관계를 맺었다.

#### 1.3 클래스간의 관계 결정하기

``` java
class Circle { // 포함관계
  Point c = new Point();
  int r;
}

class Circle extends Point { // 상속관계
  int r;
}
```

> 원(Circle)은 점(Point)이다. - Circle is a Point. (상속)
> 원(Circle)은 점(Point)을 가지고 있다. - Circle has a Point. (포함)

* 2번째가 옳다. 포함관계 사용

자동차는 스포츠카이다. X
자동차는 스포츠카를 가지고 있다. O
스포츠카는 자동차이다. O
스포츠타는 자동차를 가지고 있다. X

``` java
class Car {}
class SportsCar {}

자동차는 스포츠카를 가지고 있다. O
class Car {
  SportsCar c = new SportsCar();
}

스포츠카는 자동차이다. O
class SportsCar {
  Car c = new Car();
}
```

?둘다 가능한가?

_DrawShape.java_

``` java
public class DrawShape {
    public static void main(String[] args) {
        Point[] p = {
                new Point(100, 100),
                new Point(140, 50),
                new Point(200, 100)
        };

        Triangle t =  new Triangle(p);
        Circle c = new Circle(new Point(150,150),50);

        t.draw();
        c.draw();
    }
}

class Shape {
    String color="black";
    void draw(){
        System.out.printf("[color=%s]%n",color);
    }
}

class Point {
    int x;
    int y;

    Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    Point() {
        this(0, 0);
    }

    String getXY() {
        return "("+x+","+y+")";
    }
}

class Circle extends Shape {
    Point center;
    int r;

    Circle() {
        this(new Point(0,0),100);
    }

    Circle(Point center, int r) {
        this.center = center;
        this.r = r;
    }

    void draw() {
        System.out.printf("[center=(%d, %d), r=%d, color=%s]%n", center.x, center.y, r, color);
    }
}

class Triangle extends Shape {
    Point[] p = new Point[3];

    Triangle(Point[] p) {
        this.p = p;
    }

    void draw() {
        System.out.printf("[p1=%s, p2=%s, p3=%s, color=%s]%n", p[0].getXY(), p[1].getXY(), p[2].getXY(),  color);
    }
}
```

* **A circle is a Shpae. // 1. 원은 도형이다.**
* A circle is a Point. // 2. 원은 점이다?
* A circle has a Shpae. // 3. 원은 도형을 가지고 있다?
* **A circle has a Point. // 4. 원은 점을 가지고 있다.**

``` java
class Circle extends Shpae { // Circle과 Shape는 상속관계
  Point center;              // Circle과 Point는 포함관계
  int r;
  //...
}
```

* `Circle`, `Shpae` 클래스 모두 `draw()` 메서드를 가지고 있다.
* `Circle` 클래스에서 `draw()`를 사용할때 `Circle`에서 정의한 메서드 사용
* 조상 클래스에 정의된 메서드와 같은 메서드를 자손 클래스에 정의하는 것을 **오버라이딩**

_DeckTest_

``` java
public class DeckTest {
    public static void main(String[] args) {
        Deck d = new Deck();
        Card c= d.pick(0);
        System.out.println(c); // System.out.println(c.toString());

        d.shuffle();
        c = d.pick();
        System.out.println(c);
    }
}

class Deck {
    final int CARD_NUM = 52;
    Card cardArr[] = new Card[CARD_NUM];

    Deck () {
        int i = 0;

        for(int k = Card.KIND_MAX; k > 0; k--) {
            for (int n=0; n < Card.NUM_MAX; n++) {
                cardArr[i++] = new Card(k, n+1);
            }
        }
    }

    Card pick(int index) {
        return cardArr[index];
    }

    Card pick() {
        int index = (int)(Math.random() * CARD_NUM);
        return pick(index);
    }

    void shuffle() {
        for(int i = 0; i < cardArr.length; i++) {
            int r = (int)(Math.random() * CARD_NUM);

            Card temp = cardArr[i];
            cardArr[i] = cardArr[r];
            cardArr[r] = temp;
        }
    }
}

class Card {
    static final int KIND_MAX = 4;
    static final int NUM_MAX = 13;

    static final int SPADE = 4;
    static final int DIAMOND = 3;
    static final int HEART = 2;
    static final int CLOVER = 1;
    int kind;
    int number;

    Card() {
        this(SPADE, 1);
    }
    Card(int kind, int number) {
        this.kind = kind;
        this.number = number;
    }

    @Override
    public String toString() {
        String[] kinds = {"", "CLOVER", "HEART", "DIAMOND", "SPADE"};
        String numbers = "0123456789XJQK";
        return "kind : " + kinds[this.kind] + ", number : " + numbers.charAt(this.number);
    }
}
```

* `Deck(카드 한벌)`은 `Card`를 가지고 있다.(포함)
* `toString()`은 인스턴스의 정보를 문자열로 반환할 목적으로 정의
* `toString()`은 `Object`클래스에 정의 된것으로, 어떤 종류의 객체에서 호출 가능

#### 1.4 단일상속(single inheritance)

* 다른 객체지향 언어인 `C++`에서는 다중상속이 가능하나 `JAVA`에서는 단일상속만 지원한다.
* 다중 상속시 메서드를 구별할 수 있는 방법이 없다.

``` java
class Tv { }
class VCR {
  void play() {
    //..
  }
}

class TVCR exnteds TV {
  VCR vcr = new VCR(); // VCR 클래스를 포함시킨다.

  void play() {
    vcr.play();
  }
}
```

* `Tv`클래스를 조상으로 한 `TVCR`클래스에 `VCR`클래스를 포함
* `TVCR`클래스에 `VCR`클래스의 메서드와 일치하는 선언부를 가진 메서드를 선언`play()`
* 외부적으로는 `TVCR`클래스의 인스턴스를 사용하는것 같지만 내부적으로는`VCR`클래스의 인스턴스를 생성해서 사용

#### 1.5 Object클래스 - 모든 클래스의 조상

* `Object`클래스는 모든 클래스 상속계층도의 최상위에 있는 조상클래스이다.
* 다른 클래스로부터 상속 받지 않은 모든 클래스들은 자동적으로 `Object`클래스로부터 상속받게 함으로써 이것을 가능하게 한다.
* `toString()` 등 따로 메서드를 정의하지 않고 사용했던 이유다.

``` java
class Tv {

}

// ===

class Tv extends Object {

}
```

![다이어그램](https://raw.githubusercontent.com/shldhee/java-summary/master/images/7-8.png)


## 2. 오버라이딩(overriding)

#### 2.1 오버라이딩이란?

* 조상 클래스로부터 상속받은 메서드의 내용을 변경하는 것을 오버라이딩이라고 한다.

``` java
class Point {
  int x;
  int y;

  String getLocation() {
    return 'x :' + x + ", y :" + y;
  }
}

class Point3D extends Point {
  int z;

  String getLocation() { // 오버라이딩
    return "x : " + x + ", y :" + y + ", z : "+ z;
  }
}
```

#### 2.2 오버라이딩의 조건

* 자손 클래스에서 오버라이딩하는 메서드는 조상 클래스의 메서드와
  1. 이름이 같아야 한다.
  2. 매개변수가 같아야 한다.
  3. 반환타입이 같아야 한다.

* **한마디로 요약하면 선언부가 서로 일치해야 한다.**
* 접근 제어자와 예외는 제한된 조건 하에서만 다르게 변경할 수 있다.

1. 접근 제어자는 조상 클래스의 메서드보다 좁은 범위로 변경할 수 없다.
  * 조상 클래스에 정의된 메서드의 접근 제어자가 `protected`라면
  * 오버라이딩하는 자손 클래스의 메서드는 접근 제어자가 `protected`, `public`
  * 접근 제어자 순서(접근 범위 넓은 것에서 좁은 것 순서)
  * ***public, protected, (default), private***
1. 조상 클래스의 메서드보다 많은 수의 예외를 선언할 수 없다.

``` java
Class Parent {
  void parentMethod() throws IOException, SQLException {

  }
}

Class Child extends Parent {
  void parentMethod() throws IOException {

  }
}
```

* 선언된 예외의 갯수의 문제가 아니다.
* 범위의 문제
* `throws Exception`은 모든 예외의 최고 조상이므로 가장 많은 개수의 예외를 던질 수 있도록 선언한 것이다.(모든 예외를 다 포함하므로 가장 개수가 많다.)

* 조상 클래스의 메서드를 자손 클래스에서 오버라이딩할 때
  1. 접근 제어자를 조상 클래스의 메서드보다 좁은 범위로 변경할 수 없다.
  2. 예외는 조상 클래스의 메서드보다 많이 선얼할 수 없다.
  3. 인스턴스메서드를 `static`메서드로 또는 그 반대로 변경할 수 없다.

* `static`는 조상이나 자손 클래스에서 중복된 이림으로 사용 가능하나 오버라이딩이 아닙니다.
* 클래스 이름으로 구별이 가능하기 때문입니다.(`클래스이름.메서드이름()`)

#### 2.3 오버로딩 vs 로버라이딩

* 오버로딩(overloading) : 기존에 없는 새로운 메서들르 정의하는 것(new)
* 오버라이딩(overriding) : 상속받은 메서드의 내용을 변경하는 것(change, modify)

``` java
class Parent {
  void parentMethod () {}
}

class Child extends Parent {
  void parentMethod() {} // 오버라이딩
  void parentMethod(int i) {} // 오버로딩

  void childMethod() {}
  void childMethod(int i) {} // 오버로딩
  void childMethod() {} // 에러

}
```

#### 2.4 super

* super는 자손 클래스에서 조상 클래스로부터 상속받은 멤버를 참조하는 사용되는 참조변수이다.
* 상속받은 멤버와 자신의 클래스에 정의된 멤버의 이름이 같을 떄 사용해서 구별할 수 있다.
* 조상과 자신의 멤버를 구별하는데 사용된다는 점을 제외하고는 `super`와 `this`는 근본적으로 같다.
* `static`(클래스)메서드는 인스턴스와 관련이 없다. 그래서 `this와 마찬가지로 `super`역시 `static`메서드에서는 사용할 수 없고 인스턴스메서드에서만 사용할 수 있다.

``` java
class SuperTest {
  public static void main(String args[]) {
    Child c = new Child();
    c.method();
  }
}

class Parent {
  int x = 10;
}

class Child extends Parent {
  void method() {
    System.out.printlf("x=" + x); // x= 10
    System.out.printlf("this.x=" + this.x); // this.x = 10
    System.out.printlf("super.x=" + super.x); // super.x = 10
  }
}
```

* `x`, `this.x`, `super.x` 모두 같은 변수

``` java
class SuperTest2 {
  public static void main(String args[]) {
    Child c = new Child();
    c.method();
  }
}

class Parent {
  int x = 10;
}

class Child extends Parent {
  int x = 20;
  void method() {
    System.out.printlf("x=" + x); // x = 20;
    System.out.printlf("this.x=" + this.x); // this.x = 20
    System.out.printlf("super.x=" + super.x); // super.x = 10
  }
}
```

* 조상클래스(`Parent`)와 자손클래스(`Child`)의 같은 멤버변수가 있을시에는 `super`는 조상, `this`는 자손을 가리킨다.
* 메서드도 마찬가지

``` java
class Point {
  int x;
  int y;

  String getLocation() {
    return "x :" + x + ", y :"+ y;
  }
}

class Point3D extends Point {
  int z;

  String getLocation() {
    // return "x :" + x + ", y :"+ y + ", z :" + z; // 아래와 같다.
    return super.getLocation() + ", z :" + z; // 조상의 메서드 호출
  }
}
```

#### 2.5 super() - 조상 클래스의 생성자
....
