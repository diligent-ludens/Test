---
title: "스프링 프로젝트 시작"
excerpt: "Chapter 2"
comments: true

categories:
  - Spring
last_modified_at: 2020-07-06
---
#### Spring을 이용한 자바 프로젝트 설치
저자는 메이븐(Maven) 또는 그레이들(Gradle) 중 하나를 선택해 생성하면 된다고 한다.   
잘 골라보기 위해 메이븐과 그레이들에 대해 알아보자.

메이븐
+ 자바 빌드 도구였던 아파치 앤트를 대체
+ 현재 가장 많이 쓰이고 있는 빌드 도구
+ 빌드 처리에 xml을 이용. (pom.xml)
+ 새로운 기능을 쉽게 설치하고 업데이트
+ 단점으로 프로젝트 규모가 커질 수록 설정 내용이 길어져 가독성이 떨어짐

그레이들
+ 빌드 처리로 groovy(JVM에서 실행되는 스크립트 언어, Java와 비슷함) 사용
+ 앤트(유연한 구성)와 메이븐의 특징을 섞었으며 안드로이드 OS 빌드 도구임
+ 각 특정 도메인에서 쓸 수 있는 언어를 필요로 해 나옴   
나은 IDE 환경을 위해 Kotlin 기반 DSL 제공   
DSL(Domain-Specific Language) : 도메인 특화 언어. 언어 지향 프로그램으로도 볼 수 있음.
+ 큰 프로젝트나 많은 dependency를 다룰 땐 메이븐보다 빠른 속도를 냄   
Incrementality : 작업 입출력을 추적해 필요한 것만 실행    
Build cache : 동일한 입력은 다른 Gradle 빌드의 출력물을 재사용   
Gradle Daemon : 빌드 정보를 메모리에 보관해 프로세스를 장기간 유지   
+ 의존성 커스터마이징이 가능

찾아볼 수록 그레이들에 대한 유용성이 더 부각되었다.   
나중에 배워보고 싶은 Spring Boot과 많이 쓰이는 걸 보고 그레이들을 설치해 배워보기로 했다.

##### 그레이들 프로젝트 생성
Phase 1.   
저자는 build.gradle 파일을 생성해 그 안에 플러그인, spirng-context 모듈, gradle wrapper 설정을 작성하라고 한다.   
나는 그대로 따라하고 cmd 명령어를 넣었는데, 계속 에러가 난다. Wrapper 관련해 deprecated 에러가 뜬다   
>Deprecated Gradle features were used in this build, making it incompatible with Gradle 7.0

cmd가 warning을 세부적으로 알려주는 명령어로 알아보려 했지만 인지를 못해서... 어쩔 수 없이 다른 길을 찾아나섰다.

Phase 2.   
구글에서 찾은 `cmd`로 설정을 한 큐에 끝내는 방법이 있어 시도해 봤다.
```
cd C:\spring5fs
mkdir sp5-chap02
cd sp5-chap02
gradle init
```
이 다음에 여러 번의 선택지가 뜨는데,   
2. application
3. Java
1. Groovy
1. JUnit 4   
Project name : Enter키 (Enter키를 누르면 default 설정으로 진행)   
Source package : chap02   
순서로 골라주었다.

`BUILD SUCCESSFUL`이 뜨면 루트 폴더(sp5-chap02)에 gradlew 파일, gradlew.bat, gradle 폴더가 생성된다.   
책의 설명으로는 gradlew가 그레이들의 설치 없이 명령어를 실행할 수 있는 wrapper 파일이라고 한다.   
나중에 공유하고 싶으면 위의 파일 2개와 폴더 1개를 공유하면 된단다.   

그 다음 자바 컴파일을 실시한다.
```
gradlew compileJava
```

이제 빈 객체를 생성해서 프로그램을 만드려 하는데 아래 두 라인에 빨간 줄이 그려진다.

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

그래서 찾아보니,   
프로젝트 하위 폴더 중 `Project and External Dependencies`에 들어가면 책에 나와있는 `spring-`로 시작하는 jar 파일들이 없다.   
책에 나와 있는 build.gradle 파일에 dependencies로 springframework를 compile하는 라인이 없어서 그런 거다.   

Phase 3.   
첫 시도 때 문제가 있던 wrapper 라인 쪽으로 수정 방향을 바꿨다.

스프링에서 제공하는 설치법에 cmd로 wrapper와 그레이들 버젼을 함께 쓰는 명령어를 찾았다.   
사이트 : <https://spring.io/guides/gs/gradle/>   
gradle.build에서 task wrapper 파트만 없앤 다음, cmd로 명령어를 넣으니 된다.   
```
gradle wrapper --gradle-version 5.6.1
gradle compileJava
```
심지어 나중에 이클립스에 import 하고 나니 내가 원했던 jar 파일들이 고스란히 있다!!<br>
에러 뜰 때 좀 더 고민해 볼 걸...

##### 스프링 객체 컨테이너
책에서 소개된 일부 의존 그래프에 대한 설명을 적어본다.

스프링의 주된 기능은 <b>객체 생성 및 초기화</b>이다.

의존 그래프 최상위에 있는 `BeanFactory` 인터페이스는 객체 생성과 검색을 기능이 있고, 싱글톤 혹은 프로토타입 빈 확인 기능도 겸비한다.   

>여기서 싱글톤(Singleton)이란,
해당 클래스에서 하나의 bean 객체만 만들어져 단일 객체의 범위를 갖게 되는 거다.   
별도로 설정하지 않으면 1개의 @Bean annotation에 1개의 bean 객체를 생성한다.

생성된 객체를 검색하는 데 쓰이는 `getBean()`메서드가 정의되어 있다.

BeanFactory보다 두 칸 아래 있는 `ApplicationContext`는 메시지, 프로필 및 환경변수를 처리한다.

BeanFactory와 ApplicationContext는 <b>Spring container</b>라고 불리는데, bean 객체와 이름을 연결해주는 역할을 하기 때문이다.
ApplicationContext 같은 경우 bean 객체 생성, 초기화, 보관, 제거, 관리를 한다.

<br>
의존 그래프에서 가장 하단에 있는 구현 클래스다.
간단한 코드 작성 때 쓴 `AnnotationConfigApplicationContext` 클래스는 자바 annotation을 이용해 클래스로부터 객체 정보를 가져와 객체 생성과 초기화를 수행한다.   

GenericXMLApplicationContext는 XML로부터 정보를 불러올 때, GenericGroovyApplilcationContext는 Groovy 코드를 통해 정보를 불러올 때 사용된다.
