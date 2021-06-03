## 프로세스
프로세스는 일을 처리하는 일련의 과정이다.  
  
컴퓨터에게 프로세스란, 
1. 운영체제로 부터 시스템 자원을 할당받는 작업의 단위
2. 컴퓨터에서 연속적으로 실행되고 있는 프로그램
3. 메모리에 올라와 실행되고 있는 프로그램의 인스턴스를 말한다.

하나의 프로세스는 크게 Code, Data, Stack, Heap 영역으로 이루어져 있다.  
  
![이미지](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcarpYF%2FbtqBBVC4OfH%2FXPDhK0kHukAupHu85JZsU1%2Fimg.png)
- Code : 코드 자체를 구성하는 영역(프로그램 명령)
- Data : 전역변수, 정적변수, 배열 등 초기화된 데이터
- Stack : 지역변수, 매개변수, 리턴 값 등 임시 메모리 영역
- Heap : 동적할당 시 사용 ex) new ~(), malloc 등

  
## 스레드
한 프로세스 내에서 동작되는 여러 실행 흐름.
프로세스 하나에 자원을 공유하면서 일련의 과정들을 여러개의 단위로 실행시키는것.  

스레드는...
- 한 프로세스 내의 주소 공간이나 자원들을 대부분 공유
- 기본적으로 하나의 프로세스가 생성되면 하나의 스레드가 같이 생성되며, 이를 메인스레드라고 한다. 스레드를 추가로 생성하지 않는 한 모든 프로그램 코드는 메인 스레드에서 실행된다.
- 하나의 프로세스는 여러 개의 스레드를 가질 수 있으며 이를 멀티 스레드라고 한다.

멀티스레드의 구조는 Code, Data, Heap 영역을 스레드들이 공유하며, Stack 영역만 스레드별로 따로 가진다.
![멀티스레드의 구조](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc03dAx%2FbtqBEz6o9Lb%2FiCB5si14jlPNFXT5701sx1%2Fimg.png)
  
## 멀티 프로세스와 멀티 스레드
![멀티 프로세스](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FApUCF%2FbtqBD1a0SBH%2Fi05f8OsvEVM1gayzod7HRK%2Fimg.png)

여러 CPU 코어 동작 -> 하나 이상의 프로세스들을 각 코어에서 동시에 처리(병렬)   
- 장점 : 독립된 구조이기 때문에 안정성이 높다. Critical section 문제 없음   
- 단점 : 각각 독립된 메모리 영역을 갖기 때문에, 작업량이 많을수록 오버헤드가 발생한다. 또한 Context 스위칭으로 인한 성능 저하문제가 있다.   
   
![멀티스레드의 구조](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbuHX3v%2FbtqBBUc8DLD%2FhsXLhRppL5L9TZfUNXzXtk%2Fimg.png)
   
- 장점 : 프로그램의 응답시간 단축, 자원 소모 감소(Stack을 제외한 메모리를 공유하기 때문), 프로세스간 통신 방법에 비해 스레드간 통신방법이 간단, Context 스위칭으로 인한 성능저하 감소  
- 단점 : 스레드는 메모리를 서로 공유하기때문에 동기화 관련 문제가 발생할 가능성이 높음
