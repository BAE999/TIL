# MVVM 아키텍쳐 패턴
![](https://i.imgur.com/e3BLi83.png)

![](https://i.imgur.com/QzdDj6v.gif)

### main.dart
```dart
import 'package:provider/provider.dart';
import 'package:ui_test_refactoring/constants/strings.dart';
import 'package:flutter/material.dart';
import 'package:ui_test_refactoring/features/post/main_app.dart';
import 'package:ui_test_refactoring/features/post/viewmodel/post_viewmodel.dart';
import 'features/post/viewmodel/main_app_page.dart';

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

### main_app.dart
```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import 'package:ui_test_refactoring/constants/btm_nav_icons.dart';
import 'package:ui_test_refactoring/constants/strings.dart';
import 'package:ui_test_refactoring/features/post/viewmodel/main_app_page.dart';

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

### DataSource
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

```