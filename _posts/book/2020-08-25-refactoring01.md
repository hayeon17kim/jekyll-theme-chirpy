---
title: ":book: 리팩토링 #1 (1): 메서드 분해와 기능 재분배"
categories: book
tags: [ java ]
toc: true
---

# 리팩토링

![리팩토링](https://bookthumb-phinf.pstatic.net/cover/070/476/07047630.jpg?type=m5)

마틴 파울러의 "리팩토링: 코드 품질을 개선하는 객체지향 사고법"을 읽으면서 공부한 내용을 올립니다. 

첫 장은 주어진 프로그램 코드를 리팩토링하면서 전반적인 리팩토링의 유용성과 방법을 조망하는 챕터입니다.



## 원본 프로그램

- 프로그램 개요: 고객이 대여한 비디오와 대여 기간을 표시한 후, 비디오 종류와 대여 기간을 토대로 대여료를 계산한다. 비디오의 종류는 일반물, 아동물, 최신물 세 종류이다. 대여료 계산과 더불어 내역을 바탕으로 적립 포인트도 계산되는데, 이 포인트는 비디오가 최신물인지 아닌지에 따라 달라진다.

**Movie 클래스**: 비디오 데이터 클래스

```java
package com.monica.rms.Exam01;

// 비디오 데이터 클래스
public class Movie {
  public static final int CHILDRENS = 2;
  public static final int REGULAR = 0;
  public static final int NEW_RELEASE = 1;
  private String _title;
  private int _priceCode;

  public Movie(String title, int priceCode) {
    _title = title;
    _priceCode = priceCode;
  }

  public int getPriceCode() {
    return _priceCode;
  }

  public void setPriceCode(int priceCode) {
    _priceCode = priceCode;
  }

  public String getTitle() {
    return _title;
  }
}
```



**Rental 클래스:** 대여 정보 클래스

```java
package com.monica.rms.Exam01;
// 대여 정보 클래스

public class Rental {
  private Movie _movie;
  private int _daysRented;

  public Rental(Movie movie, int daysRented) {
    _movie = movie;
    _daysRented = daysRented;
  }

  public int getDaysRented() {
    return _daysRented;
  }

  public Movie getMovie() {
    return _movie;
  }
}

```



**Customer 클래스:** 고객 클래스

- statement() 메서드: 내역 생성

```java
package com.monica.rms.Exam01;

import java.util.Enumeration;
import java.util.Vector;

public class Customer {
  private String _name;
  private Vector _rentals = new Vector();

  public Customer(String name) {
    _name = name;
  }

  public void addRental(Rental arg) {
    _rentals.addElement(arg);
  }

  public String getName() {
    return _name;
  }

    
    
  //내역을 생성하는 statement 메서드    
  public String statement() {
    double totalAmount = 0;
    int frequentRenterPoints = 0;
    Enumeration rentals = _rentals.elements();
    String result = getName() + "님의 대여 기록\n";
    while (rentals.hasMoreElements()) {
      double thisAmount = 0;
      Rental each = (Rental) rentals.nextElement();

      // 비디오 종류별 대여료 계산
      switch (each.getMovie().getPriceCode()) {
        case Movie.REGULAR:
          thisAmount += 2;
          if (each.getDaysRented() > 2)
            thisAmount += (each.getDaysRented() - 2) * 1.5;
          break;
        case Movie.NEW_RELEASE:
          thisAmount += each.getDaysRented() * 3;
          break;
        case Movie.CHILDRENS:
          thisAmount += 1.5;
          if (each.getDaysRented() > 3)
            thisAmount += (each.getDaysRented() - 3) * 1.5;
          break;
      }
      // 적립 포인트를 1 포인트 증가
      frequentRenterPoints++;
      // 최신물을 이틀 이상 대여하면 보너스 포인트 지급
      if ((each.getMovie().getPriceCode() == Movie.NEW_RELEASE) && each.getDaysRented() > 1)
        frequentRenterPoints++;

      // 이번에 대여하는 비디오 정보와 대여료 출력
      result += "\t" + each.getMovie().getTitle() + "\t" + String.valueOf(thisAmount) + "\n";
      // 현재까지 누적된 총 대여료
      totalAmount += thisAmount;
    }
    // 푸터 행 추가
    result += "누적 포인트: " + String.valueOf(totalAmount) + "\n";
    result += "적립 포인트: " + String.valueOf(frequentRenterPoints);
    return result;
  }
}

```

### Vector와 ArrayList
- 공통점: 동일한 내부구조를 갖는다.
  - 값이 추가되면 자동으로 크기가 조절되며 그 다음 객체들은 한 자리씩 뒤로 이동한다.
- 차이점: 동기화 여부
  - Vector: 동기화된 메서드로 구성되어 있다. 따라서 멀티스레드 환경에서 안전하게 객체를 추가하고 삭제할 수 있는 장점이 있지만, 스레드가 1개일 때도 동기화를 하기 때문에 ArrayList보다 성능이 떨어진다는 단점이 있다.
  - ArrayList: 자동 동기화 기능이 빠져 있고, 동기화 옵션이 존재한다. Vector에 비해 속도가 빨라 보다 많이 쓰인다.
### Enumeration과 Iterator

- 인터페이스: 직접 new 연산자를 통해 객체를 생성할 수 없으며, 선언된 메서드는 그 인터페이스를 사용하는 클래스로 구현해서 사용해야 한다. 
  - 사용법
    - `(컬렉션 객체).iterator()`
    - `(컬렉션 객체).elements()`
- 메서드
  - 객체들의 집합에서 각각의 객체들을 한 순간에 처리할 수 있는 메서드를 제공한다.
  - Enumeration의 `hasMoreElements()` == Iterator의 `hasNext()`
  - Enumeration의 `nextElement()` == Iterator의 `next()`
- Enumeration은 Collection 프레임워크가 만들어지기 전, Iterator의 이전 버전이다. 기능이 같기 때문에 가능하면 Enumeration 대신 Iteration 사용을 권장하고 있다.



> List 혹은 Set 인터페이스를 구현하는 컬렉션은 iterator()가 컬렉션의 특징에 맞게 설계가 되어 있다.

> Vector 클래스의 elements()라는 메서드는 객체의 모든 요소들을 Enumeration 객체로 반환한다.

## 원본 코드의 문제점

- Customer 클래스의 `statement()` 메서드
  - 한 메서드에 기능이 지나치게 많이 집중되어 있다.
  - 기능 중 대부분은 다른 클래스에 들어가야 한다.
- 기능을 추가하기 어렵다.
  - HTML 내역 출력 기능을 추가하고자 한다면 어떻게 해야 할까?
  - 방법1) HTML로 출력하기 위해서는 statement() 메서드를 복사에서 필요한 부분을 수정한다.
    - 한 기능을 수정할 때마다 두 메서드를 똑같이 수정해야 한다. 따라서 실수할 가능성이 높아진다.
  - 방법2) 리팩토링을 통해 기능을 추가하기 쉽게 만든 후 기능을 추가한다.



