---
title: "이진 검색"
excerpt: "Binary Component"
comments: true

categories:
  - Algorithm
last_modified_at: 2020-07-11
---
## 이진 검색(Binary Search)
##### 이론
이진 검색은 `정렬이 되어 있다는 가정 하에 있는 배열`에서 원하는 값을 찾는 방법이다.
배열의 중간값을 원하는 값과 비교해서 원하는 값보다 작으면 중간값을 포함한 작은 값 반을 버리고, 크면 중간값을 포함한 큰 값 반을 버린다.
위의 과정을 반복해서 최종적으로 원하는 값을 찾는다.
종료 조건은 원하는 값을 찾거나, 검색 범위가 더 없는 경우다.

##### 설정
예로 오름차순으로 정렬된 n개의 값이 있는 배열 a가 있다고 가정한다.
우리가 찾는 값은 x로 정한다.

제일 왼쪽은 pl로 0의 값을 가짐   
제일 오른쪽인 pr은 (n-1)의 값을 가짐   
중간값 pc는 (n-1) / 2의 값을 가짐   

그 다음 배열 내 a[pc]의 값을 x와 비교한다.   
a[pc] < x인 경우, pl에서 a[pc]까지의 값을 제외한다.   
a[pc] > x인 경우, pr에서 a[pc]까지의 값을 제외한다.

해당 동작을 값을 찾거나 검색 범위가 없을 때까지 반복한다.
이때 검색 범위가 없다는 건, pl = pr의 상황에서 x가 나오지 않으면, pl / pr값과 x값의 비교로 pl이 pr보다 오른쪽에 가거나 pr이 pl보다 더 왼쪽으로 간다.
그렇게 되면 검색이 종료되면서 실패가 된다.

<br>

### 정렬
이진 검색은 배열이 정렬되었다는 가정에 쓸 수 있는 기술이다.
이를 위해 데이터를 미리 정렬을 하기 위해 자연 정렬(natural ordering)을 쓸 수 있다.
자연 정렬은 사람이 더 쉽게 받아들일 수 있는 정렬 방법을 의미하고 Comparable<T> 인터페이스와 compareTo() 메서드를 쓴다.

하지만 자연 정렬은 정해진 순서로만(오름차순) 정렬이 가능해 다른 기준(내림차순 등)으로는 정렬을 할 수 없는 단점이 있다.
그럴 땐 자료형에 자유로운 <u>제너릭 메서드(generic method)</u>와 <u>Comparator 인터페이스</u>와 compare() 메서드를 쓰면 된다.

<br>

### 코드 구현
이진검색은 자바 라이브러리 `java.util.Arrays의 binarySearch` 메서드를 이용해 쉽게 구현할 수 있다.
원하는 값을 찾으면 반환을 해주되, 부합하는 값이 여러 개이면 무작위로 하나의 정답 배열을 반환한다.
실패를 하면 삽입 포인트 x에서 (-x -1)한 값을 반환한다.
여기서 삽입 포인트는 마지막으로 지정한 중간값보다 큰 첫번째 요소의 인덱스를 의미한다.   
예를 들어 마지막 중간값의 인덱스가 3이라면, 삽입 포인트는 인덱스 4인 값이 되고 반환값은 (-4 -1) = -5가 된다.

아래 코드는 이름과 키가 저장된 무작위 배열에서 키 순으로 정렬을 한 뒤에 사용자가 찾고 싶은 키를 입력하면 인덱스와 인덱스 내 이름, 키 정보를 불러온다.

<br>

변수 초기화, 생성자 생성, 정보 확인용 메서드
```java
import java.util.Arrays;
import java.util.Scanner;
import java.util.Comparator;

class HeightSearch {
  static class HeightData {
    private String name;
    private String height;
  }

  public HeightData(String name, String height){
    this.name = name;
    this.height = height;
  }

  public String toString(){
    return name + " " + height;
  }
```

<br>

Comparator를 이용해 키 순서로 데이터 정렬
```java
  public static final Comparator<HeightData>HEIGHT_ORDER = new HeightOrderComparator();

  private static class HeightOrderComparator implements Comparator<HeightData> {
    public int compare(HeightData d1, HeightData d2){
      return (d1.height > d2.height) ? 1 : (d1.height < d2.height) ? -1 : 0;
    }
  }
}
```

<br>

이진 검색으로 입력한 값 찾기
```java
public static void main(String[] args){
  Scanner stdIn = new Scanner(system.in);
  HeightData[] x = {
    new HeightData("이름들", 키);
      ... ... ...
  }
  System.out.print("키 입력 : ");
  int height = stdIn.nextInt();
  int idx = Arrays.binarySearch(x, new HeightData("", height), HeightData.HEIGHT_ORDER);

  if(idx < 0){
    System.out.println("그런 키 없습니다.");
  } else {
    System.out.println("x[" + idx + "]에 있습니다.");
    System.out.println("찾은 데이터 : " + x[idx]);
  }
}
```

<br>

##### 시간 복잡도(time complexity)
복잡도(complexity)는 알고리즘의 성능을 객관적으로 평가할 수 있는 기준이다.
두 종류가 있는데, 시간 복잡도는 수행이 걸리는 시간이고 공간 복잡도는 메모리나 파일 공간의 사용 정도를 가늠한다.

시간 복잡도는 n의 order O(n)으로 나타낼 수 있으며 n은 n에 비례하는 실행 횟수를 의미한다.
무조건 한 번 실행되는 기능은 n을 1로 나타내고 비례하는 값을 n으로 쓴다.
여러 O(n)이 있을 때는 차원이 더 높은 쪽의 복잡도를 우선시하기 때문에 가장 큰 n값을 가진 시간 복잡도를 측정 대상으로 쓴다.

`O(f(n)) + O(g(n)) = O(max(f(n), g(n)))`

이진 검색의 성능을 파악하기 위해 시간 복잡도를 측정할 수 있다.
한 번만 실행되는 라인의 시간 복잡도는 O(1)이고 나머지 라인은 <u>O(log n)</u>이 된다.
그러면 이진 검색 알고리즘의 복잡도는 O(log n)이다.

참고로 이진 검색에서 검색에 필요한 비교 횟수의 평균값은 log n<이 되고,
성공의 경우 (log n - 1)이 된다.   
실패의 경우(worst case scenario)엔 바닥함수를 적용한 log(n) + 1이 된다.

<br>

##### 내가 보기 위한 정리
<b>O(log n)</b>   
대문자 O는 Big O 표기법(notation)으로 동일한 알고리즘 로직으로 input의 크기가 커질 수록 걸리는 시간 측정 방법을 정의한 복잡도 표기 방법이다. log n은 Big O 표기법에 따른 계산법을 적용해 나온 결과다.   

n은 이진 탐색을 반복하면서 남아있는 자료의 개수를 표현한다.
처음 n으로 시작한 값은 시행을 거치면서 1/2를 곱하게 되고, k번의 시행을 마치면 [(1/2)^k] * n이라고 볼 수 있다. 최악의 경우에는 k가 무한대가 되면서 1과 가까워지고, 1이라고 표현할 수 있다.
거기서 2^k를 곱하고 log를 취하면 k = log2에 n을 곱한 값이 된다.
이는 곧 Big O 표기법으로 O(log n)이 된다.

출처 : <https://jwoop.tistory.com/9>
