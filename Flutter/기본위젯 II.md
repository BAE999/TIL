# 목차
- [목차](#목차)
- [기본 위젯 II](#기본-위젯-ii)
  - [입력 위젯](#입력-위젯)
    - [TextField](#textfield)
    - [CheckBox, Switch](#checkbox-switch)
    - [Radio + ListTile](#radio--listtile)
    - [DropDownButton](#dropdownbutton)
  - [다이얼로그](#다이얼로그)
    - [AlertDialog](#alertdialog)
  - [이벤트](#이벤트)
    - [GestureDetector, InkWell](#gesturedetector-inkwell)
  - [애니메이션](#애니메이션)
    - [Hero](#hero)
  - [쿠퍼티노 디자인](#쿠퍼티노-디자인)
    - [쿠퍼티노 기본 UI](#쿠퍼티노-기본-ui)
    - [쿠퍼티노 AlertDialog](#쿠퍼티노-alertdialog)
  - [내비게이션](#내비게이션)
    - [Navigator](#navigator)
    - [Navigator + routes](#navigator--routes)
    - [페이지 생명주기](#페이지-생명주기)

# 기본 위젯 II
Flutter의 기본 위젯 II

## 입력 위젯
### TextField
사용자가 텍스트를 입력할 수 있는 입력 필드다.

<!-- omit in toc -->
### 주요 속성
`decoration` : 입력 필드의 스타일을 지정

`lableText` : 입력 필드가 비어있을 때 보여줄 텍스트, 포커스를 받거나 값이 입력되면 위로 이동

```dart
body: Column(
    mainAxisAlignment: MainAxisAlignment.center,
    children: [
        TextField(),

        SizedBox(height: 40),

        TextField(decoration: InputDecoration(labelText: '여기에 입력하세요.')),

        SizedBox(height: 40),

        TextField(
            decoration: InputDecoration(
                border: OutlineInputBorder(),
                labelText: '여기에 입력하세요.',
            ),
        ),
    ],
),
```
![](https://i.imgur.com/H6tgfxf.gif)

---

### CheckBox, Switch
`CheckBox` : 체크박스 형태로 선택한다.

`Switch` : 토글 형태로 ON / OFF 상태를 변경한다.

<!-- omit in toc -->
### 주요 속성
`value` : true, false로 현재 상태를 관리

`onChanged` : 입력 값이 변경될 때 호출되는 콜백 함수

```dart
bool isChecked = false;
bool isChecked2 = false;

body: Center(
    child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
        Checkbox(
            value: isChecked,
            onChanged: (value) {
                setState(() {
                    isChecked = value ?? false;
                });
            },
        ),
        Text('$isChecked'),

        SizedBox(height: 40),

        Switch(
            value: isChecked2,
            onChanged: (value) {
                setState(() {
                    isChecked2 = value;
                });
            },
        ),
            Text('$isChecked2'),
        ],
    ),
),
```

CheckBox의 value는 null 값을 가질 수 있기에 예외를 처리하거나, bool 타입에 null값을 허용해주면 된다.

![](https://i.imgur.com/QD1Tnlh.gif)

---

### Radio + ListTile
`Radio` : 선택 그룹 중 하나를 선택할 때 사용한다.

`ListTile` : 리스트의 개별 항목을 구성한다.

`RadioListTile` : 리스트의 개별 항목마다 가지는 영역을 클릭하여 선택할 수 있다. ListTile은 체크되는 영역만 클릭할 수 있다.

<!-- omit in toc -->
### 주요 속성
[ListTile 주요 속성](#------4)

Radio 주요속성

- `value` : true, false로 현재 상태를 관리

- `onChanged` : 입력 값이 변경될 때 호출되는 콜백 함수

- `groupValue` : 현재 선택된 값을 나타내는 변수

```dart
enum Foodkind {food1, food2, food3,}
Foodkind? foodkind = Foodkind.food1;

body: Center(
    child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
        Text('$foodkind'),
        RadioListTile(
            title: Text('한식'),
            value: Foodkind.food1,
            groupValue: foodkind,
            onChanged: (value) {
                setState(() {
                    foodkind = value;
                });
            },
        ),

        RadioListTile(
            title: Text('중식'),
            value: Foodkind.food2,
            groupValue: foodkind,
            onChanged: (value) {
                setState(() {
                    foodkind = value;
                });
            },
        ),

        RadioListTile(
            title: Text('일식'),
            value: Foodkind.food3,
            groupValue: foodkind,
            onChanged: (value) {
                setState(() {
                    foodkind = value;
                });
            },
        ),
        ],
    ),
),
```
![](https://i.imgur.com/vXQmfsB.gif)

---

### DropDownButton
여러 옵션 중 하나를 선택할 수 있도록 드롭다운 메뉴를 제공한다.

<!-- omit in toc -->
### 주요 속성
`value` : true, false로 현재 상태를 관리

`onChanged` : 입력 값이 변경될 때 호출되는 콜백 함수

`items` : 드롭다운 메뉴 항목 리스트 (DropdownMenuItem 사용)

```dart
final _valueList = ['첫 번째', '두 번째', '세 번째'];
String? _selectedValue = '첫 번째';

body: Center(
    child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
            DropdownButton(
                value: _selectedValue,
                items:
                    _valueList.map((item) {
                        return DropdownMenuItem(
                            value: item,
                            child: Text(item),
                        );
                    }).toList(),
                onChanged: (value) {
                    setState(() {
                        _selectedValue = value;
                    });
                },
            ),
            Text('선택된 항목은 `$_selectedValue` 입니다!'),
        ],
    ),
),
```
![](https://i.imgur.com/1ke4M0M.gif)

---

## 다이얼로그
### AlertDialog

사용자에게 중요한 정보를 알리고, 확인 또는 취소 등의 요청하는 팝업 창

showDialog()를 호출하면 현재 화면 위에 UI를 생성하여 다이얼로그를 띄운다.

Navigator.of(context).pop();을 통해 Flutter의 스택의 최상위 요소를 제거하여 다이얼로그를 닫는다.

<!-- omit in toc -->
### 주요 속성
`title` : 팝업 창 제목

`content` : 팝업 창 내용

`actions` : 버튼 목록(확인, 취소)

```dart
body: Center(
    child:
        ElevatedButton(
            onPressed: () {
                showDialog(
                context: context,
                barrierDismissible: false,
                builder: (BuildContext context) {
                    return AlertDialog(
                        title: Text('제목'),
                        content: SingleChildScrollView(
                            child: ListBody(
                                children: [
                                    Text('Alert Dialog 입니다.'),
                                    Text('OK를 눌러 닫습니다.'),
                                ],
                            ),
                        ),
                        actions: [
                            TextButton(
                                child: Text('OK'),
                                onPressed: () {
                                    Navigator.of(context).pop();
                                },
                            ),
                            TextButton(
                                child: Text('Cancel'),
                                onPressed: () {
                                    Navigator.of(context).pop();
                                },
                            ),
                        ],
                    );
                },
                );
            },
            child: Text('Alert Dialog 띄우기'),
        ),
),
```
![](https://i.imgur.com/HR1pR5w.gif)

---

## 이벤트

### GestureDetector, InkWell
글자나 그림 같이 이벤트 속성이 없는 위젯에서 입력을 받고 감지할 때 사용한다.

`GestureDetector` : 터치 시 물결 표시 없음

`InkWell` : 터치 시 물결 표시

<!-- omit in toc -->
### 주요 속성
`onTap` : 탭 이벤트 감지

`onDoubleTap` : 더블 탭 이벤트 감지

`onLongPress` : 길게 누르는 이벤트 감지

```dart
body: Center(
    child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
        GestureDetector(
            onTap: () {
                print('GestureDetector 클릭!');
            },
            child: Text('클릭 Me!'),
        ),

        SizedBox(height: 80),

        InkWell(
            onTap: () {
                print('InkWell 클릭!');
            },
            child: Text('클릭 Me too!'),
        ),
        ],
    ),
),
```
![](https://i.imgur.com/O1gRaoH.gif)

---

## 애니메이션
### Hero
연결된 페이지의 화면 전환 시 애니메이션을 제공해준다.

<!-- omit in toc -->
### 주요 속성
`tag` : 애니메이션을 연결하는 고유 값. 두 페이지간 동일한 tag값을 사용해야함

```dart
body: Center(
    child: GestureDetector(
        onTap: () {
            Navigator.push(context,MaterialPageRoute(
                    builder: (context) => HeroDetailPage(),
                ),
            );
        },
        child:
        Hero(
            tag: 'image',
            child: Image.asset(
                'assets/flutter.png',
                width: 100,
                height: 100,
            )
        ),
    ),
),
```
```dart
body: Hero(
    tag: 'image',
    child: Image.asset('assets/flutter.png'),
),
```
![](https://i.imgur.com/VqeKIyM.gif)

---

## 쿠퍼티노 디자인
쿠퍼티노 디자인은 Apple의 IOS 운영 체제의 디자인 가이드 라인을 따르는 IOS 스타일의 UI이다.

### 쿠퍼티노 기본 UI
쿠퍼티노 앱의 기본 계층 구조

>`CupertinoApp` : 쿠퍼티노 구조의 루트 요소
>
>> `CupertinoPageScaffold` : 쿠퍼티노의 앱의 기본적인 레이아웃 구조
>>
>>> `navigationBar` : CupertinoNavigationBar를 이용하여 쿠퍼티노의 앱바 설정
>>>
>>> `chlid` : 앱바를 제외한 나머지 메인 화면

---

### 쿠퍼티노 AlertDialog
```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return CupertinoApp(
      debugShowCheckedModeBanner: false,
      title: 'Flutter Demo',
      theme: CupertinoThemeData(brightness: Brightness.light),
      home: CuperDialog(),
    );
  }
}
```
```dart
import 'package:flutter/cupertino.dart';

class CuperDialog extends StatelessWidget {
  const CuperDialog({super.key});

  @override
  Widget build(BuildContext context) {
    return CupertinoPageScaffold(
      navigationBar: CupertinoNavigationBar(
        middle: Text('쿠퍼티노 Dialog'),
      ),
      child: Center(
        child: CupertinoButton(
          color: CupertinoColors.activeOrange,
          onPressed: () => _showAlertDialog(context),
          child: Text(
            '쿠퍼티노 버튼',
            style: TextStyle(color: CupertinoColors.white),
          ),
        ),
      ),
    );
  }

  void _showAlertDialog(BuildContext context) {
    showCupertinoDialog(
      context: context,
      builder: (BuildContext context) => CupertinoAlertDialog(
        title: Text('다이얼로그 제목'),
        content: Text('내용'),
        actions: [
          CupertinoDialogAction(
            isDefaultAction: true,
            onPressed: () {
              Navigator.pop(context);
            },
            child: Text(
              'Ok',
              style: TextStyle(
                color: CupertinoColors.systemBlue,
              ),
            ),
          ),
          CupertinoDialogAction(
            onPressed: () {
              Navigator.pop(context);
            },
            child: Text(
              'Cancel',
              style: TextStyle(
                color: CupertinoColors.destructiveRed,
              ),
            ),
          )
        ],
      )
    );
  }
}
```
쿠퍼티노 AlertDialog 주요 계층 구조

>`CupertinoApp` : 쿠퍼티노 구조의 루트 요소
>
>> `CupertinoPageScaffold` : 쿠퍼티노의 앱의 기본적인 레이아웃 구조
>>
>>> `navigationBar` : CupertinoNavigationBar를 이용하여 쿠퍼티노의 앱바 설정
>>>
>>> `chlid` : 앱바를 제외한 나머지 메인 화면
>>>
>>>> `Center` CupertinoButton 중앙 배치
>>>> 
>>>>> `CupertinoButton` : 쿠퍼티노 기본 버튼 onPressed 속성을 이용하여 클릭시 _showAlertDialog 메서드 호출

> `_showAlertDialog` : UI 호출 메서드
>
>> `showCupertinoDialog` : 실제 UI 생성 메서드
>>
>>> `CupertinoAlertDialog` : AlertDialog 생성
>>>
>>>> `CupertinoDialogAction` : AlertDialog 내부 버튼 설정

![](https://i.imgur.com/n4I9HEW.gif)

---

## 내비게이션
### Navigator
Navigator는 화면 간 이동을 관리한다.

스택구조를 사용하여 화면을 추가(push), 제거(pop)하는 방식으로 동작한다.

<!-- omit in toc -->
### 화면 이동
`push` : 새로운 화면으로 이동
```dart
Navigator.push(
  context,
  MaterialPageRoute(builder: (context) => SecondPage()),
);
```

`pop` : 현재 페이지를 닫고, 이전 페이지로 이동
```dart
Navigator.pop(context);
```

<!-- omit in toc -->
### 값 전달
전달 할 페이지의 생성자 매개변수로 값을 전달한다.

- **이름과 나이를 가지는 클래스 정의**
  ```dart
  class Person {
    String name;
    int age;

    Person(this.name, this.age);
  }
  ```
- **first_Page.dart**
  ```dart
  floatingActionButton: ElevatedButton(
    onPressed: () {
      final person = Person('홍길동', 30);

      Navigator.push(
        context,
        MaterialPageRoute(builder: (context) => SecondPage(person: person)),
      );
    },
    child: Text('다음 페이지로'),
  ),
  ```

- **second_Page.dart**
  ```dart
  Person? person;

  SecondPage({this.person});

    floatingActionButton: ElevatedButton(
      onPressed: () {
        Navigator.pop(context);
      },
      child: Text('이전 페이지로'),
    ),
  ```
<!-- omit in toc -->
### 값 반환
.pop()의 인자 값으로 값을 반환한다.

- **first_Page.dart**
  
  `async` : 비동기 함수를 정의하는 키워드

  `awiat` : async 함수 내부에서 사용하는 키워드로 실행되는 작업이 완료될 때까지 기다리는 역할

  `비동기` : 병렬적 처리

  `동기` : 직렬적 처리

  ```dart
  floatingActionButton: ElevatedButton(
    onPressed: () async {
      final person = Person('홍길동', 30);

      final result = await Navigator.push(
        context,
        MaterialPageRoute(builder: (context) => SecondPage(person: person)),
      );

      print(result);
    },
    child: Text('다음 페이지로'),
  ),
  ```
  awiat 키워드를 사용하는 이유 : 비동기 함수 내 동기적으로 코드를 처리하기 위해서

  -> Navigator.push()의 반환값을 기다리지 않으면 다음 코드가 실행되어 오류가 발생

- **second_Page.dart**

  .pop()의 두번 째 인자로 값을 반환한다.

  ```dart
  Person? person;

  SecondPage({this.person});

    floatingActionButton: ElevatedButton(
      onPressed: () {
        Navigator.pop(context, "잘 전달 됨");
      },
      child: Text('이전 페이지로'),
    ),
  ```

### Navigator + routes
`Router` : 네트워크간 데이터를 주고받는 장치

`routes` : Flutter 앱에서 화면의 페이지 별 경로를 이름으로 정의

`Navigator + routes` : Navigator로 화면 이동을 관리 / routes로 이동할 화면을 이름으로 정의 및 관리

- **main.dart**

  routes로 정의한 페이지 이름과 클래스를 연결

  ```dart
  routes: {
    '/first':(context) => FirstPage(),
    '/second':(context) => SecondPage(),
  },
  ```

- **first_Page.dart**

  pushNamed() 메서드를 사용하여 화면을 이동시키고, 값을 전달한다.
  
  pushNamed()의 매개변수 :
  
  `routeName` : 이동할 페이지 이름

  `arguments` : 전달 할 데이터

  ```dart
  floatingActionButton: ElevatedButton(
    onPressed: () async {
      final person = Person('홍길동', 30);

      var result = await Navigator.pushNamed(context, '/second', arguments: person);

      print(result);
    },
    child: Text('다음 페이지로'),
  ),
  ```

- **second_Page.dart**

  `ModalRoute.of(context)` : 현재 표시되는 화면의 ModalRoute 객체를 반환

  `settings.arguments` : 반환된 Route 객체의 설정에 전달된 arguments 값을 반환

  `as Person` : 반환된 arguments 값을 Person 객체로 형변환

  ```dart
  class SecondPage extends StatelessWidget {
    Person? person;

    SecondPage({this.person});

    @override
    Widget build(BuildContext context) {
      person = ModalRoute.of(context)?.settings.arguments as Person;

      return Scaffold(
        appBar: AppBar(title: Text('Second Page')),
        body: Container(
          color: Colors.green,
          child: Text('${person?.name}, ${person?.age}'),
        ),
        floatingActionButton: ElevatedButton(
          onPressed: () {
            Navigator.pop(context, "잘 전달됨");
          },
          child: Text('이전 페이지로'),
        ),
      );
    }
  }
  ```

### 페이지 생명주기
[위젯의 생명주기 메서드](https://github.com/BAE999/TIL/blob/main/Flutter/%EC%9C%84%EC%A0%AF%20%EC%83%9D%EB%AA%85%EC%A3%BC%EA%B8%B0.md)를 통해 페이지의 생명주기를 확인해보자

<!-- omit in toc -->
### fist.dart

createState -> initState -> didChangeDependencies -> build 순으로 호출된다.

![](https://i.imgur.com/R71SY90.png)

![](https://i.imgur.com/W6k08Na.png)

<!-- omit in toc -->
### first.dart -> second.dart

첫번째 페이지에서 두번째 페이지로 넘어갈 때 역시 똑같이 호출된다.

그런데 첫번째 페이지에서는 아무것도 호출되지 않는 것을 볼 수 있다.

그 이유는 첫번째 페이지는 사라지지 않고 여전히 메모리에 남아있기 때문이다.

![](https://i.imgur.com/sHaBE4V.png)

![](https://i.imgur.com/kosgwNY.png)

<!-- omit in toc -->
### second.dart -> first.dart

두번째 페이지에서 첫번째 페이지로 넘어갈 시에는 deactivate -> dispose 순으로 호출된다.

이는 두번째 페이지는 메모리상에서 완전히 사라진다는 것이다.

![](https://i.imgur.com/ybdR9eo.png)

![](https://i.imgur.com/VwrsZEE.png)

<!-- omit in toc -->
## 전체 코드

![](https://i.imgur.com/5n5teJ0.gif)

<!-- omit in toc -->
### main.dart
```dart
import 'package:hello/first.dart';
import 'package:hello/second.dart';
import 'package:flutter/material.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
      ),
      home: First(),
      routes: {
        '/first':(context) => First(),
        '/second':(context) => Second(),
      },
    );
  }
}
```
<!-- omit in toc -->
### person.dart
```dart
class Person {
  String name;
  int age;

  Person(this.name, this.age);
}
```

<!-- omit in toc -->
### first.dart
```dart
import 'package:flutter/material.dart';
import 'package:hello/person.dart';

class First extends StatefulWidget {
  const First({super.key});

  @override
  State<First> createState() {
    print('first_page createState()호출');
    return _FirstState();
  }
}

class _FirstState extends State<First> {
  final TextEditingController _controller1 = TextEditingController();
  final TextEditingController _controller2 = TextEditingController();

  @override
  void initState() {
    super.initState();
    print('first_page initState() 호출');
  }

  @override
  void didChangeDependencies() {
    super.didChangeDependencies();
    print('first_page didChangeDependencies() 호출');
  }

  @override
  void didUpdateWidget(covariant First oldWidget) {
    super.didUpdateWidget(oldWidget);
    print('first_page didUpdateWidget() 호출');
  }

  @override
  void deactivate() {
    super.deactivate();
    print('first_page deactivate()호출');
  }

  @override
  void dispose() {
    super.dispose();
    print('first_page dispose() 호출');
  }

  @override
  Widget build(BuildContext context) {
    print('first_page build() 호출');
    return Scaffold(
      body: Center(
        child: Padding(
          padding: EdgeInsets.all(10.0),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              TextField(
                controller: _controller1,
                decoration: InputDecoration(
                  border: OutlineInputBorder(),
                  labelText: '이름을 입력하세요.',
                ),
              ),

              SizedBox(height: 40),

              TextField(
                controller: _controller2,
                decoration: InputDecoration(
                  border: OutlineInputBorder(),
                  labelText: '나이를 입력하세요.',
                ),
              ),
            ],  
          ),
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () async{
          final person = Person(
            '${_controller1.text}',
            int.tryParse(_controller2.text) ?? 0,
          );

          var result = await Navigator.pushNamed(context, '/second', arguments: person);
        },
        child: Text('입력'),
      ),
    );
  }
}
```

<!-- omit in toc -->
### second.dart
```dart
import 'package:hello/person.dart';
import 'package:flutter/material.dart';

class Second extends StatefulWidget {

  @override
  State<Second> createState() {
    print('second_page createState() 호출');
    return _SecondState();
  }
}

class _SecondState extends State<Second> {

  @override
  void initState() {
    super.initState();
    print('second_page initState() 호출');
  }

  @override
  void didChangeDependencies() {
    super.didChangeDependencies();
    print('second_page didChangeDependencies() 호출');
  }

  @override
  void didUpdateWidget(covariant Second oldWidget) {
    super.didUpdateWidget(oldWidget);
    print('second_page didUpdateWidget() 호출');
  }

  @override
  void deactivate() {
    super.deactivate();
    print('second_page deactivate()호출');
  }

  @override
  void dispose() {
    super.dispose();
    print('second_page dispose() 호출');
  }

  @override
  Widget build(BuildContext context) {
    print('second_page build() 호출');
    Person? person = ModalRoute.of(context)?.settings.arguments as Person;

    return Scaffold(
      appBar: AppBar(title: Text('Second Page')),
      body: Center(
        child: Text('${person?.name}, ${person?.age}', style: TextStyle(fontSize: 40)),
      ),
      floatingActionButton: ElevatedButton(
        onPressed: () {
          Navigator.pop(context, "잘 전달됨");
        },
        child: Text('이전 페이지로'),
      ),
    );
  }
}
```

