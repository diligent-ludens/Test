---
title: "블로그 설정 바꾸기"
excerpt: "하다 보면 신나게 바꾸고 있는 나를 발견한다."
comments: true

categories:
  - GitHub
last_modified_at: 2020-07-02
---
##### 주의
minimal-mistakes의 폴더/파일 이름 앞에 underbar가 쓰여도 이 글엔 포함시키지 않았습니다.   
Atom으로 작업하다 underbar를 쓰면 다음 글이 모조리 보라색이 되더군요.

<br>

#### 계기
처음 블로그용으로 쓴 글을 읽으려 두근대는 마음으로 블로그 첫 화면을 띄웠을 때 꽤 경악했다.   
글자가 내가 어릴 때 읽던 동화책 글자보다 더 크게 나온다.   
양쪽에 빈 공간이 가뜩이나 큰 글씨를 중간으로 몰고 있어 더 그랬다.   

나의 멘탈과 가독성을 위해 처리하기로 했다.   

<br>

#### 여정
##### 1. 오른쪽 여백 없애기
minimal-mistakes를 만든 분의 layout 문서에 나와 있다.   
<https://mmistakes.github.io/minimal-mistakes/docs/layouts/>
조금 내려가면 Single Layout 파트에 wide page 만드는 법이라고 쓰여 있다.   

> To expand the main content to the right, filling the space of what is normally occupied by the table of contents. Add the following to a post or page's YAML Front Matter:

`
classes: wide
`

그냥 저 한 줄을 config 파일 내 Defaults의 post와 pages layout 밑에 넣어 주면 적용된다.

<br>

##### 2. 폰트 사이즈 조정
`sass/minimal-mistakes/reset.scss` 파일에 들어가 font-size를 바꿨는데도 적용이 되지 않았다.

마침 찾아보니 개발자의 질의응답 섹션에 친히 본인이 알려준 방법이 있다.   
<https://github.com/mmistakes/minimal-mistakes/issues/1219>   
괜히 sass 파일 손대지 말고 html font-size 코드를 `assets/css/main.css`에 추가하라고 한다.   
이러면 font size를 cascade와 override가 가능하다고.   
원하는 크기로 조정만 해주면 된다.

`
html {
  font-size: 12px;
        ...
}
`

적용해보는 데 아무리 새로 고침을 해도 변경되지 않아 당황했는데, 질의응답 밑에 나처럼 당황하는 사람이 있었다.   
개발자는 GitHub의 웹 캐시(web cache) 문제가 꽤 있으니 좀 기다렸다 다시 오라고 한다.   
실제로 5분 뒤에 새로 고침을 하니 그땐 적용이 된다.

> 웹 캐시는 웹 사이트에 접속할 때 정적 콘텐츠(HTML, CSS, 이미지 등)를 특정 위치에 저장한다. 필요할 때 매번 서버에서 다운 받지 않고 저장된 위치에서 불러오니까 응답시간과 서버 트래픽 감소를 노릴 수 있다.

<br>

##### 3. 폰트 종류 변경
사이즈를 바꾸니 한글 폰트가 묘하게 걸린다.
그럼 바꿔야지.

Google에서 오픈 소스로 폰트를 제공해준다.
거기서 나는 Noto Sans KR을 골랐다. 영어와 한글이 둘 다 되고 글씨체도 깔끔해서 보기 좋았다.
아래에 보면 폰트 페어링이라고 추천해주는데, 디자인소리에 의하면 2020년 디자인 트렌드란다.
한 프로젝트에 여러 글꼴을 섞어서 가독성과 흥미를 올려준다고 한다.

스타일 중 아무거나 `+ Select this style`을 누르면 사용할 폰트가 추가된다.
다 고르면 Review 옆에 Embed를 클릭해 '@import'를 선택한다.
거기 나온 내용을 복사해 `assets/css/main.scss`에 넣어 준다.

그 다음 sass/minimal-mistakes/variable.scss에 들어가 system typefaces 아래에 추가한 폰트를 적어준다.   

`
...
$sans-serif: -apple-system, BlinkMacSystemFont, "Roboto", "Noto Sans KR", "Segoe UI",
	"Helvetica Neue", "Lucida Grande", Arial, sans-serif !default;
...
`

웹 캐시를 다시 기억하며 다른 일하다 돌아오면 적용이 되어 있다.

#### 후기
처음엔 별 생각 없이 시작했는데 내가 원하는 대로 변해가는 블로그를 보니 엄청 신난다.   
시간 나면 틈틈이 바꿔보고 싶다.
