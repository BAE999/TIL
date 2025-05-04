# 목차
- [목차](#목차)
- [MVVM 아키텍처 패턴](#mvvm-아키텍처-패턴)
  - [상호작용 흐름](#상호작용-흐름)
  - [특징](#특징)
  - [장점](#장점)
  - [단점](#단점)
  - [MVVM 패턴 적용](#mvvm-패턴-적용)
    - [Model](#model)
    - [Data](#data)
    - [Repository](#repository)
    - [ViewModel](#viewmodel)
    - [View](#view)

# MVVM 아키텍처 패턴

MVVM 이란 Model - View - ViewModel로 구성된 아키텍쳐 패턴이다.

애플리케이션을 구조화하기 위한 디자인 패턴으로 주로 UI 개발을 더 모듈화 하고,  
테스트 가능하며, 유지보수하기 쉽게 적용 가능한 아키텍처이다.

## 상호작용 흐름
1. View: 사용자의 입력 액션

2. View - > ViewModel: View에서 발생한 사용자 액션을 ViewModel로 전달

3. ViewModel -> Model: ViewModel에서 사용자 요청에 따른 비지니스 로직을 Model에게 요청

4. Model: 요청 받은 데이터 처리 및 비지니스 로직을 수행

5. Model -> ViewModel: Model의 데이터가 변경되면, ViewModel은 이를 감지하거나 Model로부터 업데이트

6. ViewModel -> View: View는 ViewModel을 데이터 상태관리(Provider, Stream)를 통해 View를 업데이트

## 특징
- View와 Model의 의존성이 없음

- ViewModel은 특정 View에 종속되지 않기에 여러 View가 하나의 ViewModel을 참조할 수 있음

- 상태관리 도구(Provider, Riverpod, Bloc 등)를 통해 ViewModel의 상태 변경을 View에 반영

## 장점
- 관심사 분리
  - UI(View), 표현 로직(ViewModel), 비지니스 로직 및 데이터(Model)가 명확히 분리되어 코드를 이해하고 관리하기 쉬움

- 테스트 용이성
  - ViewModel은 UI에 종속되지 않으므로, UI 없이 ViewModel의 로직을 단위 테스트하기 쉬움

- 유지보수성
  - 각 구성 요소가 독립적이므로 한 부분의 변경이 다른 부분에 미치는 영향을 최소화하여 코드 수정 및 기능 추가가 용이함

- 협업 용이성
  - 프론트 / 백엔드 각각 독립적으로 개발할 수 있음

- 데이터 바인딩 활용
  - 상태관리도구(Provider, Riverpod, GexX, Bloc 등)를 통해 View와 ViewModel 간의 데이터 동기화를 자동화 할 수 있음

## 단점
- 초기 복잡성
  - 간단한 앱의 경우, MVVM 패턴을 적용하는 것이 오히려 복잡해질 수 있음

- ViewModel 비대화
  - 잘못 설계시 ViewModel이 너무 많은 로직을 가지게 되어 비대해질 수 있음

## MVVM 패턴 적용

![](https://i.imgur.com/QzdDj6v.gif)

기존 레거시 코드에서 MVVM 패턴을 적용

![](https://i.imgur.com/73OgfZ0.png)

**↓**

![](https://i.imgur.com/FyQapWy.png)

```
lib/
├── constants/ 앱 전반 공통적으로 사용되는 상수를 정의
├── core/ 애플리케이션 전체에서 재사용 가능한 핵심 요소
│   ├── widgets/
├── features/ 앱의 핵심 기능
│   ├── post/ 
│   │   ├── data/ 외부 데이터 소스와 통신
│   │   ├── model/ 데이터 설계
│   │   ├── repository/ Data 접근 
│   │   ├── view/ 사용자 UI
│   │   ├── viewmodel/ View의 상태를 관리하며, View의 비지니스 로직을 담당
│   ├── main_app.dart
├── routing/ 화면 이동 관련 정의
├── main.dart 
```

<!-- omit in toc -->
### main.dart
```dart
import 'package:provider/provider.dart';
import 'package:ui_test_refactoring/constants/strings.dart';
import 'package:flutter/material.dart';
import 'package:ui_test_refactoring/features/main_app.dart';
import 'package:ui_test_refactoring/features/post/viewmodel/post_viewmodel.dart';
import 'routing/main_app_router.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: Strings.appName,
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.white),
      ),
      home: MultiProvider(
        providers: [
          ChangeNotifierProvider(create: (_) => MainAppPage()),
          ChangeNotifierProvider(create: (_) => PostViewModel()),
        ],
        child: MainApp(),
      ),
    );
  }
}
```

<!-- omit in toc -->
### main_app.dart
```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import 'package:ui_test_refactoring/constants/btm_nav_icons.dart';
import 'package:ui_test_refactoring/constants/strings.dart';
import 'package:ui_test_refactoring/routing/main_app_router.dart';

class MainApp extends StatelessWidget {
  const MainApp({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(Strings.appName),
        centerTitle: true,
        actions: [
          IconButton(
            onPressed: () {},
            icon: Icon(Icons.add),
          ),
        ],
      ),
      body: Consumer<MainAppPage>(
        builder: (context, provider, child) {
          return provider.pages[provider.index];
        },
      ),
      bottomNavigationBar: Consumer<MainAppPage>(
        builder: (context, provider, child) {
          return BottomNavigationBar(
            currentIndex: provider.index,
            onTap: (index) => provider.changePage(index),
            items: [
              BtmNavIcons.home,
              BtmNavIcons.assessment,
              BtmNavIcons.account,
            ],
          );
        },
      ),
    );
  }
}
```

### Model
```dart
class Post {
  int? id;
  String? title;
  String content;

  Post({this.id, this.title, required this.content});

  factory Post.fromJson(Map<String, dynamic> json) {
    return Post(content: json['content']);
  }

}
```

### Data
```dart
import 'dart:convert';
import 'package:http/http.dart' as http;
import 'package:ui_test_refactoring/features/post/model/post.dart';

class PostData {
  Future<List<Post>> getPostList() async {
    final response = await http.get(Uri.parse('http://localhost:8888/'));

    return jsonDecode(utf8.decode(response.bodyBytes))
        .map<Post>((json) => Post.fromJson(json))
        .toList();
  }
}
```

### Repository
```dart
import 'package:ui_test_refactoring/features/post/data/post_data.dart';
import 'package:ui_test_refactoring/features/post/model/post.dart';

class PostRepository {
  final PostData _postData = PostData();

  Future<List<Post>> getPostList() {
    return _postData.getPostList();
  }
}
```

### ViewModel
```dart
import 'package:flutter/material.dart';
import 'package:ui_test_refactoring/features/post/model/post.dart';
import 'package:ui_test_refactoring/features/post/repository/post_repository.dart';

class PostViewModel extends ChangeNotifier{
  late final PostRepository _postRepository;
  List<Post> _postList = List.empty(growable: true);
  List<Post> get postList => _postList;

  PostViewModel() {
    _postRepository = PostRepository();
    _getPostList();
  }

  Future<void> _getPostList() async {
    _postList = await _postRepository.getPostList();
    notifyListeners();
  }

}
```

### View
```dart
import 'package:flutter/material.dart';
import 'package:ui_test_refactoring/core/widgets/icon_widget.dart';
import 'package:ui_test_refactoring/core/widgets/list_item_builder.dart';
import 'package:ui_test_refactoring/core/widgets/slide_image.dart';

class Page1 extends StatelessWidget {
  const Page1({super.key});


  @override
  Widget build(BuildContext context) {
    return ListView(children: [IconWidget(), SlideImage(), ListItemBuilder()]);
  }
}
```

<!-- omit in toc -->
### core/widgets/icon_widget.dart
```dart
import 'package:flutter/material.dart';
import 'package:ui_test_refactoring/constants/top_icons.dart';

class IconWidget extends StatelessWidget {
  const IconWidget({super.key});

  @override
  Widget build(BuildContext context) {
    List<String> topIcons = TopIcons.topIcons;
    return Padding(
      padding: const EdgeInsets.only(top: 20, bottom: 20),
      child: Column(
        children: List.generate(2, (rowIndex) {
          return Padding(
            padding: const EdgeInsets.only(bottom: 20),
            child: Row(
              mainAxisAlignment: MainAxisAlignment.spaceEvenly,
              children: List.generate(topIcons.length, (index) {
                bool isLastInvisible = (rowIndex == 1) && (index == topIcons.length - 1);
                return Opacity(
                  opacity: isLastInvisible ? 0.0 : 1.0,
                  child: GestureDetector(
                    onTap: () {
                      print('클릭!');
                    },
                    child: Column(
                      children: [
                        Icon(Icons.local_taxi, size: 40),
                        Text(topIcons[index]),
                      ],
                    ),
                  ),
                );
              }),
            ),
          );
        }),
      ),
    );
  }
}
```

<!-- omit in toc -->
### core/widgets/list_item_builder.dart
```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';

import '../../features/post/model/post.dart';
import '../../features/post/viewmodel/post_viewmodel.dart';

class ListItemBuilder extends StatelessWidget {
  const ListItemBuilder({super.key});

  @override
  Widget build(BuildContext context) {
    return Consumer<PostViewModel>(
      builder: (context, provider, child) {
        late List<Post> postList;
        postList = provider.postList;
        return ListView.builder(
          physics: NeverScrollableScrollPhysics(),
          shrinkWrap: true,
          itemCount: postList.length,
          itemBuilder: (context, index) {
            return ListTile(
              leading: Icon(Icons.notifications_none),
              title: Text(postList[index].content),
            );
          },
        );
      },
    );
  }
}
```

<!-- omit in toc -->
### core/widgets/slide_image.dart
```dart
import 'package:carousel_slider/carousel_slider.dart';
import 'package:flutter/material.dart';

import '../../constants/assets.dart';

class SlideImage extends StatelessWidget {
  const SlideImage({super.key});

  @override
  Widget build(BuildContext context) {
    return CarouselSlider(
      options: CarouselOptions(height: 150.0, autoPlay: true),
      items:
      Assets.main_img.map((url) {
        return Builder(
          builder: (BuildContext context) {
            return Container(
              width: MediaQuery.of(context).size.width,
              margin: EdgeInsets.symmetric(horizontal: 5.0),
              child: ClipRRect(
                borderRadius: BorderRadius.circular(8.0),
                child: Image.network(url, fit: BoxFit.cover),
              ),
            );
          },
        );
      }).toList(),
    );
  }
}
```

<!-- omit in toc -->
### References
- [totally-developer](https://totally-developer.tistory.com/115)

- [LY Corporation Tech Blog](https://techblog.lycorp.co.jp/ko/flutter-clean-architecture)