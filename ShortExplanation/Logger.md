### Logger
개념   
특정 시스템이나 어플리케이션에 메세지를 출력하기 위해 사용한다.   
개발 시 오류 발생 시점을 잡기 위한 디버그용으로 사용됨. 서버나 애플리케이션 관리 시 client의 개별 실행 정보를 확인 가능하다.   
<br>
특성   
level : 상황 별에 따른 로그 기록 시점을 의미   
- debug() : 개발자 debug 작업
- info() : 관리자 정보 활용
- warn() : 단순 경고
- error() : exception과 같은 경우
- fatal() : 치명적 에러(실행 불가능)
레벨을 debug로 설정하면 상위 레벨 메서드가 다 출력이 가능하다.   
<br>
appender : 로그 기록의 출력위치 설정   
전반적으로 목적지는 console창이나 개발자가 설정한 log 파일로 지정한다.   
console : ConsoleAppender // file : DailyRollingFileAppender   
<br>
layout : 출력 형식(pattern) 설정   
데이터 디자인은 개발자가 직접 설정하며 날짜, 시간, 로그 레벨, 클래스명, line, 메서드 값 등 설정 가능하다.
<br>
활용   
logger를 적용하고자 하는 곳(ex. ddd.class)에 객체 생성   
```java
Logger logger = Logger.getLogger("ddd.class");
```
```java
Logger logger = Logger.getLogger("ddd.class");
```
log를 출력할 때, +연산자를 쓰기 보다 { } 연산자를 사용하는 게 성능적으로 더 좋다.    
+연산자는 선언한 개수만큼 문자열 연산이 일어나기 때문이다.
```java
logger.debug("Hello {}.", userNm);
```
<br>
사용 시 주의할 점   
- Logger를 잘못 사용하면 애플리케이션 성능에 악영향을 준다. (개발 과정 중에는 안 보이고 성능 테스트에서 발견됨)
- System.out 또는 System.err을 사용하면 로그 레벨을 관리할 수 없고 성능 하락을 불러와서 문제가 된다.
- 같은 맥락으로 stack trace를 로깅할 때 ex.printStackTrace() 사용을 지양한다. 왜냐면 해당 메서드는 내부적으로 System.err을 사용해 로그를 남기기 때문이다.
- 성능이 중요한 애플리케이션이거나 긴 문자열 생성이 일어나면 가용 로그 레벨을 먼저 체크하는 방법을 사용해야 한다.
- 받아오는 파라미터가 3개 이상이면 Object[]가 생성되는 비용이 발생하기 때문에 되도록 2개 이하로 진행하는 게 좋다.  
만약 한꺼번에 가져오고 싶으면 User 클래스에서 toString을 override해서 전부 가져오는 기본 골격을 만들어줘야 한다.
