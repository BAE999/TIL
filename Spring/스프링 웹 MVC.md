# 목차
- [목차](#목차)
- [스프링 웹 MVC](#스프링-웹-mvc)
  - [기본 구성 요소](#기본-구성-요소)
  - [프로젝트 구조](#프로젝트-구조)
  - [프로젝트 실행과정](#프로젝트-실행과정)
  - [프로젝트 환경 설정](#프로젝트-환경-설정)
    - [web.xml](#webxml)
  - [Spring MVC 환경 설정](#spring-mvc-환경-설정)
    - [root-context.xml](#root-contextxml)
    - [servlet-context.xml](#servlet-contextxml)
      - [컨트롤러 매핑 설정](#컨트롤러-매핑-설정)
      - [자바 클래스의 Bean 객체 설정](#자바-클래스의-bean-객체-설정)
      - [정적 리소스 설정](#정적-리소스-설정)
      - [뷰 매핑 설정](#뷰-매핑-설정)
  - [Maven 환경 설정](#maven-환경-설정)
    - [의존성 라이브러리 정보](#의존성-라이브러리-정보)
    - [Maven Scope](#maven-scope)

# 스프링 웹 MVC
스프링 웹 MVC는 스프링이 제공하는 웹 애플리케이션 개발 전용 프레임워크다.

모델(Model) - 뷰(View) - 컨트롤러(Controller) MVC 패턴을 사용하고 있다.
- 웹 애플리케이션의 모델, 뷰, 컨트롤러 사이의 의존 관계를 스프링 컨테이너가 관리

- 스프링이 제공하는 많은 기능을 확장하여 웹 애플리케이션 구축이 가능

![](https://i.imgur.com/dsRTwXp.png)

## 기본 구성 요소
`디스패처서블릿(DispatcherServlet)`
- 웹으로부터 요청을 전달받음
- 요청을 컨트롤러에 전달하고, 컨트롤러가 반환한 결과 값을 뷰에 전달하여 사용자에게 보여줄 응답을 생성 하는 등의 모든 절차(흐름)을 담당

`핸들러 매핑(HandlerMapping)`
- 클라이언트의 요청 URL에 대해 어떤 컨트롤러가 처리할지를 결정

`핸들러 어댑터(HandlerAdapter)`
- 핸들러 매핑 클래스에 의해 결정된 컨트롤러를 호출

`컨트롤러(Controller)`
- 클라이언트의 요청을 처리한 뒤 결과를 반환
- 응답 결과에서 보여줄 데이터를 모델에 담아서 뷰로 전달

`모델 & 뷰(ModelAndView)`
- 컨트롤러가 처리한 결과 정보 및 뷰 선택에 필요한 정보를 담는 객체

`뷰 리졸버(ViewResolver)`
- 컨트롤러의 처리 결과를 사용자에게 보여줄 뷰를 결정

`뷰(View)`
- 컨트롤러의 처리 결과 화면을 생성
- JSP를 비롯한 다양한 뷰 템플릿 엔진이 사용됨
- 클라이언트에 요청 처리 결과를 전송

## 프로젝트 구조
![](https://i.imgur.com/O7DGYbY.png)

## 프로젝트 실행과정
![](https://i.imgur.com/XR3Oxe1.png)

1. `웹 브라우저` : http://localhost:8080 호출

2. `Tomcat : web.xml` : web.xml 파일에 설정된 DispatcherServlet이 웹 요청 URL을 제어

3. `Spring : servlet-context.xml` : servlet-context.xml 파일에서 컨트롤러의 클래스를 검색

4. `Spring : HomeController.java` : 컨트롤러는 웹 요청 URL을 처리한 후 결과를 출력할 뷰 이름을 DispatcherServlet에 반환

5. `Spring : servlet-context.xml` : 컨트롤러에서 반환한 뷰 이름으로 뷰를 검색

6. `Tomcat + Spring : home.jsp` : 처리 결과가 포함된 뷰를 DispatcherServlet에 반환하고 최종 결과를 브라우저에 전달

## 프로젝트 환경 설정

### web.xml
- 웹 프로젝트의 배치 설명자/기술서(deployment descriptor)
- 웹 프로젝트가 배포되는 데 이용되는 XML 형식의 자바 웹 애플리케이션 환경 설정을 담당
- Servlet 3.0 이상에서는 Annotation으로 대체 가능

```html
<?xml version="1.0" encoding="UTF-8"?>
<web-app metadata-complete="false" version="6.0"
  xmlns="https://jakarta.ee/xml/ns/jakartaee"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee
    https://jakarta.ee/xml/ns/jakartaee/web-app_6_0.xsd">

  <!-- 서블릿 컨텍스트가 시작될 때 전달할 파라메터를 설정 -->
  <context-param> ... </context-param>

  <!-- 서블릿 컨테이너의 각종 이벤트를 수신/처리할 이벤트 리스너를 등록 -->
  <listener> ... </listener>

  <!-- 사용자의 요청을 처리할 서블릿을 등록 -->
  <servlet> ... </servlet>

  <!-- 서블릿을 사용자 요청 URL과 매핑 -->
  <servlet-mapping> ... </servlet-mapping>

  <!-- 서블릿 호출 전처리 필터를 등록 -->
  <filter> ... </filter>

  <!-- 필터를 요청 URL과 매핑 -->
  <filter-mapping> ... </filter-mapping>

</web-app>
```

![](https://i.imgur.com/xJ97lhI.png)

<!-- omit in toc -->
### 루트 컨텍스트 설정
- 스프링 애플리케이션 전체에서 공유할 수 있는 루트 컨텍스트 설정파일의 위치를 지정
  - 공통 빈(Service, Repository(DAO), DB, log 등)을 설정
  - 주로 View 지원을 제외한 Bean을 설정
- `<context-param>` 요소를 이용하여 설정파일의 위치를 지정
- 네임 스페이스와 스키마 선언(xmlns, xmlns:xsi, xsi:schemaLocation)
  - 웹 애플리케이션 설정 파일의 문법검증을 위한 네임스페이스와 스키마 선언

```html
...
<context-param>
  <param-name>contextConfigLocation</param-name>
  <param-value>/WEB-INF/spring/root-context.xml</param-value>
</context-param>
<listener>
  <listener-class>
    org.springframework.web.context.ContextLoaderListener
  </listener-class>
</listener>
...
```

<!-- omit in toc -->
### 서블릿 컨텍스트 설정
- DispatcherServlet이 모든 요청을 받을 수 있도록 설정
  - DispatcherServlet은 요청 URL을 처리할 컨트롤러에 처리를 위임함
  - `<init-param>` 요소로 설정 파일의 위치를 지정
- `<servlet>` 요소를 이용하여 설정
- `<servlet-mapping>` 요소를 이용하여 DispatcherServlet이 처리할 사용자 요청 URL 패턴을 설정
- `<load-on-startup>`
  - 서블릿을 로딩하는 순서를 설정
  - 서블릿이 여러 개일 때 로딩되는 순서를 제어할 수 있음

```html
<servlet>
  <servlet-name>appServlet</servlet-name>
  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  <init-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>/WEB-INF/spring/servlet-context.xml</param-value>
  </init-param>
  <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
  <servlet-name>appServlet</servlet-name>
  <url-pattern>/</url-pattern>
</servlet-mapping>
```

## Spring MVC 환경 설정
`root-context.xml`
- 뷰(JSP 웹페이지)와 관련 없는 Bean 객체를 설정
- 서비스, 저장소, 데이터베이스, 로그 등 웹 애플리케이션의 공유자원을 위한 루트 컨텍스트 설정

`servlet-context.xml`
- 뷰(JSP 웹페이지)와 관련 있는 Bean 객체를 설정
- 컨트롤러, MultipartResolver, Interceptor, URI와 관련된 설정

![](https://i.imgur.com/v2bzvAc.png)

### root-context.xml
- 다른 웹 컴포넌트들과 공유하는 자원들을 설정
- 뷰와 관련되지 않은 Service, Repository(DAO), DB등 객체를 정의
- 공통 Bean을 설정하며 주로 뷰 지원을 제외한 Bean 객체를 설정

```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```

### servlet-context.xml
- 웹 요청을 직접 처리할 컨트롤러의 매핑을 설정(HandlerMapping) 하거나 뷰를 어떻게 처리할지 설정(ViewResolver)

```html
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/mvc"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xmlns:beans="http://www.springframework.org/schema/beans"
             xmlns:context="http://www.springframework.org/schema/context"
             xsi:schemaLocation="http://www.springframework.org/schema/mvc
                http://www.springframework.org/schema/mvc/spring-mvc.xsd
                http://www.springframework.org/schema/beans
                http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/context
                http://www.springframework.org/schema/context/spring-context.xsd">

  <!-- 컨트롤러 매핑 설정: 스프링 컴포넌트를 어노테이션 기반으로 관리하도록 설정 -->
  <annotation-driven/>

  <!-- 자바 클래스의 Bean 객체 설정: Bean을 XML에 일일이 선언하지 않고 기준이 되는 패키지 하위의 모든 클래스 파일을 검색하여 Bean 객체를 자동 등록 -->
  <context:component-scan base-package="com.springmvc.*" />

  <!-- 정적 리소스 파일들을 찾을 기본 경로 설정 -->
  <resources mapping="/resources/**" location="/resources/" />

  <!-- 뷰 매핑 설정 -->
  <beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">

  <!-- View 이름으로 View를 찾을 때 Controller에서 반환한 View 이름 앞/뒤에 붙여줄 문자로 최종 View의 경로를 만들 때 사용 -->
    <beans:property name="prefix" value="/WEB-INF/views/" />
    <beans:property name="suffix" value=".jsp" />
  </beans:bean>
</beans:beans>
```

#### 컨트롤러 매핑 설정
요청 URL을 처리하는 컨트롤러 매핑 설정

`<annotation-driven>`
- @Controller, @RequestMapping 같은 어노테이션을 사용할 때 필요한 빈 객체들을 자동으로 등록
- 핸들러 매핑과 핸들러 어댑터의 빈 객체도 대신 등록됨

```html
<annotation-driven/>
```

`RequestMappingHandlerMapping`
- 웹 요청 URL과 컨트롤러를 매핑시켜주는 핸들러 매핑 클래스
- 클라이언트의 요청 URL을 어떤 컨트롤러가 처리할지를 결정

`RequestMappingHandlerAdapter`
- 핸들러 매핑 클래스에 의해 결정된 컨트롤러를 호출하는 핸들러 어댑터 클래스
- DispatcherServlet의 처리 요청을 변환해서 컨트롤러에게 전달하고 컨트롤러의 응답 결과를 DispatcherServlet이 요구하는 형식으로 변환

```html
<beans:bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>

<beans:bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"/>
```

#### 자바 클래스의 Bean 객체 설정
Spring MVC에서 사용할 Bean 객체를 XML에 등록하지 않고 설정된 패키지 하위 경로의 모든 클래스를 검색하여 자동 등록

```html
<context:component-scan base-package="com.springmvc.*" />
```

```html
<beans:bean class="com.springmvc.controller.HomeController"/>
```

자동으로 감지하는 어노테이션

- @Component, @Repository, @Service, @Controller, @RestController, @ControllerAdvice, @Configuration

구성요소 클래스에서 자동으로 활성화 하는 어노테이션

- @Required, @Autowired, @PostConstruct, @PreDestroy, @Resource, @PersistenceContext, @PersistenceUnit

#### 정적 리소스 설정
`정적 리소스`
- 브라우저의 요청 리소스가 이미 만들어져 있어 그대로 응답가능한 자원

`정적 리소스 설정`
- 이미지, js, css 파일과 같은 정적 리소스를 웹 브라우저에 효율적으로 전달하도록 최적화된 캐시 헤더와 함께 제공하기 위한 핸들러를 구성
- Spring의 리소스 처리를 통해 도달 가능한 모든 경로에서 리소스를 제공

```html
<resources mapping="/resources/**" location="/resources/" />
```

#### 뷰 매핑 설정
브라우저에 요청 결과를 전달할 View 관련 설정
속성
- prefix
  - View의 위치를 결정할 때 컨트롤러에서 반환한 View 이름 앞에 붙일 접두사
- suffix
  - View의 위치를 결정할 대 컨트롤러에서 반환한 View 이름 뒤에 붙일 접미사
  
![](https://i.imgur.com/wNYtFtM.png)

## Maven 환경 설정
`Maven`
- Java 프로젝트들을 위한 빌드 자동화 도구
- 소프트웨어의 종속성과 빌드하는 방법을 관리
- 종속성 관리
  - 프로젝트 내에 사용할 라이브러리와 라이브러리가 참조하는 다른 라이브러리까지 프로젝트에서 참조할 수 있도록 지원
  - 라이브러리는 네트워크를 통해 자동으로 다운로드됨
- 빌드관리
  - 프로젝트를 배포형식(jar, war)에 맞게 패키징
  - 종속성이 있는 라이브러리리는 패키징 시 포함시킴

`pom.xml`
- POM(Project Object Model)
- XML 형식으로 Maven 프로젝트에 관련된 내용을 설정함

### 의존성 라이브러리 정보
dependencies 요소 안에 dependency 요소로 정의

- groupId: 라이브러리가 속한 그룹
- artifactId: 라이브러리의 ID
- version: 라이브러리의 버전
- scope: 라이브러리가 사용되는 범위

Maven은 설정된 종속성 라이브러리를 기본적으로 Maven Central 사이트를 통해 다운로드함

[mvnrepository](https://mvnrepository.com)

```html
...
<dependencies>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>6.2.1</version>
    <scope>compile</scope>
  </dependency>
...
</dependencies>
...
```


### Maven Scope
`종속성 범위(Dependency scope)`
- 종속성의 전이성을 제한하고 종속성이 클래스 경로에 포함되는 시점을 결정

`종속성 범위 종류`
- compile
  - 기본 범위로 설정하지 않으면 적용됨
  - 프로젝트의 모든 클래스 경로에서 사용할 수 있음
  - 종속 프로젝트로 전파됨

- provided
  - 컴파일과 유사
  - JDK 또는 컨테이너가 런타임에 종속성을 제공
  - 컴파일 및 테스트 단계에 클래스 경로에 추가되지만 런타임 단계에는 추가되지 않음

- runtime
  - 컴파일에 필요하지 않지만 실행에는 필요한 종속성을 의미
  - Maven은 런타임 및 테스트 클래스 경로에 이 범위가 있는 종속성을 포함하지만 컴파일에는 포함하지 않음

- test
  - 테스트 컴파일 및 테스트 실행 단계에서만 사용할 수 있음

- system
  - provided와 유사하지만 명시적으로 포함된 JAR를 제공해야 함
  - 항상 사용가능 하며 Maven Repository에서 조회하지 않음

- import
  - `<dependencyManagement>` 섹션의 pom 유형의 종속성에서만 지원됨
  - 종속성이 지정된 POM의 `<dependencyManagement>` 섹션에 있는 종속성 목록으로 대체됨