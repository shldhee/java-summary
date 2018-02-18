# 예외처리

## 1.1 프로그램 오류

프로그램이 실행 중 어떤 원인에 의해서 오작동을 하거나 비정상적으로 종료되는 경우가 있다. 이러한 결과를 초래하는 원인을 프로그램 에러 또는 오류라고 한다.
발생 시점에 따라 에러를 구분한다.

* 컴파일 에러 - 컴파일 시에 발생하는 에러
* 런타임 에러 - 실행 시에 발생하는 에러
* 논리적 에러 - 실행은 되지만, 의도와 다르게 동작하는것

자바에서는 실행 시(runtime) 발생할 수 있는 프로그램 오류를 2개로 구분한다.

* 에러(error) - 프로그램 코드에 의해서 **수습될 수 없는 심각한 오류**
* 예외(exception) - 프로그램 코드에 의해서 **수습될 수 있는 다소 미약한 오류**

## 1.2 예외 클래스의 계층 구조

모든 클래스의 조상은 `Object`이므로 `Exception`과 `Error`클래스 역시 `Object`클래스의 자손들이다.

![exception1](https://raw.githubusercontent.com/shldhee/java-summary/master/images/exception1.png)

* 모든 예외의 최고 조상은 `Exception`클래스이다.
* 예외 클래스는 크게 두 그룹으로 나뉜다.

![exception2](https://raw.githubusercontent.com/shldhee/java-summary/master/images/exception2.png)

* RuntimException클래스들 - 프로그래머의 실수로 발생하는 예외 - 예외처리 선택(컴파일러가 예외 체크 안함)
  * 배열의 범위를 벗어나거나(IndexOutOfBoundsException)
  * 값이 null인 참조 변수의 멤버를 호출(NullPointerException)
  * 클래스간의 형변환을 잘못했다던가(ClassCastException)
  * 정수를 0으로 나누려고(ArithmeticException)하는 경우에 발생
* Exception클래스들 - 사용자의 실수와 같은 외적인 요인에 의해 발생하는 예외 - 예외처리 필수(컴파일러가 예외 체크 한다) 사용자의 동작의 의해 발생
  * 존재하지 않은 파일의 이름을 입력(FileNotFoundException)
  * 실수로 클래스의 이름을 잘못 적었다던가(ClassNotFoundException)
  * 입력한 데이터 형식이 잘못된 경우(DataFormatException)

## 1.3 예외처리하기 - try-catch 문

프로그램 실행 도중에 발생하는 에러는 어쩔 수 없지만, 예외는 프로그래머가 이에 대한 처리를 미리 해줘야 한다.

발생한 예외를 처리하지 못하면, 프로그램은 비정상 종료 되고, 처리되지 못한 예외(uncaught exception)는 JVM의 '예외처리기(UncaughtExceptionHandler)'가 받아서 예외의 원인을 화면에 출력한다.

> 예외처리(exception handling)란?

* 정의 : 프로그램 실행 시 발생할 수 있는 예기치 못한 예외의 발생에 대비한 코드를 작성하는 것
* 목적 : 프로그램의 비정상 종료를 막고, 정상적인 실행상태를 유지하는 것

try-catch문의 구조

``` java
  try {
      // 예외가 발생할 가능성이 있는 문장들을 넣는다.
  } catch (Exception1 e1) {
      // Exception1이 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
  } catch (Exception2 e2) {
      // Exception2이 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
  }
```

하나의 `try`블럭 다음에는 여러 종류의 예외를 처리할 수 있도록 하나 이상의 `catch`블럭이 올 수 있으며, 이 중 **발생한 예외의 종류와 일치하는 단 한 개**의 `catch`블럭만 수행된다. 일치하는 `catch`블럭이 없으면 예외는 처리되지 않는다.

try-catch문(참조변수 e)

``` java
  try {
      try { } catch (Exception e) { }
  } catch (Exception e) {
      try { } catch (Exception e) { } // 에러 변수 e가 중복 선언되었다.
      }
  }

  try {

  } catch (Exception e) {  }
```

* 하나의 메서드의 여러 `try-catch`문 사용 가능
* 중첩해서 사용 가능(`catch`블럭 내의 코드에서 예외가 발생할 수 있기 때문)
* 중첩될때는 변수 `e`가 겹치면 안되므로 중첩될때는 변수를 다르게 한다.

0으로 나눌때 에러

``` java
public class ExceptionEx2 {
  public static void main(String[] args) {
    int number = 100;
    int result = 0;

    for(int i =0; i < 10; i++) {
        result = number / (int)(Math.random() * 10);
        System.out.println(result);
    }
  }
}
```

`Exception in thread "main" java.lang.ArithmeticException: / by zero at ExceptionEx2.main(ExceptionEx2.java:7)`

* 7번째 쭐에서 랜덤값이 0이 나와 나눌때 에러가 발생
* `ArithmeticException`이 발생, 이 예외는 산술연산과정에서 오류가 있을때 발생한다.

try-catch문으로 수정

``` java
public class ExceptionEx2 {
  public static void main(String[] args) {
    int number = 100;
    int result = 0;

    for(int i =0; i < 10; i++) {
      try {
        result = number / (int)(Math.random() * 10);
        System.out.println(result);
      } catch (ArithmeticException e) {
        System.out.println("0");
      }
    }
  }
}
```

* 이제 랜덤값이 0이 나올경우 `catch`블럭이 실행된다.

## 1.4 try-catch 문에서의 흐름

### try블럭 내에서 예외가 발생한 경우

1. 발생한 예외와 일치하는 `catch`블럭이 있는지 확인
1. 일치하는 `catch`블럭을 찾으면 블럭 내용을 실행하고 `try-catch`문을 빠져나간다.
1. 만일 일치하는 `catch`블럭을 찾지 못하면, 예외는 처리되지 못한다.

### try블럭 내에서 예외가 발생하지 않은 경우

1. `catch`블럭을 거치지 않고 전체 `try-catch`문을 빠져나가서 수행을 계속한다.

``` java
public static void main(String[] args) {
  System.out.println(1);
  System.out.println(2);
  try {
    System.out.println(3);
    System.out.println(4);
  } catch (Exception e) {
    System.out.println(5);
  }
  System.out.println(6);
}

/* 실행결과
  1
  2
  3
  4
  6
*/
```

예외 발생

``` java
public static void main(String[] args) {
  System.out.println(1);
  System.out.println(2);
  try {
    System.out.println(3);
    System.out.println(0/0);
    System.out.println(4);
  } catch (ArithmeticException ae) {
    System.out.println(5);
  }
  System.out.println(6);
}

/* 실행결과
  1
  2
  3
  5
  6
*/
```

## 1.5 예외의 발생과 catch블럭

* 예외가 발생하면, 예외에 해당하는 클래스의 인스턴스가 만들어진다.
* `ArithmeticException`이 발생하면 ArithmeticException 인스턴스가 만들어진다.
* `catch`블럭의 괄호()내에 선언된 참조변수의 종류와 예외클래스의 인스턴스를 `instacnof`연산자를 이용하여 검사한다.
* `true`인 `cacth`블럭을 발견하면 실행 없으면 예외는 처리되지 않는다.
* 모든 예외 클래스는 `Exception`클래스의 자손이므로 `catch`블럭의 괄호()에 `Exception`클래스 타입의 참조변수를 선언해 놓으면 어떤 종류의 예외가 발생해도 처리된다.

``` java
public static void main(String[] args) {
  System.out.println(1);
  System.out.println(2);
  try {
    System.out.println(3);
    System.out.println(0/0);
    System.out.println(4);
  } catch (Exception e) {
    System.out.println(5);
  }
  System.out.println(6);
}
```

``` java
public static void main(String[] args) {
  System.out.println(1);
  System.out.println(2);
  try {
    System.out.println(3);
    System.out.println(0/0);
    System.out.println(4); // 예외처리 되서 실행되지 않는다.
  } catch (ArithmeticException ae) {
    if (ae instanceof ArithmeticException)
      System.out.println("true");
      System.out.println("ArithmeticException");
  } catch (Exception e) {
    System.out.println("Exception");
  }
  System.out.println(6);
}

/*
  1
  2
  3
  true
  AritheticExcetion
  6
*/
```

* `catch (ArithmeticException ae)` 가 실행되므로 `catch (Exception e)` 블럭은 실행되지 않는다.
* `Exception`은 `catch`블럭 맨 끝에 넣는다.

### `printStackTrace()와 getMessage()`

* printStackTrace() - 예외발생 당시의 호출스택(Call Stack)에 있었던 메서드의 정보와 예외 메시지를 화면에 출력한다.
* getMessage() - 발생한 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있다.

``` java
public static void main(String[] args) {
  System.out.println(1);
  System.out.println(2);
  try {
    System.out.println(3);
    System.out.println(0/0);
    System.out.println(4);
  } catch (ArithmeticException ae) {
    ae.printStackTrace();
    System.out.println("예외메시지 : " + ae.getMessage());
  }
  System.out.println(6);
}

/*
  1
  2
  3
  java.lang.ArithmeticException: / by zero
    at ExceptionEx1.main(ExceptionEx1.java:8)
  예외메시지 : / by zero
  6
*/
```

#### 멀티 catch블럭 (JDK 1.7)

``` java
try {
  //...
} catch (ExceptionA e) {
  e.printStackTrace();
} catch (ExceptionB e) {
  e2.printStackTrace();
}
```

``` java
try {
  //...
} catch (ExceptionA | ExceptionB e) {
  e.printStackTrace();
}
```

* `|`를 사용해서 중복된 코드를 줄일 수 있다.
* 예외클래스의 개수 제한이 없다.
* 만약 예외클래스가 조상, 자식 관계에 있다면 컴파일 에러 발생(조상클래스 하나만 써주면 되는데 불필요하면 코드이므로 에러 발생)
* 여러 예외 클래스가 있어 어떤 예외가 발생한지를 확인 하려면 `instancof` 연산자 사용

``` java
try {
  // ...
} catch (ExceptionA | ExceptoinB e) {
  e.methodA(); // 에러 ExceptionA에 선언된 methodA()는 호출불가

  if(e instanceof ExceptoinA) {
    ExceptionA e1 = (ExceptionA)e;
    e1.methodA(); // OK. ExceptionA에 선언된 메서드 호출가능
  } else { // if(e instanceof ExceptionB)
    // ...
  }

  e.printStackTrace();
}
```

* 위와 같이 합칠일은 거의 없다.