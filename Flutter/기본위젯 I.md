# 목차
- [목차](#목차)
- [기본위젯 I](#기본위젯-i)
  - [화면 배치 위젯](#화면-배치-위젯)
    - [Container](#container)
      - [주요 속성](#주요-속성)
    - [Column, Row](#column-row)
      - [주요 속성](#주요-속성-1)
    - [Stack](#stack)
      - [주요 속성](#주요-속성-2)
    - [SingleChildScrollView](#singlechildscrollview)
      - [주요 속성](#주요-속성-3)
    - [ListView, ListTile](#listview-listtile)
      - [주요 속성](#주요-속성-4)
    - [GridView](#gridview)
      - [주요 속성](#주요-속성-5)
    - [PageView](#pageview)
    - [Tab 기반 UI](#tab-기반-ui)
  - [위치, 정렬, 크기 위젯](#위치-정렬-크기-위젯)
    - [Center](#center)
    - [Padding](#padding)
    - [Align](#align)
      - [주요 속성](#주요-속성-6)
    - [Expanded](#expanded)
      - [주요 속성](#주요-속성-7)
    - [SizedBox](#sizedbox)
      - [주요 속성](#주요-속성-8)
    - [Card](#card)
      - [주요 속성](#주요-속성-9)
  - [버튼 계열 위젯](#버튼-계열-위젯)
  - [화면 표시 위젯](#화면-표시-위젯)
    - [Text](#text)
      - [주요 속성](#주요-속성-10)
    - [Image](#image)
      - [주요 생성자](#주요-생성자)
    - [Icon](#icon)
      - [주요 속성](#주요-속성-11)
    - [Progress](#progress)
    - [CircleAvatar](#circleavatar)

# 기본위젯 I
Flutter의 기본 위젯 I

## 화면 배치 위젯

### Container
영역을 지정하고, 스타일을 입힐 수 있다.

#### 주요 속성
`child` : 위젯 안에 하나의 자식 위젯을 배치

`width`, `height` : 너비와 높이 지정

`color` : 배경색 지정

`padding` : 안쪽 여백

`margin` : 바깥쪽 여백

`alignment` : 자식 위젯 위치 지정

`decoration` : 테두리, 둥근 모서리, 그림자 등 스타일링 지정

`constraints` : 너비, 높이 제약조건


```dart
  body: Container(
    width: 100,
    height: 100,
    color: Colors.red,
    child: Text('안녕하세요'),
    padding: EdgeInsets.all(8.0),
    margin: EdgeInsets.all(8.0),
  ),
```
![](https://i.imgur.com/awyAF5H.png)

### Column, Row
자식 위젯들을 수직, 수평 방향으로 정렬시킨다.

#### 주요 속성

`children` : 위젯안에 여러 개의 자식 위젯들을 배치

`mainAxisAlignment` : 자식 위젯들의 주축을 기준으로 정렬

`crossAxisAlignment` : 자식 위젯들의 교차축을 기준으로 정렬

`mainAxisSize` : 자식 위젯들의 주축을 기준으로 얼만큼 공간을 차지할지 지정

```dart
  body: Row( 
    mainAxisAlignment: MainAxisAlignment.center,
    children: [ 
      Container(
        width: 100,
        height: 100,
        color: Colors.red,
        margin: EdgeInsets.all(8.0),
        padding: EdgeInsets.all(8.0),
        child: Text('안농'),
      ),
      Container(
        width: 100,
        height: 100,
        color: Colors.blue,
        margin: EdgeInsets.all(8.0),
        padding: EdgeInsets.all(8.0),
        child: Text('안농'),
      ),
      Container(
        width: 100,
        height: 100,
        color: Colors.yellow,
        margin: EdgeInsets.all(8.0),
        padding: EdgeInsets.all(8.0),
        child: Text('안농'),
      ),
    ],
  ),
```
![](https://i.imgur.com/SwKrJ9L.png)

### Stack
자식 위젯들을 겹칠 수 있게 배치해준다.

#### 주요 속성
`children` : 위젯안에 여러 개의 자식 위젯들을 배치

`alignment` : 자식 위젯들의 겹치는 위치를 정렬 (기본 정렬은 topStart)

`fit` : 부모 크기에 맞출지 여부

`clipBehavior` : 넘친 부분 잘라낼지 설정

```dart
  body: Stack(
    alignment: Alignment.center,
    children: [
      Container(width: 100, height: 100, color: Colors.red),
      Container(width: 80, height: 80, color: Colors.blue),
      Container(width: 60, height: 60, color: Colors.yellow),
    ],
  ),
```
![](https://i.imgur.com/08iejS0.png)

### SingleChildScrollView
전체 화면보다 데이터가 많을 때 스크롤을 제공해준다.

#### 주요 속성

`scrollDirection` : 스크롤 방향을 설정

`reverse` : 스크롤 방향을 반대로 설정

`padding` : 안쪽 여백

`physics` : 스크롤 동작 방식을 설정

```dart
  final items = List.generate(100, (i) => i).toList();

  body: SingleChildScrollView(
    physics: NeverScrollableScrollPhysics(),
    child: ListBody(
      children: items.map((i) => Text('$i')).toList()
    ),
  ),
```
![](https://i.imgur.com/RTvoxwl.png)

이미지에는 안나오지만 터치시 스크롤이 나타난다.

### ListView, ListTile

`ListView` : 스크롤이 가능한 리스트 목록을 만든다.

`ListTile` : 리스트의 개별 항목을 구성한다.

#### 주요 속성
ListView

- `children` : 리스트 내부 배치할 위젯 목록

- `scrollDirection` : 스크롤 방향

- `shrinkWrap` : 리스크 크기를 자동 조절 여부

- `physics` : 스크롤 동작 방식을 설정

ListTile

- `title` : 텍스트

- `subtitle` : 추가 설명 텍스트

- `leading` : 왼쪽에 표시할 아이콘 또는 위젯

- `trailing` : 오른쪽에 표시할 아이콘 또는 위젯

- `onTap` : 클릭 이벤트

```dart
  body: ListView(
    scrollDirection: Axis.vertical,
    children: [
      ListTile(
        leading: Icon(Icons.home),
        title: Text('home'),
        trailing: Icon(Icons.navigate_next),
        onTap: () {},
      ),
      ListTile(
        leading: Icon(Icons.event),
        title: Text('Event'),
        trailing: Icon(Icons.navigate_next),
        onTap: () {},
      ),
      ListTile(
        leading: Icon(Icons.camera),
        title: Text('Camera'),
        trailing: Icon(Icons.navigate_next),
        onTap: () {},
      ),
    ],
  ),
```
![](https://i.imgur.com/z5qWya4.png)

### GridView

리스트를 그리드 형식으로 표시해준다.

#### 주요 속성
`crossAxisCount` : GridView.count에서 사용하고, 고정된 열 수를 지정한다.

```dart
  body: GridView.count( // 열 수를 지정하여 그리드 형태로 표시하는 위젯
    crossAxisCount: 2,
    children: [
      Container(color: Colors.red, margin: EdgeInsets.all(8.0),),
      Container(color: Colors.blue, margin: EdgeInsets.all(30.0),),
      Container(color: Colors.yellow,),
      Container(color: Colors.green, margin: EdgeInsets.all(50.0),),
    ],
  ),
```
![](https://i.imgur.com/fR1uT0q.png)

### PageView

페이지 단위로 스크롤 하여 화면을 전환해주는 기능을 제공한다.

```dart
  body: PageView(
    children: [
      Container(color: Colors.red,),
      Container(color: Colors.blue,),
      Container(color: Colors.yellow,),
    ],
  ),
```

### Tab 기반 UI
AppBar, TabBar, Tab, TabBarView + 추가 위젯을 활용하여 Tab 기반 화면 전환 UI를 구성해보자.

![](https://i.imgur.com/SY77h1d.png)
```dart
  @override
  Widget build(BuildContext context) {
    return DefaultTabController(
      length: 3,
      child: Scaffold(
        appBar: AppBar(
          title: Text('Tab'),
          bottom: TabBar(
            tabs: [
              Tab(icon: Icon(Icons.tag_faces),),
              Tab(text: '메뉴2',),
              Tab(icon: Icon(Icons.info), text: '메뉴3',),
            ],
          ),
        ),
        body: TabBarView(
          children: [
            Container(color: Colors.red, margin: EdgeInsets.all(30.0),),
            Container(color: Colors.blue,),
            Container(color: Colors.yellow,),
          ],
        ),
        floatingActionButton: FloatingActionButton(
            child: Icon(Icons.add),
            onPressed: () {},
        ),
        bottomNavigationBar: BottomNavigationBar(items: [
          BottomNavigationBarItem(
            icon: Icon(Icons.home),
            label: 'Home',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.person),
            label: 'pserson',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.notifications),
            label: 'Home',
          )
        ]),
      ),
    );
  }
```
`DefaultTabController` : TabBar와 TabBarView를 연결

`appBar` : 화면 상단에 위치

`TabBar` : 여러개의 탭을 구성하고, TabBarView와 연결하여 탭별 화면을 구성

`tabs` : 탭 목록을 정의

`Tab` : TabBar 내부 개별 탭을 정의

`TabBarView` : TabBar와 연결되어 탭을 변경할 때 전환되는 화면을 표시

`floatingActionButton` : 화면에 떠 있는 원형 버튼

`bottomNavigationBar` : 앱 하단에 위치하여 여러 개의 탭을 제공

## 위치, 정렬, 크기 위젯

### Center
자식 위젯을 정중앙에 고정시킨다.

```dart
body: Center(
    child: Container(
        width: 100,
        height: 100,
        color: Colors.red,
    ),
),
```
![](https://i.imgur.com/KFEv69t.png)

### Padding
안쪽 여백을 설정한다.

`EdgeInsets` 클래스를 이용하여 여백 값을 정의한다.

margin 위젯은 별도로 없고 다른 위젯의 속성으로 사용한다.https://i.imgur.com/8omBHmK.png

```dart
body: Padding(
    padding: EdgeInsets.all(40.0),
    child: Container(
        color: Colors.red,
    ),
),
```
![](https://i.imgur.com/cX775LP.png)

### Align
자식 위젯의 위치를 정렬할때 사용한다.

#### 주요 속성
`alignment` : 자식 위젯의 위치 지정

```dart
body: Align(
    alignment: Alignment.bottomRight,
    child: Container(
        color: Colors.red,
        width: 100,
        height: 100,
    ),
),
```
![](https://i.imgur.com/KBWCSDE.png)

### Expanded
자식 위젯의 크기를 최대한 확장시킨다.

#### 주요 속성
`flex` : 공간을 차지하는 비율

```dart
body: Column(
    children: [
        Expanded( // 자식 위젯의 크리를 확장시켜주는 위젯
            flex: 2,
            child: Container(color: Colors.red),
        ),
        Expanded(
            child: Container(color: Colors.blue),
        ),
        Expanded(
            child: Container(color: Colors.yellow),
        ),
    ],
),
```
![](https://i.imgur.com/rGTp0sY.png)

### SizedBox
자식 위젯을 특정 크기로 고정시킨다.

Container는 스타일을 추가할 수 있고, SizedBox는 크기만 조정한다.

#### 주요 속성
`width` : 너비

`height` : 높이

```dart
body: SizedBox(
    width: 100,
    height: 100,
    child: Container(
        color: Colors.red,
    ),
),
```
![](https://i.imgur.com/8omBHmK.png)

### Card
자식 위젯을 카드 형태의 모양으로 스타일 할 수 있다. (그림자 깊이, 보더 종류, 모서리 라운드...)

#### 주요 속성

`shape` : 카드의 모양

`elevation` : 그림자의 깊이

```dart
body: Center(
    child: Card(
        shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(80.0),
        ),
        elevation: 60.0,
        child: SizedBox(
            width: 200,
            height: 200,
            child: Center(child: Text('내용')),
        ),
    ),
),
```
![](https://i.imgur.com/J4XW7po.png)

## 버튼 계열 위젯
`ElevatedButton` : 클릭 시 그림자 애니메이션이 생긴다.

`TextButton` : 테두리 없이 텍스트만 있는 버튼이다.

`IconButton` : 아이콘 버튼이다.

`FloatingActionButton` : 화면에 떠 있는 원형 버튼이다.

```dart
body: Column(
    children: [
        ElevatedButton(onPressed: () {}, child: Text('RaiseButton')),
        TextButton(onPressed: () {}, child: Text('TextButton')),
        IconButton(onPressed: () {}, icon: Icon(Icons.add), iconSize: 100.0,),
        FloatingActionButton(child: Icon(Icons.add), onPressed: () {}),
    ],
),
```
![](https://i.imgur.com/DctunA8.png)

## 화면 표시 위젯
### Text
텍스트를 표시하는 위젯이다.

#### 주요 속성

`style` : TextStyle을 이용하여 글꼴, 크기, 색상 등을 설정

```dart
body: Center(
    child: Text(
        'Hello World!',
        style: TextStyle(
            fontSize: 40.0,
            color: Colors.red,
            fontStyle: FontStyle.italic,
            fontWeight: FontWeight.bold,
        ),
    ),
)
```
![](https://i.imgur.com/zAtT3mq.png)

### Image
이미지를 표시하는 위젯이다.

pubspec.yaml 파일에서 assets을 설정해야 한다.

#### 주요 생성자
`.asset` : 앱 내부 assets 폴더에서 이미지를 불러옴

`.network` : 인터넷 URL에서 이미지를 불러옴

`.file` : 로컬 파일에서 이미지를 불러옴

`.memory` : 메모리에서 이미지를 불러옴

```dart
body: Image.asset('assets/flutter.png'),
```
![](https://i.imgur.com/uJHLeAl.png)

### Icon
아이콘을 표시하는 위젯이다.

#### 주요 속성
`icon` : 표시할 아이콘을 설정한다.

`size` : 아이콘 크기를 지정한다.

`color` : 아이콘 색상을 지정한다.

```dart
body: Icon(
    Icons.home,
    color: Colors.red,
    size: 600.0,
),
```
![](https://i.imgur.com/7zoo74I.png)

### Progress
로딩을 표시한다.

`CircularProgressIndicator()` : 원형 진행 로딩, 작업이 얼마나 진행되었는지 알 수 없을 때 사용

`LinearProgressIndicat()` : 직석현 진행 로딩, 진행률을 퍼센트로 나타낼 수 있을 때 사용

```dart
body: Column(
    children: [
        CircularProgressIndicator(),
        LinearProgressIndicator()
    ],
),
```
![](https://i.imgur.com/SiFlQT4.png)

### CircleAvatar
원형 프로필 이미지나, 아이콘을 표시한다.
```dart
body: Center(
    child: CircleAvatar(
        child: Icon(Icons.person),
    ),
),
```
![](https://i.imgur.com/dKIx2hf.png)