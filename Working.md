### 코루틴이 스레드보다 빠른 이유

코루틴은 스레드에서 실행된다. 이것은 대원칙이다. 코루틴이 단독으로 또는 다른 수단에 의해 실행될 수 없다는 것을 명심하자. 그리고 여러개의 코루틴을 하나의 스레드를 지정하여 실행할 수 있지만 동시에 실행 하는 것은 불가능하고, 한번에 하나의 명령어만 실행할 수 있다. 그 이유는 코루틴은 스레드내에서 실행되고 중단 지점(suspension point)에 도달하자마자 스레드를 떠나 대기중인 다른 코루틴을 선택할 수 있도록 해방하기 때문이다. 이렇게하면 스레드와 메모리 사용량이 줄어들어 많은 동시성 작업을 수행할 수 있게 된다.

좀 더 낮은 관점에서 살펴보면 코틀린의 코루틴은 스택리스 코루틴(Stackless Coroutine)이다. 스택리스 코루틴은 스택이 없다는 의미다. 또한 특정 스레드에 종속되지도 않는다. 그렇기 때문에 프로세서에서 Context Switching이 필요하지 않다. 이러한 이유들로 인해 수천개의 스레드를 생성하는 것보다 수천개의 코루틴을 생성하는 것이 더 빠르고, 자원을 적게 사용한다. 

> 프로세스나 스레드가 인터럽트에 의해 다음 우선순위를 가진 프로세스나 스레드가 실행될때 기존의 정보들은 PCB에 저장이되고 새 프로세스나 스레드의 정보를 가져와서 교체하는 작업을 Context Swiching이라 한다.   
> 스레드는 Heap, Code, Data 영역을 공유하고 Stack 영역만 자신의것을 가지고 있기때문에 Context Switching에서 프로세스보다 더 가볍다. 
> 그런데 코루틴은 스택이 없다. 즉 컨택스트 스위칭을 위해 정보를 저장하고 교체할 필요가 없기때문에 빠르다.  
출처 : https://www.charlezz.com/?p=44634


#### Process Control Block
A process control block (PCB) is a data structure used by computer operating systems to store all the information about a process.  
It is also known as a process descriptor.   
When a process is created (initialized or installed), the operating system creates a corresponding process control block. 
OS 차원에서 관리 해줌.
   
