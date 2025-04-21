# 목차
- [목차](#목차)
- [Spring 웹 MVC의 계층적 구조](#spring-웹-mvc의-계층적-구조)
  - [계층적 구조](#계층적-구조)
  - [구현 과정](#구현-과정)
    - [도메인 객체](#도메인-객체)
    - [퍼시스턴스 계층](#퍼시스턴스-계층)
    - [서비스 계층](#서비스-계층)
    - [프레젠테이션 계층](#프레젠테이션-계층)

# Spring 웹 MVC의 계층적 구조
전체 애플리케이션의 관심사를 계층별로 분리
- 각 계층을 느슨하게 결합
- 계층 간에 유연하게 동작할 수 있도록 애플리케이션을 구조화

웹 애플리케이션을 좀 더 효율적으로 개발하고 개발한 후 유지 보수를 쉽게 하기 위해 시스템 구조를 계층화 하여 개발

계층적 구조 없이 한 곳에서 모든 작업을 처리하면 많은 문제가 발생
- 코드의 복잡성 증가
- 유지 보수의 어려움
- 유연성 부족
- 중복 코드의 증가
- 낮은 확장성 등

## 계층적 구조
![](https://i.imgur.com/Pj2vbn7.png)

도메인 객체(Domain Object)
- 정보를 저장하는 데이터 모델

퍼시스턴스(영속) 계층(Persistence Layer) = 데이터 액세스 계층
- 데이터베이스나 파일에 접근하여 데이터를 처리

서비스 계층(Service Layer) = 비지니스 계층
- 애플리케이션이 제공하는 포괄적인 서비스들을 표현
- 브라우저에서 요청한 데이터를 처리하기 위해 퍼시스턴스 계층을 호출
- 프레젠테이션 계층과 퍼시스턴스 계층 사이의 연결

프레젠테이션 계층(Presentation Layer) = 표현 계층
- 애플리케이션과 사용자(브라우저)와의 최종 접점
- 사용자로부터 데이터를 입력 받거나 데이터를 출력하는 계층
  - 사용자의 요청을 애플리케이션으로 전달하거나 처리된 결과를 사용자에게 보여줌

## 구현 과정
![](https://i.imgur.com/tlaYsez.png)

먼저 계층간 데이터를 전달하기 위한 데이터를 정의

도메인 객체 -> 퍼시스턴스 계층 -> 서비스 계층 -> 프레젠테이션 계층(컨트롤러 -> 뷰) 순으로 개발

### 도메인 객체
애플리케이션에서 다른 계층과 상호 데이터 교환을 위한 데이터 객체

도메인 객체는 데이터 모델로 필요한 속성(클래스와 필드)들을 정의하고 각 속성에 setter(), getter() 메소드를 생성

```java
package com.springmvc.domain;

  public class Book {
    private String bookId; // 도서ID
    private String name; // 도서명
    private int unitPrice; // 가격
    private String author; // 저자
    private String description; // 설명
    private String publisher; // 출판사
    private String category; // 분류
    private long unitsInStock; // 재고수
    private String releaseDate; // 출판일(월/년)
    private String condition; // 신규 도서 or 중고 도서 or 전자책

    public Book() {
      super();
    }

    public Book(String bookId, String name, int unitPrice) {
      super();
      this.bookId = bookId;
      this.name = name;
      this.unitPrice = unitPrice;
    }

    // getters and setters ...  

}
```

### 퍼시스턴스 계층
데이터 엑세스(접근) 계층

특정 클래스에 @Repository를 선언하면 선언된 클래스는 **저장소 클래스**라는 의미를 가짐

```java
package com.springmvc.repository;

import java.util.List;
import com.springmvc.domain.Book;

public interface BookRepository {
  List<Book> getAllBookList();
}
```

```java
package com.springmvc.repository;

@Repository
public class BookRepositoryImpl implements BookRepository {
  private List<Book> listOfBooks = new ArrayList<>();

  public BookRepositoryImpl() {
    Book book1 = new Book("ISBN1234", "C# 교과서", 30000);
    book1.setAuthor("박용준");
    book1.setDescription("C# 교과서』는 생애 첫 프로그래밍 언어로 C#을 시작하는 독자를 대상으로 한다.
      특히 응용 프로그래머를 위한 C# 입문서로, C#을 사용하여 게임(유니티), 웹, 모바일, IoT 등을 개발할 때 필요한
      C# 기초 문법을 익히고 기본기를 탄탄하게 다지는 것이 목적이다.");
    book1.setPublisher("길벗");
    book1.setCategory("IT전문서");
    book1.setUnitsInStock(1000);
    book1.setReleaseDate("2020/05/29");

    Book book2 = new Book("ISBN1235", "Node.js 교과서", 36000);
    book2.setAuthor("조현영");
    book2.setDescription("이 책은 프런트부터 서버, 데이터베이스, 배포까지 아우르는 광범위한 내용을 다
      룬다. 군더더기 없는 직관적인 설명으로 기본 개념을 확실히 이해하고, 노드의 기능과 생태계를 사용해보면서 실제
      로 동작하는 서버를 만들어보자. 예제와 코드는 최신 문법을 사용했고 실무에 참고하거나 당장 적용할 수 있다.");
    book2.setPublisher("길벗");
    book2.setCategory("IT전문서");
    book2.setUnitsInStock(1000);
    book2.setReleaseDate("2020/07/25");

    Book book3 = new Book("ISBN1236", "어도비 XD CC 2020", 25000);
    book3.setAuthor("김두한");
    book3.setDescription("어도비 XD 프로그램을 통해 UI/UX 디자인을 배우고자 하는 예비 디자이너의 눈높
      이에 맞게 기본적인 도구를 활용한 아이콘 디자인과 웹&앱 페이지 디자인, UI 디자인, 앱 디자인에 애니메이션과 인
      터랙션을 적용한 프로토타이핑을 학습합니다.");
    book3.setPublisher("길벗");
    book3.setCategory("IT활용서");
    book3.setUnitsInStock(1000);
    book3.setReleaseDate("2019/05/29");

    this.listOfBooks.add(book1);
    this.listOfBooks.add(book2);
    this.listOfBooks.add(book3);
  }

  @Override
  public List<Book> getAllBookList() {
  return this.listOfBooks;
  }
}
```

### 서비스 계층
서비스 계층은 애플리케이션이 제공하는 포괄적인 서비스들을 처리하는 계층

프레젠테이션 계층과 퍼시스턴스 계층 사이를 연결

특정 ㅋ늘래스에 @Service를 선언할 경우 선언된 클래스는 **서비스 클래스**라는 의미를 가짐

```java
package com.springmvc.service;

import java.util.List;
import com.springmvc.domain.Book;

public interface BookService {
  List<Book> getAllBookList();
}
```

```java
package com.springmvc.service;
...

@Service
public class BookServiceImpl implements BookService {
  @Autowired
  private BookRepository bookRepository;

  @Override
  public List<Book> getAllBookList() {
    return this.bookRepository.getAllBookList();
  }
}
```

### 프레젠테이션 계층

<!-- omit in toc -->
#### 컨트롤러(Controller)
- 브라우저의 요청을 처리하는 자바 클래스
- 특정 클래스에 @Controller를 선언하면 선언된 클래스는 **컨트롤러**라는 의미를 가짐

```java
package com.springmvc.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import com.springmvc.domain.Book;
import com.springmvc.service.BookService;

@Controller
public class BookController {
  @Autowired
  private BookService bookService;

  @RequestMapping(value="/books", method=RequestMethod.GET)
  public String requestBookList(Model model) {
    List<Book> list = bookService.getAllBookList();
    model.addAttribute("bookList", list);

    return "books";
  }
}
```

<!-- omit in toc -->
#### 뷰(View)
- 브라우저의 요청 처리 결과를 출력하는 웹 페이지(JSP)
- 자바 서버 페이지 표준 태그 라이브러리(Java Serer Pages Standard Tag Library:JSTL)를 적용하여 웹 페이지를 표현

<!-- omit in toc -->
#### 모델(Model)
- JSP 웹 페이지에 출력할 데이터 형식

```java
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="jakarta.tags.core"%>
<%@ taglib prefix="fn" uri="jakarta.tags.functions"%>
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>도서 목록</title>
  <link href="<c:url value="/resources/css/bootstrap.css"/>" rel="stylesheet">
</head>
<body>
  <nav class="navbar navbar-expand navbar-dark bg-dark">
    <div class="container">
      <div class="navbar-header">
        <a class="navbar-brand" href="./home">Home</a>
      </div>
    </div>
  </nav>
  <div class="jumbotron">
    <div class="container">
      <h1 class="display-3">도서 목록</h1>
    </div>
  </div>
  <div class="container">
    <div class="row" align="center">
      <c:forEach items="${bookList}" var="book">
        <div class="col-md-4">
          <h3>${book.name}</h3>
          <p>${book.author}
          <br> ${book.publisher} | ${book.releaseDate}
          <p align=left>${fn:substring(book.description, 0, 100)}...
          <p>${book.unitPrice}원
        </div>
      </c:forEach>
    </div>
    <hr>
    <footer>
      <p>&copy; BookMarket</p>
    </footer>
  </div>
</body>
</html>
```