## 테스트 코드 작성

리팩토링 단계마다 프로그램이 정상적으로 작동하는지 확인하기 위해 다음과 같이 테스트 코드를 작성해보았다.

```java
package com.monica.rms.Exam01;

public class App {
  public static void main(String[] args) {

    Movie coffeeAndCigarette = new Movie("커피와 담배", Movie.REGULAR);
    Movie floridaProject = new Movie("플로리다 프로젝트", Movie.REGULAR);
    Movie parasite = new Movie("기생충", Movie.NEW_RELEASE);
    Movie roma = new Movie("로마", Movie.NEW_RELEASE);
    Movie moana = new Movie("모아나", Movie.CHILDRENS);
    Movie pansLabyrinth = new Movie("판의 미로", Movie.CHILDRENS);

    Customer monica = new Customer("Monica");
    monica.addRental(new Rental(coffeeAndCigarette, 4));
    monica.addRental(new Rental(roma, 4));

    Customer rachel = new Customer("Rachel");
    rachel.addRental(new Rental(parasite, 3));
    rachel.addRental(new Rental(moana, 5));

    Customer phoebe = new Customer("Phoebe");
    phoebe.addRental(new Rental(floridaProject, 10));
    phoebe.addRental(new Rental(pansLabyrinth, 10));

    System.out.println(monica.statement());
    System.out.println("==================");
    System.out.println(rachel.statement());
    System.out.println("==================");
    System.out.println(phoebe.statement());
  }
}
```

