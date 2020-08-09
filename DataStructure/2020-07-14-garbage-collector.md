---
title: "Garbage Collector"
excerpt: "네, 그냥 두는 기능이 아니었습니다."
comments: true

categories:
  - Java
last_modified_at: 2020-07-14
---
### 개요
인터넷에서 이리저리 부유하다가 JDK 15 버젼이 나왔다는 기사를 보게 됐다.   
링크 : <https://www.infoworld.com/article/3534133/jdk-15-the-new-features-in-java-15.html>   

업그레이드로 새로 추가된 기능을 소개하는 란이 길게 있다.
무슨 기능인지 감이 잘 안 잡혀서 내용을 훑고만 있었는데, `Z Garbage Collector(ZGC)`라는 게 눈에 들어왔다.
그동안 타 버전에서 조심스럽게 적용하고 있던 실험적 기능을 이번에 완전히 도입하겠다는 내용이었다.
가비지 콜렉터의 기능이 중요하다고 최근에 들은 참이라, 이번에 한 번 제대로 알아보자 싶었다.

<br>

### Garbage Collector(GC)란?
원체 나의 짧은 지식에 의하면 GC는 자바에서 제공하는 기능으로 메모리에 할당되었지만 더 이상 쓰지 않는 쓰레기 객체를 자동으로 찾아서 제거해준다. (끝!!!)

자바에서 제공하는 Hotspot JVM은 높은 퍼포먼스와 큰 확장성을 제공하는 아키텍쳐다. 해당 구조에서 퍼포먼스 튜닝에 중추적 역할을 맡는 기능으로 힙 영역, JIT, GC가 있다.

자동 제거 기능을 자세히 보면 JVM 힙 영역에서 `Marking`과 `Deletion`이라는 2가지 과정을 거친다.
Makring 때 GC는 메모리에서 살아있는 객체와 참조되지 않는 객체를 구분해서 마킹한다.
Deletion 과정은 두 가지 종류가 있다.
Normal deletion은 쓰레기 객체를 없앤 다음, 제거하고 남은 빈 공간을 memory allocator가 참조하도록 한다. 나중에 새 객체가 들어올 때 참조한 자리에 넣어준다.
Deletion with compacing은 쓰레기 객체를 제거한 다음 살아 있는 객체를 한 곳에 모아서 정리한다. 그러면 새 객체를 더 쉽고 빠르게 넣을 수 있다.

객체 할당 방식을 보면서 JVM 퍼포먼스 향상을 시도할 수 있다. 이때 알아야 할 개념으로 힙 영역에 있는 세부 영역들이다. 이들은 `Young Generation`, `Old(Tenured) Generation`, `Permanent Generation` 로 불린다.

##### Young Generation
새 객체가 할당되는 곳이다. 여기는 `eden`과 `survivor space(S0, S1)` 2개로 총 3개가 있다.
어느 정도 차게 되면 `minor garbage collection`이 발생한다. 실행하는 이유는 새 객체가 금방 없어질 거라는 가정 하에 있기 때문이다. 이렇게 minor gc 이후에도 살아 남은 객체는 Old Generation으로 이동한다.
모든 minor gc는 `Stop the World` 이벤트라고 한다. 이유는 이 작업이 끝날 때까지 모든 애플리케이션 쓰레드가 멈춰야 한다.

##### Old Generation
오랫동안 살아 남은 객체가 이 영역에 보관된다. 여기서는 `major garbage collection`이 발생한다. major gc도 Stop the World 이벤트인데, 살아있는 모든 객체도 확인하기 때문에 진행 속도가 훨씬 느리다. <u>major gc를 조율하면 퍼포먼스 향상에 도움이 된다.</u>

##### Permanent Generation
애플리케이션에서 쓰이는 클래스와 메서드의 메타 데이터를 보관하는 영역이다. 자바의 라이브러리 클래스와 메서드도 여기 담긴다. JVM이 더 이상 쓰지 않는다고 인식된 클래스를 모아서 처분한다.

<br>

이제 세 영역의 특징에 대해 가볍게 봤으니까 어떻게 작동하는 지를 본다.

<b>Step</b>   
1번째 : 새 객체는 eden에 할당되며, 2개의 survivor space는 빈 채로 시작한다.   
2번째 : eden이 어느 정도 차면 minor gc가 발동한다.   
3번째 : 살아 있는 객체는 첫번째 survivor space(S0)로 가고 참조되지 않는 객체는 없어진다.   
4번재 : 다음 minor gc에서 참조된 객체와 S0에 있던 객체 모두 S1으로 이동한다. 그리고 Eden가 S0은 비워진다.   
5번째 : 그 다음 minor gc에서 S1에 있던 객체와 참조된 객체가 S0으로 이동하고 Eden과 S1이 비워진다.   
6번째 : 3번째에서 5번째 동작을 어느 정도 반복하고 나면 숙성(aged)된 객체는 old generation으로 승격한다.   
7번째 : old generation에도 어느 정도 객체가 쌓이면 major gc가 작동된다.

<br>

### 왜 중요한가?
여기서 GC라는 기능 자체보다 왜 자동으로 두면 안 되는지를 알아야 한다.   
GC는 자바 애플리케이션의 퍼포먼스에 영향을 주는 기능이다. 자동 GC가 발동할 때마다 모든 메모리를 스캔해서 객체를 마킹할 때 많은 CPU 파워를 쓰면서 퍼포먼스 저하를 일으킨다. 또한 GC 작업으로 애플리케이션 멈춤 현상도 일어날 수 있다. 마지막으로 자동 작업이라 개발자가 객체 제거를 조율할 권리가 주어지지 않으며 수동적으로 효과적인 메모리 관리를 할 수 없다는 점이다.

따라서 퍼포먼스 최적화를 위해 JVM의 GC 활동을 모니터하면서 조정해주는 게 좋다. 고려해볼 사항으로는
+ 언제 GC가 작동되는가
+ 얼마나 자주 작동되는가
+ 한 번 발동할 때 얼만큼의 메모리가 수집되는가
+ GC가 얼만큼의 시간 동안 작동하는가
+ 어떤 GC가 작동하는가 (부분 또는 전체)
+ CPU 사용량   
...   
이 있다.

상황을 모니터하려면 JDK에서 제공되는 Visual VM 프로그램을 쓰면 된다. Visual GC 플러그인을 다운 받아서 사용하면 된다.

추후 새로운 내용을 보게 되면 추가하겠습니다.

<br>

### Z Garbage Collector란?
ZGC는 짧은 대기 시간과 확장성을 제공하는 GC다. 여러 애플리케이션을 10ms 이상 멈추지 않으면서 일을 수행할 수 있다.   
ZGC에서 가장 중요한 튜닝 옵션은 최대 힙 사이즈 `-Xmx`를 정하는 거다. 무작정 크게 정하기 보다 메모리 사용량과 GC가 실행되는 횟수를 감안해서 적당한 사이즈를 찾는 게 중요하다.
두번째 튜닝 옵션으로 동시에 실행되는 GC 쓰레드 개수 `-XX:ConcGCThreads`를 정하는 거다. ZGC가 자체적으로 개수를 정하지만 GC 수행 시 사용하는 CPU를 고려해서 수동으로 조정할 일도 생긴다.

<br>

##### 참고 자료
+ <https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html>
+ <https://d2.naver.com/helloworld/1329>
+ <https://docs.oracle.com/en/java/javase/11/gctuning/z-garbage-collector1.html#GUID-CD1DF73A-11D2-4478-BE14-20CBF8DA2830>
+ <https://www.eginnovations.com/blog/what-is-garbage-collection-java/>
