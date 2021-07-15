``` dart
Future<Album?> fetchAlbumTest(http.Client client) async {
  final response = await client
      .get(Uri.parse('test'));

  print('response : ${response.body}');
  if (response.statusCode == 200) {
    // If the server did return a 200 OK response,
    // then parse the JSON.
    return Album.fromJson(jsonDecode(response.body));
  } else {
    // If the server did not return a 200 OK response,
    // then throw an exception.
    throw Exception('Failed to load album');
  }
}

@GenerateMocks([http.Client]) // http.Client Mock 객체 생성 요청 어노테이션
void main() {
  group('fetchAlbum', () {
    test('returns an Album if the http call completes successfully', () async {
     final client = MockClient();

      when(client.get(Uri.parse('test'))).thenAnswer((realInvocation) async =>
      http.Response('{"userId": 1, "id": 2, "title": "mock"}', 200));

      var result = await fetchAlbumTest(client);

      expect(result, isA<Album>());
    });
    test('throw exception if the http call not succeeded', () async {
      final client = MockClient();

      when(client.get(Uri.parse('test'))).thenAnswer((realInvocation) async =>
      http.Response('fail', 501));
      //피 테스트 함수에서 예외를 던지는경우,
      //테스트 함수에서 try catch 를 통해 예외구문마다 expect를 다르게 설정해서 처리한다.
      try {
        var result;
        result = await fetchAlbumTest(client);
        expect(result, isA<Album>());
      } on Exception catch (e) {
        expect(e, isA<Exception>());
      }
    });
  });
}
```

1. Mocking 할 객체를 GenerateMocks어노테이션에 추가한다.
2. flutter pub run build_runner build 명령어로 해당 클래스를 추가시킨다.
3. 추가된 Mock객체는 Mock객체(); 형식으로 사용할 수 있다.
4. when().thenAnswer() 메서드로 Mock 객체가 수행할 행동을 정의한다.
5. expect()로 기댓값과 수행값을 비교한다.

참고 : https://pub.dev/packages/mockito