> 테스트 스위트(test suite)란?
>
> - 테스트 케이스를 하나로 묶은 것으로, 테스트 케이스를 실행한다.
>   - 테스트 케이스: 하나의 메서드를 테스트하는 단위를 말한다.
> - 테스트 스위트를 정의하면 특정 클래스나 특정 메서드를 선택하여 단위 테스트를 할 수 있다. 

테스트 코드 실행 결과는 다음과 같다.

```console
Monica님의 대여 기록
	커피와 담배	5.0
	로마	12.0
누적 포인트: 17.0
적립 포인트: 3
==================
Rachel님의 대여 기록
	기생충	9.0
	모아나	4.5
누적 포인트: 13.5
적립 포인트: 3
==================
Phoebe님의 대여 기록
	플로리다 프로젝트	14.0
	판의 미로	12.0
누적 포인트: 26.0
적립 포인트: 2
```



## statement 메서드 분해와 기능 재분배

- 메서드 추출 기법을 적용한 코드

```java
package com.monica.rms.Exam02;

// <statement 메서드 분해와 기능 재분배>
// 논리적 코드 뭉치를 찾아 메서드 추출 기법을 적용
//
import java.util.Enumeration;
import java.util.Vector;

public class Customer {
  private String _name;
  private Vector _rentals = new Vector();

  public Customer(String name) {
    _name = name;
  }

  public void addRental(Rental arg) {
    _rentals.addElement(arg);
  }

  public String getName() {
    return _name;
  }

  public String statement() {
    double totalAmount = 0;
    int frequentRenterPoints = 0;
    Enumeration rentals = _rentals.elements();
    String result = getName() + " 고객님의 대여 기록\n";
    while (rentals.hasMoreElements()) {
      double thisAmount = 0;
      Rental each = (Rental) rentals.nextElement();

      // 비디오 종류별 대여료 계산 함수를 호출
      thisAmount = amountFor(each);
        
      // 적립 포인트를 1 포인트 증가
      frequentRenterPoints++;
      // 최신물을 이틀 이상 대여하면 보너스 포인트 지급
      if ((each.getMovie().getPriceCode() == Movie.NEW_RELEASE) && each.getDaysRented() > 1)
        frequentRenterPoints++;

      // 이번에 대여하는 비디오 정보와 대여료 출력
      result += "\t" + each.getMovie().getTitle() + "\t" + String.valueOf(thisAmount) + "\n";
      totalAmount += thisAmount;

      // footer 행 추가
      result += "누적 대여료: " + String.valueOf(totalAmount) + "\n";
      result += "적립 포인트: " + String.valueOf(frequentRenterPoints);

    }
    return result;
  }

  // 비디오 종류별 대여료 계산 기능을 빼어 별도의 함수로 작성
  private double amountFor(Rental each) {
    double thisAmount = 0;
    switch (each.getMovie().getPriceCode()) {
      case Movie.REGULAR:
        thisAmount += 2;
        if (each.getDaysRented() > 2)
          thisAmount += (each.getDaysRented() - 2) * 1.5;
        break;
      case Movie.NEW_RELEASE:
        thisAmount += each.getDaysRented() * 3;
        break;
      case Movie.CHILDRENS:
        thisAmount += 1.5;
        if (each.getDaysRented() > 3)
          thisAmount += (each.getDaysRented() - 3) * 1.5;
        break;
    }
    return thisAmount;
  }
}
```

- 리팩토링은 프로그램을 조금씩 단계적으로 수정하므로 실수해도 버그를 찾기 쉽다.
- 이클립스의 메서드 추출 기능을 사용하면 쉽게 메서드를 추출할 수 있다. 
- 추가로 다음과 같이 변수명을 수정하여 용도를 확실히 드러냈다.

