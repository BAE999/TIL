# 목차
- [목차](#목차)
- [기본 위젯 II](#기본-위젯-ii)
  - [입력 위젯](#입력-위젯)
    - [TextField](#textfield)
      - [주요 속성](#주요-속성)
    - [CheckBox, Switch](#checkbox-switch)
      - [주요 속성](#주요-속성-1)
    - [Radio + ListTile](#radio--listtile)
      - [주요 속성](#주요-속성-2)
    - [DropDownButton](#dropdownbutton)
      - [주요 속성](#주요-속성-3)
  - [다이얼로그](#다이얼로그)
    - [AlertDialog](#alertdialog)
      - [주요 속성](#주요-속성-4)

# 기본 위젯 II
Flutter의 기본 위젯 II

## 입력 위젯
### TextField
사용자가 텍스트를 입력할 수 있는 입력 필드다.

#### 주요 속성
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

### CheckBox, Switch
`CheckBox` : 체크박스 형태로 선택한다.

`Switch` : 토글 형태로 ON / OFF 상태를 변경한다.

#### 주요 속성
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

### Radio + ListTile
`Radio` : 선택 그룹 중 하나를 선택할 때 사용한다.

`ListTile` : 리스트의 개별 항목을 구성한다.

`RadioListTile` : 리스트의 개별 항목마다 가지는 영역을 클릭하여 선택할 수 있다. ListTile은 체크되는 영역만 클릭할 수 있다.

#### 주요 속성
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

### DropDownButton
여러 옵션 중 하나를 선택할 수 있도록 드롭다운 메뉴를 제공한다.

#### 주요 속성
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

## 다이얼로그
### AlertDialog
사용자에게 중요한 정보를 알리고, 확인 또는 취소 등의 요청하는 팝업 창

showDialog()를 호출하면 현재 화면 위에 UI를 생성하여 다이얼로그를 띄운다.

Navigator.of(context).pop();을 통해 Flutter의 스택의 최상위 요소를 제거하여 다이얼로그를 닫는다.
#### 주요 속성
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