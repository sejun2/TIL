## Dart 언어의 특징
 - AOT(Ahead Of Time) 컴파일 방식. fast, predictable, native code 를 위함. Flutter 를 fast하게 해줄뿐 아니라 모든것들을 customizable하게 해줌
 - Also JIT(Just In Time) 컴파일 방식 지원. 
 - 싱글 스레드
 - 매끄러운 애니메이션과 transition등을 가능하게 해줌 60fps 속도를 보장하며.
 - 객체를 할당하고 가바지컬렉션을 lock 없이 할수있음. 플러터는 native 코드로 변환되기때문에 bridge같은 영역이 필요없음
 - 분리된 layout언어를 가질 필요가없음. 
 - 배우기 쉬움.


`JIT 컴파일 방식은 개발도중에 사용됨. 빠르기때문. release 시에는 AOT 로 컴파일됨.  두가지 컴파일방식을 지원함으로써 개발시에는
빠르고 Release 시에는 안정적임.
`