```java
  private double amountFor(Rental aRental) {
    double result = 0;
    switch (aRental.getMovie().getPriceCode()) {
      case Movie.REGULAR:
        result += 2;
        if (aRental.getDaysRented() > 2)
          result += (aRental.getDaysRented() - 2) * 1.5;
        break;
      case Movie.NEW_RELEASE:
        result += aRental.getDaysRented() * 3;
        break;
      case Movie.CHILDRENS:
        result += 1.5;
        if (aRental.getDaysRented() > 3)
          result += (aRental.getDaysRented() - 3) * 1.5;
        break;
    }
    return result;
  }
```



## 대여료 계산 메서드 이동

- 메서드 이동 이유
  - `amountFor()`메서드의 경우 현재 속해 있는 `Cusmomer` 클래스의 정보를 사용하는 것이 아니라 다른 클래스의 정보를 사용하고 있다.
  - 메서드는 자신이 사용하는 데이터와 같은 객체에 들어 있어야 한다.

- `Customer`클래스의 `amountFor()` 메서드 코드를 `Rental` 클래스로 복사
- 클래스 환경에 맞게 수정
  - 매개변수 삭제
  - 메서드명 변경

```java
  double getCharge() {
    double result = 0;
    switch (getMovie().getPriceCode()) {
      case Movie.REGULAR:
        result += 2;
        if (getDaysRented() > 2)
          result += (getDaysRented() - 2) * 1.5;
        break;
      case Movie.NEW_RELEASE:
        result += getDaysRented() * 3;
        break;
      case Movie.CHILDRENS:
        result += 1.5;
        if (getDaysRented() > 3)
          result += (getDaysRented() - 3) * 1.5;
        break;
    }
    return result;
  }
```

- `Customer.amountFor()` 메서드 내용을 수정하여 새 메서드로 처리를 넘기게 한다.

```java
  private double amountFor(Rental aRental) {
    return aRental.getCharge();
  }
```

- 테스트 코드가 잘 작동되는 지 확인한다.
- 기존 메서드 참조 부분을 찾아 새 메서드 참조로 수정한다.

```java
  // 비디오 종류별 대여료 계산
  thisAmount = each.getCharge();
```

- 기존 Customer.amountFor() 메서드는 삭제한다.

> 만일 메서드가 `public`이고 다른 클래스의 인터페이스를 수정하지 않아야 한다면,  기존 메서드가 새 메서드로 처리를 넘기게 놔둔다.



### 임시변수를 메서드 호출로 전환

- 임시변수 사용의 단점
  - **임시변수가 많으면 불필요하게 많은 매개변수를 전달하게 된다.**
  - 임시변수의 용도가 직관적이지 않다.
  - 성능이 떨어진다.
- 해결방안: 메서드 호출 방법 사용
  - 효과적인 최적화 가능

- thisAmount 변수는 each.charge() 메서드의 결과를 저장하는 데만 사용되고 그 후엔 사용되지 않는다. 따라서 임시변수 메서드 호출로 전환 기법을 사용하여 thisAmount 변수를 삭제한다.

```java
  public String statement() {
    double totalAmount = 0;
    int frequentRenterPoints = 0;
    Enumeration rentals = _rentals.elements();
    String result = getName() + " 고객님의 대여 기록\n";
    while (rentals.hasMoreElements()) {
      Rental each = (Rental) rentals.nextElement();

      frequentRenterPoints++;
      if ((each.getMovie().getPriceCode() == Movie.NEW_RELEASE) && each.getDaysRented() > 1)
        frequentRenterPoints++;

      // thisAmount를 each.getCharge()로 대체
      result += "\t" + each.getMovie().getTitle() + "\t" + String.valueOf(each.getCharge()) + "\n";
      totalAmount += each.getCharge();
    }
    result += "누적 대여료: " + String.valueOf(totalAmount) + "\n";
    result += "적립 포인트: " + String.valueOf(frequentRenterPoints);

    }
    return result;
  }
```



