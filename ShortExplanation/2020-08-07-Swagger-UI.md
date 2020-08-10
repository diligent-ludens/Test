---
title: "Swagger UI"
excerpt: "이 세상엔 알아야 할 게 엄청 많다!"
comments: true

categories:
  - API
last_modified_at: 2020-08-07
---
### 사담
코딩을 하겠다고 본격적으로 뛰어든 건 1년도 안 되지만 나에겐 숙원사업이 몇 개 있다.
그 중 하나가 국비지원 때 진행했던 세미 파이널 프로젝트를 수정하는 일이다.

한창 이상은 높고 실력은 바닥일 때 진행했던 세미 파이널은 많은 걸 알려줬지만 나에게 좌절감도 많이 안겨줬다.
분명 배운 개념이고 열심히 했지만 에러를 다 못 잡고 미완성으로 끝낸 작품이라 아쉬움도 많이 남는 프로젝트다.
백엔드 개발자로 나아가겠다 정한 후 아무리 생각해도 저 프로젝트를 정상적으로 굴러가게는 만들고 싶다는 생각이 들었다.
영화에서 과거의 기억으로부터 도망치다가 결국 다시 돌아가 맞서 멋있게 승리한 주인공처럼, 나도 도전해보려 한다.

<br>

### 왜 Swagger UI인가?
어쩌다가 마주치게 된 기능이다.
보자마자 내 프로젝트에 한 번 써보고 싶다고 생각했다.
왜냐면 프로젝트 작업을 하며 제일 힘든 게 너무 많은 페이지와 url이 있어 한 눈에 전부 구동이 되는지 파악하기 힘들었다.
하지만 Swagger UI로 url을 시각화 해 전부 다 잘 작동하는지 볼 수 있다.    
대신 그러려면 Spring/SpringBoot와 Maven/Gradle 같은 프로젝트 관리 도구가 필요하다.

<br>

### Swagger UI란?
API spec문서를 자동화해주는 기술이다.
지정한 URL을 HTML 화면으로 확인할 수 있고 API 메서드 기능 테스트도 가능하다.

브라우저에 띄우면 내가 지정한 Controller와 각 Controller에 대한 test 기능, 요구하는 파라미터, 반환하는 값에 대해 보여준다. 

<br>

##### 설정
내가 본 적용법은 SpringBoot와 Gradle을 이용한 방법이다.
참고 사이트 : <https://www.notion.so/Swagger-UI-759e9c7b5240408caf93d34dd8a5a986#fa953de37db746b2b0b7f1e6f6ca03bf>

Step 1. 의존성 추가
build.gradle에서 작성한다.
```
compile group: 'io-springfox', name: 'springfox-swagger2', version '2.5.0'
compile group: 'io-springfox', name: 'springfox-swagger-ui', version '2.5.0'
```

Step 2. 프로젝트에 Swagger 설정 Bean을 등록한다.
보통 `SwaggerConfiguration.java`라는 파일 명칭을 쓰는 것 같다.
```
@Configuration
@EnableSwagger2
public class SwaggerConfig {

    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
                .select()
                .apis(RequestHandlerSelectors.any()) 
                .paths(PathSelectors.ant("/api/**")) 
                .build();
    }
}
```
여기서 PathSelectors.ant("/api/**")는 /api/ path를 가진 url만 공개한다는 뜻이다.
다른 url을 쓰고 싶으면 /api 부분을 없애면 된다.

Step 3. 브라우저에 띄우기
url에 프로젝트에 쓰이는 로컬 호스트 번호와 swagger-ui를 부르면 된다.
예를 들어, 번호가 8080이라면 `localhost:8080/swagger-ui.html`이라 쓰면 된다.

### TBC
아직 모은 자료가 빈약해서 실제로 구동을 시킨 다음 다시 써보기로 하겠다.
