# 목차
- [목차](#목차)
- [Null Safety](#null-safety)
  - [Nullalbe 타입](#nullalbe-타입)
  - [Non-nullable 타입](#non-nullable-타입)
  - [late 키워드](#late-키워드)
  - [Null 인지 연산자(null-aware)](#null-인지-연산자null-aware)
    - [??](#)
    - [??=](#-1)
    - [?.](#-2)
    - [!.](#-3)
  - [required](#required)

# Null Safety
Null Safety란 null 값으로 인해 발생하는 오류를 방지하기 위해 도입된 개념이다.

Dart 2.12 버전 부터 기본적으로 non-nullable 타입을 하도록 변경되었다.

## Nullalbe 타입
타입 뒤에 ? 를 붙여서 null을 허용할 수 있다.

```dart
String? name;
```

## Non-nullable 타입
Dart는 기본적으로 모든 변수에 null을 허용하지 않으며, 반드시 초기화 해야한다.

```dart
String name = "hong";
```

## late 키워드
변수를 나중에 반드시 초기화할 것을 보장할 때 사용한다.
```dart
late String username;

void init() => print(username = "홍길동");

init();
```
```
홍길동
```

## Null 인지 연산자(null-aware)
### ??
피연산자가 null값이면 오른쪽 값을 반환한다.

null값에 대한 예외처리 혹은 기본값 출력이라고 볼 수 있다.

```dart
String? username;

print(username ?? '기본값');
```
```
기본값
```

### ??=
변수가 null 값일 때만 값을 할당한다.

```dart
String? username;
username ??= "기본값";

print(username);
```

?? 연산자와 다른 점은 ??= 연산자는 변수에 값을 할당하고, ?? 연산자는 값을 반환한다.

### ?.
객체가 null 값이면 메서드나 속성을 호출하지 않고 null을 반환한다.

```dart
String? name;

print(name?.length);
```
```
null
```

### !.
null값이 아니라고 확신할 때 사용한다. 만약 null일 경우 런타임 오류가 발생한다.
```dart
List<String>? names;

print(names!.length);
```
```
Error: Unexpected null value., error: Error: Unexpected null value.
```

## required
선택적 매개변수에서 해당 매개변수가 값을 필수로 받을 때 사용한다.
```dart
class User {
  String name;
  int age;

  User({required this.name, required this.age});
}

void main() {
  User user = User(name: "홍길동", age: 25);

  print('이름 : ${user.name}, 나이 : ${user.age}');
}
```
```
이름 : 홍길동, 나이 : 25
```