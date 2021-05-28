## Basic widgets
- Text : Text위젯
- Row, Column : 유연한 레이아웃 구성을 하게 해주는 위젯. horizontal(Row) and vertical(Column) 방향. 웹의 flexbox 레이아웃 모델을 따른다.
- Stack : 수평이나 수직 방향 대신에, 위에서 쌓이는 레이아웃. 그리고 Positioned위젯을 사용해서 상대적 위치도 지정할 수 있다.
- Container : 사각형 모양의 위젯. 이 위젯은 BoxDecoration으로 데코레이트 될수 있는데 예를들어 백그라운드나 border 또는 그림자같은것들. 또한 margin, padding, constraints도 설정 가능하다. 게다가 matrix를 사용해서 3차원의 변형도 가능하다.  
이 위젯들을 활용한 예제코드
```dart
import 'package:flutter/material.dart';

class MyAppBar extends StatelessWidget {
  MyAppBar({this.title});

  // Fields in a Widget subclass are always marked "final".

  final Widget title;

  @override
  Widget build(BuildContext context) {
    return Container(
      height: 56.0, // in logical pixels
      padding: const EdgeInsets.symmetric(horizontal: 8.0),
      decoration: BoxDecoration(color: Colors.blue[500]),
      // Row is a horizontal, linear layout.
      child: Row(
        // <Widget> is the type of items in the list.
        children: <Widget>[
          IconButton(
            icon: Icon(Icons.menu),
            tooltip: 'Navigation menu',
            onPressed: null, // null disables the button
          ),
          // Expanded expands its child to fill the available space.
          Expanded(
            child: title,
          ),
          IconButton(
            icon: Icon(Icons.search),
            tooltip: 'Search',
            onPressed: null,
          ),
        ],
      ),
    );
  }
}

class MyScaffold extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // Material is a conceptual piece of paper on which the UI appears.
    return Material(
      // Column is a vertical, linear layout.
      child: Column(
        children: <Widget>[
          MyAppBar(
            title: Text(
              'Example title',
              style: Theme.of(context).primaryTextTheme.title,
            ),
          ),
          Expanded(
            child: Center(
              child: Text('Hello, world!'),
            ),
          ),
        ],
      ),
    );
  }
}

void main() {
  runApp(MaterialApp(
    title: 'My app', // used by the OS task switcher
    home: MyScaffold(),
  ));
}
```
만약 메터리얼 디자인을 사용하고 싶다면? pubspec.yaml파일에서 flutter 섹션에 uses-material-disign: true 적용하면 된다.

