---
title: "GitHub 블로그 만들기"
excerpt: "Jekyll을 통해 GitHub 블로그 설치"
comments: true

categories:
  - GitHub
last_modified_at: 2020-06-29
---

### 목적   
- GitHub에 올린 프로젝트 과정, 맞닥뜨린 문제 및 해결 방법과 느낀 점 등을 기록하기 위함

인터넷에 잘 설명된 글이 많아서 과정에 생략된 부분이 많다는 점을 감안하고 읽어야 한다.

### 설치
#### Repository 생성   
GitHub에서 내 아이디.github.io라는 이름의 repository를 만든다. 나는 .io 대신 .com을 사용해도 된다 해서 .com으로 지었다.    
<br>
그런데 나중에 블로그 링크를 쓸 때 확장자를 .com으로 쓰면 404 페이지가 떴다. 거기선 .io로 써줘야 한다.   
repository 주소를 clone한 걸 보면 확장자는 무조건 .io로 고정되는 것 같다.   
<br>
만들어진 repository 주소를 복사해 원하는 디렉토리에 clone 한다. 나는 git bash를 사용했다.

```bash
$ git clone https://github.com/heejahn/heejahn.github.io.git
```

#### 테마 고르기
Jekyll theme site : <https://jekyllthemes.io/free>   
해당 사이트에 무료로 받을 수 있는 테마가 수두룩하게 있다. 최종 선택은 minimal-mistake로 했다.   
Now가 1순위긴 했지만 minimal-mistake가 구글에서 가장 많이 언급되어 문제가 생겼을 때 해결하기 수월해 보여 선택했다.

#### 테마 respository 끌어오기
처음엔 fork를 하려고 했다. 근데 fork보다 latest release를 zip file로 받는 게 더 안정적이라고 한다.   
그래서 노선을 틀어 zip 파일을 받아 로컬 저장소에 그대로 복사해 붙였다.   
latest release : <https://github.com/mmistakes/minimal-mistakes/releases>

#### Ruby 설치
Jekyll은 Ruby 기반이라 설치는 필수다.   
Ruby 설치 사이트 : <https://rubyinstaller.org/downloads/>   
설치 사이트 옆 컬럼에 친절히 DevKit이 포함된 버전을 추천한다.    
호환 가능한 Gem을 가장 많이 제공하고, C/C++ 확장 모듈이 있는 Gem 컴파일이 가능하다는 이유였다.   
권장하시니 그걸로 받았다.

#### Jekyll bundler 설치
cmd를 작동시켜 로컬 저장소가 있는 디렉토리로 이동 후 설치한다.
```console
cd C:\Users\user\Documents\GitHub\heejahn.github.io
gem install bundler
bundle exec jekyll serve
```
Server address와 Server running...이 뜨면, 웹 주소에 localhost:4000을 넣어 본다.  
지금까지 실수가 없다면 설정 전 블로그 페이지가 뜬다.

#### 블로그 커스터마이징
minimal-mistake 페이지에 삭제해도 되는 파일 리스트가 있으니, 참고해서 지워준다.   
시작 가이드 페이지 : <https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/>   
가이드 페이지에 진짜 하나부터 열까지 다 가르쳐주기 때문에 찬찬히 읽고 실행하면 된다.   
블로그를 꾸며주기 위해 _config.yml, _pages, _data/navigation.yml을 설정한다.

#### GitHub에 push
다 됐으면 대망의 첫 포스팅을 거행한다!
```bash
$ git add .
$ git commit -m "first post"
$ git push origin master
```
...라고 했지만 뜻하지 않게 에러가 나왔다.   
<br>
에러 메시지에서 `Note about fast-forwards`을 참고하라 해서 찾아봤다.   
읽어보니 GitHub에 만든 README.md를 나의 로컬 저장소는 commit이 된지 인식을 못한다고 써있다.   
GitHub와 로컬 저장소의 인식 부조화인 듯 했다.   
<br>
Stack Overflow에서 알려준 방법으로 우선 push에 성공은 했다.
```bash
$ git push -f origin master
```
문제는 전체 파일이 다 같이 commit이 되서 GitHub에 있던 기존 README.md도 덮어졌다.    
미래에 세부사항 변경 때마다 전체 commit을 한다는 건 좀 아니지 싶다.      
그래서 GitHub의 파일을 로컬 저장소에 pull해 둘의 싱크를 맞춘 다음 다시 push를 시도하니 된다.

### 후기
이제 나는 GitHub 블로그를 만들기 힘들었다고 얘기하는 사람에게 온 마음을 다해 공감해줄 수 있다.   

또한, 괜히 잘 해보겠다고 너무 많은 자료를 이것저것 적용하다 보니 에러가 발생했다. 앞으로 참고 사이트는 되도록 1-2개로 간소하게 봐야 겠다.   

### 할 일

- [ ] 블로그 세부 내용 조율   
- [x] Atom과 연동해 Markdown preview로 작성 (git-plus)