## 적립 포인트 계산을 메서드로 빼기

- 메서드 추출 이전

```java
//Customer.statement() 메서드

// 적립 포인트를 1 포인트 증가
frequentRenterPoints++;

// 최신물을 이틀 이상 대여하면 보너스 포인트 지급
if ((each.getMovie().getPriceCode() == Movie.NEW_RELEASE) && each.getDaysRented() > 1)
	frequentRenterPoints++;
```

- 메서드 추출 이후

```java
//Customer.statement() 메서드

frequentRenterPoints = each.getFrequentRenterPoints();
```

```java
//Rental 클래스
int getFrequentRenterPoints () {
    if ((getMovie().getPriceCode() == Movie.NEW_RELEASE) && getDaysRented() > 1)
        return 2;
    else
        return 1;
}
```



## 임시변수 없애기

- 임시변수를 쿼리 메서드 호출로 전환

> 쿼리 메서드(Query Method)란? 필요한 값을 반환하고자 호출되는 메서드

- 쿼리 메서드 추출 전 `totalAmount`, `frequentRentalPoints` 임시변수 사용 코드

```java
  public String statement() {
    double totalAmount = 0;
    int frequentRenterPoints = 0;
    Enumeration rentals = _rentals.elements();
    String result = getName() + " 고객님의 대여 기록\n";
    while (rentals.hasMoreElements()) {
      Rental each = (Rental) rentals.nextElement();

      // 적립 포인트를 1 포인트 증가
      frequentRenterPoints = each.getFrequentRenterPoints();

      result += "\t" + each.getMovie().getTitle() + "\t" + String.valueOf(each.getCharge()) + "\n";
      totalAmount += each.getCharge();
    }
    result += "누적 대여료: " + String.valueOf(totalAmount) + "\n";
    result += "적립 포인트: " + String.valueOf(frequentRenterPoints);

    }
    return result;
  }
```



- 쿼리 메서드(`getTotalCharge()`, `getFrequentRenterPoints()`) 추출 후 

```java
  public String statement() {
    int frequentRenterPoints = 0;
    Enumeration rentals = _rentals.elements();
    String result = getName() + " 고객님의 대여 기록\n";
    while (rentals.hasMoreElements()) {
      Rental each = (Rental) rentals.nextElement();

      frequentRenterPoints = each.getFrequentRenterPoints();

      result += "\t" + each.getMovie().getTitle() + "\t" + String.valueOf(each.getCharge()) + "\n";
    }
    result += "누적 대여료: " + String.valueOf(getTotalCharge()) + "\n";
    result += "적립 포인트: " + String.valueOf(frequentRenterPoints);

    return result;
  }

  private double getTotalCharge() {
    double result = 0;
    Enumeration rentals = _rentals.elements();
    while (rentals.hasMoreElements()) {
      Rental each = (Rental) rentals.nextElement();
      result += each.getCharge();
    }
    return result;
  }
```

## 리팩토링 효과
- htmlStatement()메서드를 추가한다면, 계산 처음의 statement 메서드에 들어 있던 계산 코드를 전부 재사용할 수 있다. 
- statement의 바꿀 수 있는 지역변수를 메서드 호출로 대체하였기 때문에 많은 수정 없이 성공적으로 메서드를 복제하여 비슷한 기능을 하는 메서드를 만들 수 있었다.

```java
  public String htmlStatement() {
    Enumeration rentals = _rentals.elements();
    String result = "<h1><em>" + getName() + " 고객님의 대여 기록</em></h1><p>\n";
    while (rentals.hasMoreElements()) {
      Rental each = (Rental) rentals.nextElement();

      result += each.getMovie().getTitle() + ": " + String.valueOf(each.getCharge()) + "<br>\n";

    }
    result += "<p>누적 대여료: <em>" + String.valueOf(getTotalCharge()) + "</em><p>\n";
    result += "적립 포인트: <em>" + String.valueOf(getTotalFrequentRenterPoints()) + "</em><p>";

    return result;
  }
```
