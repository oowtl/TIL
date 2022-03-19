# Node.js

> 비동기 이벤트 주도 JavaScript 런타임으로써 Node.js 는 확장성 있는 네트워크 애플리케이션을 만들 수 있도록 설계되었습니다.
>
> - [공식문서](https://nodejs.org/ko/about/)

- **Javascript를 브라우저 밖에서도 실행할 수 있도록 하는 Javascript의 런타임**
- 특징, keyword
  - 비동기 (Asychronous)
  - 이벤트 주도 (Event-driven)
  - Non-Blocking I/O



## Process & Thread

> Asychrounous, Non-Blocking I/O 에 대해서 설명하기 위해서 알아야하는 키워드 Process & Thread
>
> 자세한 지식은 차후에 개별적인 파트로 구분하고자 한다.



### Process

- 설명
  - 컴퓨터에서 연속적으로 실행되고 있는 프로그램
    - 프로그램이 실행되면 프로세스 인스턴스가 생성된다.
  - 메모리에 올라와 실행되고 있는 프로그램의 인스턴스(독립적인 개체)
    - 인스턴스가 생성된다는 의미는 프로그램 실행에 필요한 내용이 컴퓨터 메모리에 적재된다는 뜻이다.
  - 운영체제로부터 시스템 자원을 할당받는 작업의 단위
  - 동적인 개념으로는 실행된 프로그램
    - 엄밀하게는 프로세스와 프로그램은 다른 개념이다.
- 프로그램, 프로세스, 멀티 프로세스
  - 프로그램 : 어떤 작업을 하기 위해서 실행할 수 있는 파일 또는 프로그램
  - 프로세스 : 메모리제 적재되고 CPU 자원을 할당받아서 프로그램이 싱행되고 있는 상태
  
  - 멀티 프로세스 (Multi Process) : 하나의 프로그램을 여러개의 프로세스로 구성해서 각 프로세스가 하나의 작업을 처리한다.



### Thread

- 설명
  - 프로세스내에서 실행되는 여러 흐름의 단위
  - 프로세스의 특정한 수행 경로
  - 프로세스가 할당받은 자원을 이용하는 실행의 단위
    - 하나의 애플리케이션(프로그램)은 하나 이상의 프로세스를 가지고 있고,
      하나의 프로세스는 반드시 하나 이상의 스레드를 가진다.
    - 프로세스를 생성하면 기본적으로 하나의 스레드가 생성된다.


- 싱글 스레드, 멀티스레드
  - 설명
    - 일반적으로 하나의 프로그램은 하나의 스레드를 가지고 있다.
      프로그램의 환경에 따라서 둘 이상의 스레드를 동시에 실행할 수 있다.
      이것을 **멀티스레드**(multi Thread) 라고 한다.
  - 싱글 스레드 (Single Thread) : 하나의 프로세스에서 하나의 스레드를 실행한다.
  - 멀티 스레드 (Multi Thread) : 하나의 애플리케이션, 프로그램을 여러 개 의 스레드로 구성하여 하나의 스레드가 하나의 작업을 처리하도록 하는 것이다.



## Node.js is Single Thread

> Why Node.js is a Single Threaded language??

- Node는 싱글스레드가 아니다.
  Node도 여러 개의 스레드를 가지고 있다.
  하지만 JavaScript 를 실행하는 스레드가 하나이기 때문에 Node를 싱글스레드라고 한다.
  그리고 그 싱글 스레드가 바로 이벤트 루프이다.



## Non-blocking I/O



### what is I/O

- `I/O` 는 각각 Input 과 output 의 줄임말이다.
  `입출력` 이라고 한다.
- 컴퓨터 내부 또는 외부의 장치와 프로그램간의 데이터를 주고받는 것을 의미한다.
- I/O 모델에는 5가지가 존재한다.
  (서버 io 모델)
  1. Blocking I/O 모델
  2. Non-Blocking I/O 모델
  3. I/O Multiplexing 모델
  4. Signal Driven I/O 모델
  5. Asynchrounous I/O 모델













## 참고

- https://ko.wikipedia.org/wiki/Node.js
- https://velog.io/@nomoredopi/IO-%EB%AA%A8%EB%8D%B8
- https://medium.com/@vdongbin/node-js-%EB%8F%99%EC%9E%91%EC%9B%90%EB%A6%AC-single-thread-event-driven-non-blocking-i-o-event-loop-ce97e58a8e21
- https://juyoung-1008.tistory.com/47
- https://charlezz.medium.com/process%EC%99%80-thread-%EC%9D%B4%EC%95%BC%EA%B8%B0-5b96d0d43e37
- https://velog.io/@daeseongkim/Node.js-Node.js%EB%8A%94-%EC%8B%B1%EA%B8%80-%EC%8A%A4%EB%A0%88%EB%93%9C
- https://medium.com/@gwakhyoeun/%EC%99%9C-node-js%EB%8A%94-single-thread-%EC%9D%B8%EA%B0%80-bb68434027a3