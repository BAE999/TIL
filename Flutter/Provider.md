# Provider
Provider란 상태 관리를 쉽게 할 수 있도록 도와주는 라이브러리다.

기존 setState()를 사용하면 여러화면에서 동일한 상태를 공유하게 될 시 코드가 복잡해지고, 중복될 수 있다.

Provider는 이러한 상태를 전역으로 관리하여 여러화면에서 동일한 상태를 공유하고 관리할 수 있게 해주며, 데이터가 변할 시 자동으로 UI에 반영해준다.

## ChangeNotifier / notifyListeners()
상태관리 클래스를 선언하여 ChangeNotifier를 상속받는다.

ChangeNotifier는 상태 변경을 감지하고, 상태 변경 시 notifyListeners() 메서드를 호출한다.

notifyListeners() 메서드는 이 상태를 구독하고 있는 위젯들에게 상태가 바뀌었다고 알린다. 

이를 통해 UI는 자동으로 갱신되며, 상태를 업데이트한다.

```dart
class ItemProvider extends ChangeNotifier {
  final List<Item> _itemList = List.empty(growable: true);
  
  List<Item> getItemList() {
    _fetchItems();
    return _itemList;
  }
  
  void _fetchItems() async {
    final response = await http.get(Uri.parse('http://localhost:8888/'));
    final List<Item> result = jsonDecode(response.body)
      .map<Item>((json) => Item.fromJson(json))
      .toList();

    _itemList.clear();
    _itemList.addAll(result);
    notifyListeners();
  }
}
```

## Consumer
상태를 구독하고 있는 위젯에서는 Consumer 또는 Provider.of()를 통해 상태를 받아서 UI를 재빌드한다.

Provider.of()는 위젯 전체가 rebuild 되며, Consumer는 상태가 변경된 부분만 rebuild 하게 된다.
```dart
body: Consumer<ItemProvider>(
  builder: (context, provider, child) {
    itemList = provider.getItemList();
    return ListView.builder(
      itemCount: itemList.length,
      itemBuilder: (context, index) {
        return InkWell(
          onTap: () {},
          child: Container(
            padding: EdgeInsets.all(15),
            child: Text("${itemList[index].id}: ${itemList[index].title}, ${itemList[index].content}"),
          ),
        );
      }
    );
  }
),
```