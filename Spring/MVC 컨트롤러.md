# 목차
- [목차](#목차)
- [컨트롤러](#컨트롤러)
  - [구현 과정](#구현-과정)
  - [`<context:component-scan>` 요소로 컨트롤 등록](#contextcomponent-scan-요소로-컨트롤-등록)
- [요청 경로 매핑](#요청-경로-매핑)
  - [@RequestMapping](#requestmapping)
    - [클래스 수준 @ResuestMapping](#클래스-수준-resuestmapping)
    - [메소드 수준 @RequestMapping](#메소드-수준-requestmapping)
- [요청 처리 메소드와 모델](#요청-처리-메소드와-모델)
  - [요청 처리 메소드](#요청-처리-메소드)
  - [모델과 뷰](#모델과-뷰)
    - [ModelAndView](#modelandview)

# 컨트롤러
웹 브라우저에서 요청한 사용자의 요청을 구현된 메소드에서 처리하고 그 결과를 뷰에 전달하는 스프링의 빈(Bean) 클래스

![](https://i.imgur.com/3f5kmQz.png)

## 구현 과정
도서 쇼핑몰에서 전체 도서 목록을 가져와 웹 브라우저에 출력하는 BookController 컨트롤러를 구현하는 과정

![](https://i.imgur.com/fXu4GIk.png)

## `<context:component-scan>` 요소로 컨트롤 등록
스프링 MVC 설정 파일(servlet-context.xml)에 빈 클래스로 등록
- @Controller 가 선언된 컨트롤러를 빈으로 자동 등록
- 자동 의존성 관리
- `<context:component-scan>` 요소 이용

  - `<context-component-scan base-package="기본 패키지 명"/>`

  - 자동으로 감지하여 빈으로 관리하는 타입
    - @Component, @Repository, @Service, @Controller, @RestController, @ControllerAdvice, @Configuration

  - 감지된 클래스에서 자동으로 활성화하는 어노테이션
    - @Required, @Autowired, @PostConstruct, @PreDestroy, @Resource, @PersistenceContext, @PersistenceUnit

# 요청 경로 매핑
## @RequestMapping
웹에서 들어온 사용자 요청을 처리하는 컨트롤러와 메소드를 매핑

클래스 및 메소드에 지정할 수 있음

@RequestMapping(value= "웹 요청 URL", method=RequestMethod.HTTP 요청형식, ...)

![](https://i.imgur.com/m7llett.png)

### 클래스 수준 @ResuestMapping
웹에서 사용자가 요청 URL에 매핑되는 컨트롤러에 선언함

기본 매핑 경로를 설정하지 않은 @RequestMapping만 선언된 요청 처리 메서드가 있을 수도 있음

```java
package com.springmvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
@RequestMapping(value="/exam02", method=RequestMethod.GET)
public class Example02Controller {
    
  @RequestMapping
  public void requestMethod() {
    System.out.println("@RequestMapping 예제입니다.");
    System.out.println("웹 요청 URL은 /exam02 입니다.");
  }

}
```

### 메소드 수준 @RequestMapping
웹에서 사용자가 요청한 URL을 처리할 컨트롤러의 메소드를 지정

method 속성의 기본 값은 GET

매핑되는 URL은
- 클래스에 적용된 @RequestMapping의 value와
- 메소드에 매핑된 @RequestMapping의 value를 결합한 값이 적용됨

![](https://i.imgur.com/u4a7cZX.png)

# 요청 처리 메소드와 모델
## 요청 처리 메소드
Spring Web MVC에서 사용자 요청을 처리하는 메소드

@RequestMapping에 설정된 요청 경로 매핑 메소드를 호출

```java
@RequestMapping(...)
public String 메소드 이름() {
  // 모델에 응답 데이터 저장
  return "뷰 이름";
}
```

## 모델과 뷰
`모델` : 사용자의 요청을 처리한 결과 데이터를 관리하고 전달

`뷰` : 처리된 결과 데이터를 웹 브라우저에 출력하는 웹 페이지 역할

![](https://i.imgur.com/lMqBNMo.png)

### ModelAndView
모델과 뷰를 하나로 관리할 수 있도록 제공되는 클래스

public ModelAndView addObject(String attributeName, @Nullable Object attribute Value)
- 역할 : 지정된 이름으로 데이터를 저장

- 매개변수 :
  - attributeName : 속성의 이름(null이 될 수 없음)
  - attributeValue : 속성의 값(null이 될 수 있음)

```java
package com.springmvc.controller;
...
@Controller
@RequestMapping("/books")
public class BookController {
  ...
  @GetMapping("/all")
  public ModelAndView requestAllBooks() {
    List<Book> list = this.bookService.getAllBookList();
    ModelAndView mav = new ModelAndView();
    mav.addObject("bookList", list);
    mav.setViewName("books");
    return mav;
  }
}
```

public void setViewName(@Nullable String viewName)
  - 역할 : ModelAndView를 위한 뷰 이름을 설정

  - 매개변수 :
    -  viewName : 뷰 이름