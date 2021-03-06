## BoxShadow 속성을 이용하여 flutter.dev에 있는 카드 모양 뷰 만들기  
  
처음에는 단순히 elevation 값만 주면 쉽게 만들수 있다고 생각했는데,   
elevation속성으로 인해 생기는 그림자와는 조금 달랐다.   BoxShadow 속성을 이용하면 비슷하게 구현할수 있을것 같아서 따라해봄  
  
1. spreadRadius - 그림자의 크기
2. blurRadius - spreadRadius를 기준으로 blur될 크기 ? 정도로 이해하면 될듯
3. blurStyle -   
/// Fuzzy inside and outside. This is useful for painting shadows that are
  /// offset from the shape that ostensibly is casting the shadow.
  normal,

  /// Solid inside, fuzzy outside. This corresponds to drawing the shape, and
  /// additionally drawing the blur. This can make objects appear brighter,
  /// maybe even as if they were fluorescent.
  solid,

  /// Nothing inside, fuzzy outside. This is useful for painting shadows for
  /// partially transparent shapes, when they are painted separately but without
  /// an offset, so that the shadow doesn't paint below the shape.
  outer,

  /// Fuzzy inside, nothing outside. This can make shapes appear to be lit from
  /// within.
  inner,

   
spreadRadius는 0, blurRadius는 10, blurstyle은 outer을 사용했다. 
blurRadius가 시작될 그림자의 크기는 card의 크기와 같아야하므로 0, blurRadius는 적당한 값 10,   
그리고 card의 color 속성에 opacity가 들어가게 만들것이므로 outer을 선택했다.
  
`코드`

```dart
class _SixthPageState extends State<SixthPage> {
  @override
  Widget build(BuildContext context) {
    return Center(
      child: Table(
        columnWidths: {
          0: FixedColumnWidth(180),
          1: FixedColumnWidth(180),
          2: FixedColumnWidth(180),
          3: FixedColumnWidth(180),
          4: FixedColumnWidth(180),
        },
        defaultVerticalAlignment: TableCellVerticalAlignment.middle,
        children: [
          TableRow(
            children: [
              buildCard(),
              buildCard(),
              buildCard(),
              buildCard(),
              buildCard(),
            ].map((e) => Center(child: e,)).toList(),
          ),
        ],
      ),
    );
  }

  Widget buildCard() {
    return SizedBox(
      width: 140,
      height: 140,
      child: Container(
        decoration: BoxDecoration(
          color: Colors.amber,
          boxShadow: [
            BoxShadow(spreadRadius: 0,blurRadius: 10,color: Colors.grey, blurStyle: BlurStyle.outer),
          ],
          borderRadius: BorderRadius.all(Radius.circular(8)),
        ),
      ),
    );
  }
}
```

`결과 비교`


![s1](https://user-images.githubusercontent.com/33044667/149259227-42b6841a-b61d-42c2-b037-009c9810cd63.png)
![s2](https://user-images.githubusercontent.com/33044667/149259220-e7f7cc45-26b8-43cd-ba37-68288afdd8c8.png)



