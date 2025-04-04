# DOM(Document Object Model)
DOM(Document Object Model)은 HTML 문서 구조를 객체 형태로 표현한 것이다.

HTML 문서 구조는 `<html>`, `<head>`, `<body>`, `<div>`와 같은 다양한 태그들로 구성되어 있다.  

![](https://i.imgur.com/qwl1D41.png)

이러한 HTML 문서를 객체로 표현한 것이 바로 DOM이며,  
JavaScript와 같은 프로그래밍 언어는 이 DOM을 통해 문서의 구조, 스타일, 내용 등을 동적으로 변경할 수 있다.

결국 DOM은 웹페이지와 프로그래밍언어 간 연결을 도와주는 징검다리 역할을 한다.

![](https://static.tosspayments.com/docs/glossary/dom-diagram.png)

## DOM의 계층 구조
DOM은 트리 형태인 계층 구조를 가지고 있다.

이 트리를 구성하는 각 객체를 노드(Node)라고 한다.

브라우저가 생성하는 최상위 루트 노드는 `document`이며,   
그 자식 노드인 `html`은 HTML 문서 구조의 루트 요소이다.   
html 노드 아래에는 여러 개의 요소 노드들이 연결되어, 전형적인 트리 구조를 구성하고 있다.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5a/DOM-model.svg/500px-DOM-model.svg.png?20190523145648)

Reference :
* [Wikipedia](https://ko.wikipedia.org/wiki/%EB%AC%B8%EC%84%9C_%EA%B0%9D%EC%B2%B4_%EB%AA%A8%EB%8D%B8)  
* [Toss](https://docs.tosspayments.com/resources/glossary/dom)




