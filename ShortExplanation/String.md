### String
constant 객체로 바뀔 수 없다. 즉, immutable해서 문자열을 더하면 새로운 String 객체가 생성되고 기존에 사용하던 객체는 참조가 불가능해진다.   
바뀌려면 mutable한 StringBuilder나 StringBuffer를 써야 한다.   
null값을 String에 주게 되면 NullPointerException 예외가 발생한다.

##### StringBuilder
도큐먼트에서 StringBuffer와 같은 기능을 쓰지만 보다 더 빠른 성능을 내기 때문에 권장된다.
대신 멀티 쓰레드 상황에서는 safe하지 않기 때문에 StringBuffer를 권장한다.
모든 자료형을 다룰 수 있다.   

사용 가능한 method들    
append : 저장된 builder 뒤에 문자열 추가
insert : 특정 지점에 문자열 추가

##### StringBuffer
문자열을 수정(mutable)할 수 있는 클래스로 멀티 쓰레드 환경에서 안전하다. (thread-safe)   
> The methods are synchronized where necessary so that all the operations on any particular instance behave as if they occur in some serial order that is consistent with the order of the method calls made by each of the individual threads involved.
모든 자료형을 다룰 수 있다.

String 내부의 문자열은 수정할 수 없다. 문자열을 추가하게 되면 새로운 객체를 생성해 기존에 있던 문자열 뒤로 붙는 형식으로 산출된다.   
계속 추가하면서 String 객체 수가 늘어나면서 성능을 저하시킨다.   

문자열 변경 작업에 StringBuffer나 StringBuilder를 쓰는 게 좋다.   
내부 버퍼(buffer, 데이터를 임시로 저장하는 메모리)에 문자열을 저장해 추가, 수정, 삭제 작업을 할 수 있게 해준다.   

StringBuffer는 멀티 쓰레드 환경에서, StirngBuilder는 단일 쓰레드 환경에서 사용할 수 있다.   
연산자 +로 String에서 StringBuilder로 변환한다.   
JDK 1.5 버전 후부터 String에 `+`연산을 쓰면 컴파일 때 StringBuilder로 자동 변환을 해 성능 최적화가 이뤄진다.   

컴파일한 코드의 소스파일을 디컴파일해 성능 최적화를 확인할 수 있음   

**성능 최적화**가 이뤄지는 경우   
하나의 String을 선언하고 +연산자로 디컴파일 했을 때 하나의 String으로 변환   
대신 StringBuilder로 변환되도 **성능 저하**가 발생할 수 있다.   

상황1. 여러 줄에 걸쳐서 선언   
상황2. for문으로 여러 번 선언   

위 상황들은 String 객체를 여러 번 호출해 컴파일 때 같은 수의 StringBuilder 객체가 생성되는 경우다.   

하지만 StringBuilder로 **자동 변환되지 않는 경우**도 있다.   
상황3. concat() 메서드 사용   
