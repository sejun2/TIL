## Hover 시 색이 물드는 애니메이션
   

![스크린샷 2022-01-11 14 33 02](https://user-images.githubusercontent.com/33044667/148886839-2594902c-17e5-4bae-9dce-58e6780b5e2f.png)
![스크린샷 2022-01-11 14 32 52](https://user-images.githubusercontent.com/33044667/148886855-33dd798a-7953-455c-9bea-5ce4b366e837.png)   
   
Flluter.dev 에서 카드 에니메이션 따라하기   
실제로 에니메이션은 bottomleft부터 시작해서 topright 까지 색이 순차적으로 변하는 애니메이션이다.  

Inkwell 위젯이 제공해주는 hoverColor로는 세세한 애니메이션을 구현할수가 없어서, TweenCurve 애니메이션과 Animatedbuilder를 이용해 만들었다. 
  
`Animation Initialize 코드`
```dart
 _animationController =
        AnimationController(vsync: this, duration: const Duration(milliseconds: 500), lowerBound: 0.0, upperBound: 5.0);
    _animation =
        Tween(begin: 0.0, end: 5.0).animate(CurvedAnimation(parent: _animationController, curve: Curves.elasticInOut));
    super.initState();
```
값이 변하는 에니메이션이므로 Tween으로 만들고 Curve를 주기위해 CurvedAnimation을 붙여주었다.

`AnimatedBuilder 코드`
```dart
AnimatedBuilder(
                    animation: _animation,
                    builder: (context, widget) {
                      return Container(
                        decoration: BoxDecoration(
                            borderRadius:
                                const BorderRadius.all(Radius.circular(24)),
                            gradient: RadialGradient(
                                colors: const [Colors.blue, Colors.white],
                                radius: _animationController.value,
                                center: Alignment.bottomLeft)),
                        width: 700,
                      );
                    }),
```
퍼져가는 색상 에니메이션을 위해서 RadialGradient롤 만들고, 반지름인 radius를 조절해주는 방식으로 구현했다. 
원본 flutter.dev에 있는 카드는 색이 bottomleft부터 퍼지므로 마찬가지로 RadialGradient의 시작지점을 bottomLeft로 잡아주었다.

`Hover가 동작하기위해선 onTap이 null이면 안되는것 같다. onTap까지 구현을 해주어야 정상적으로 hover가 동작한다.`

`결과`

https://user-images.githubusercontent.com/33044667/148888146-813d3f01-a864-4874-9ed1-3092bf7e4996.mp4


   
`전체 코드`
```dart
import 'package:flutter/material.dart';

class FifthPage extends StatefulWidget {
  const FifthPage({Key? key}) : super(key: key);

  @override
  _FifthPageState createState() => _FifthPageState();
}

class _FifthPageState extends State<FifthPage>
    with SingleTickerProviderStateMixin {

  late AnimationController _animationController;
  late Animation _animation;

  @override
  void initState() {
    _animationController =
        AnimationController(vsync: this, duration: const Duration(milliseconds: 500), lowerBound: 0.0, upperBound: 5.0);
    _animation =
        Tween(begin: 0.0, end: 5.0).animate(CurvedAnimation(parent: _animationController, curve: Curves.elasticInOut));
    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Card(
          elevation: 10,
          shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.all(Radius.circular(24))),
          child: InkWell(
            borderRadius: BorderRadius.all(Radius.circular(24)),
            onHover: (bool) {
              setState(() {
                if (bool) {
                  _animationController.forward();
                } else {
                  _animationController.reverse();
                }
              });
            },
            child: Stack(
              children: [
                AnimatedBuilder(
                    animation: _animation,
                    builder: (context, widget) {
                      return Container(
                        decoration: BoxDecoration(
                            borderRadius:
                                const BorderRadius.all(Radius.circular(24)),
                            gradient: RadialGradient(
                                colors: const [Colors.blue, Colors.white],
                                radius: _animationController.value,
                                center: Alignment.bottomLeft)),
                        width: 700,
                      );
                    }),
                Positioned(
                  top: 0,
                  bottom: 0,
                  right: 0,
                  left: 0,
                  child: Padding(
                    padding: EdgeInsets.all(58),
                    child: Container(
                      width: 700,
                      height: 800,
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: [
                          ClipRRect(
                            borderRadius: BorderRadius.all(Radius.circular(16)),
                            child: Image.network(
                                'https://dummyimage.com/600x400/000/fff'),
                          ),
                          Text(
                            'This is title',
                            style: TextStyle(
                                fontWeight: FontWeight.bold,
                                color: Colors.black,
                                fontSize: 46),
                          ),
                          Text(
                            'Read more',
                            style: TextStyle(
                                fontWeight: FontWeight.bold,
                                color: Colors.black,
                                fontSize: 18),
                          )
                        ],
                      ),
                    ),
                  ),
                ),
              ],
            ),
          )),
    );
  }
}

```
 
