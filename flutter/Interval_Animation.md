`Interval`    
애니메이션의 Duration을 0-1 간격으로 보았을때 설정된 간격 안에 에니메이션이 실행될수 있게 해준다. 여러 에니메이션을 동시 또는 일정 간격으로 실행해야될때, 또는 애니메이션을 실행시간을 의도적으로 늦추고 싶을때 사용하면
좋을것 같다.
   
`AnimationController.view`  
이 애니메이션 컨트롤러에 대해 Animation<double>을 반환하여 해당 포인터의 사용자가 AnimationController 상태를 변경하지 않고도 이 개체에 대한 포인터를 전달할 수 있습니다.
라고 한다.
  
`샘플`   

  

https://user-images.githubusercontent.com/33044667/148500868-94e503ce-f33c-4a87-9a03-d87d7712cff1.mp4


`샘플코드`
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Animated_builder sample',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({Key? key}) : super(key: key);

  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage>
    with SingleTickerProviderStateMixin {
  late AnimationController _animationController;
  late Animation _animationControllerView;
  late Animation _opacityAnimation;
  late Animation _widthAnimation;
  late Animation _heightAnimation;
  late Animation _borderRadiusAnimation;

  initAnimationResources() {
    _animationController =
        AnimationController(vsync: this, duration: Duration(seconds: 2));
    _animationControllerView = _animationController.view;
    _opacityAnimation = Tween(begin: 0.0, end: 1.0).animate(CurvedAnimation(
        parent: _animationController,
        curve: Interval(0, 0.3, curve: Curves.fastOutSlowIn)));
    _widthAnimation = Tween(begin: 50.0, end: 400.0).animate(CurvedAnimation(
        parent: _animationController,
        curve: Interval(0.3, 0.8, curve: Curves.fastOutSlowIn)));
    _heightAnimation = Tween(begin: 100.0, end: 400.0).animate(CurvedAnimation(
        parent: _animationController,
        curve: Interval(0.5, 0.8, curve: Curves.fastOutSlowIn)));
    _borderRadiusAnimation = Tween(begin: 0.0, end: 50.0).animate(
        CurvedAnimation(
            parent: _animationController,
            curve: Interval(0.7, 1.0, curve: Curves.fastOutSlowIn)));
  }

  @override
  void initState() {
    initAnimationResources();
    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        body: AnimatedBuilder(
            animation: _animationControllerView,
            builder: (buildContext, widget) {
              return Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  TextButton(
                      onPressed: () {
                        _animationController.forward();
                      },
                      child: Text('forward')),
                  TextButton(
                      onPressed: () {
                        _animationController.reverse();
                      },
                      child: Text('backward')),
                  Container(
                    alignment: Alignment.center,
                    child: Opacity(
                      opacity: _opacityAnimation.value,
                      child: Container(
                        width: (_widthAnimation.value as int).toDouble(),
                        height: (_heightAnimation.value as int).toDouble(),
                        alignment: Alignment.center,
                        child: Text(
                          "TTHIS IS CONTENTTHIS IS CONTENTTHIS IS CONTENTTHIS IS CONTENTTHIS IS CONTENTHIS IS CONTENT",
                          overflow: TextOverflow.ellipsis,
                          maxLines: 3,
                        ),
                        decoration: BoxDecoration(
                            border:
                                Border.all(color: Colors.blueGrey, width: 4),
                            shape: BoxShape.rectangle,
                            color: Colors.red,
                            borderRadius: BorderRadius.all(
                                Radius.circular(_borderRadiusAnimation.value))),
                      ),
                    ),
                  ),
                ],
              );
            }));
  }
}


```
  
