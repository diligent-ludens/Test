---
title: "GitHub ID 변경"
excerpt: "바꾸는 건 선택이지만 수습은 너의 몫이다."
comments: true

categories:
  - GitHub
last_modified_at: 2020-06-30
---
### 발견
GitHub 블로그 생성 초기에 문제 아닌 문제를 발견했다.
그건 바로 나의 `GitHub ID` 였다.

기존에 있던 아이디는 diligent-ludens로, 성실하게 노는 사람이라는 좋은 의미로 지었다.    
하지만 블로그용 repository를 만들며 문제를 인지했다.

```
https://github.com/diligent-ludens/diligent-ludens.github.io
```

괴물 같이 긴 이름의 주소가 만들어지는 걸 보고 기함을 토했다.   
저 긴 이름을 계속해서 타이핑할 나도 진저리가 나는데 타인이야 오죽할까 싶다.

### 정보 수집
다행히 GitHub는 ID 변경 옵션을 제공한다.

근데 ID를 바꾸려 하자 경고문이 촤르륵 뜨며 주의사항을 알려준다.   
자세히 적힌 주의사항은 아래 링크에 있다.   
<https://help.github.com/en/github/setting-up-and-managing-your-github-user-account/changing-your-github-username>

읽어 보면 ID 변경 시 레퍼런스도 자동으로 새 ID로 repository에 적용된다 한다.   
이제 기존 ID가 쓰인 링크를 클릭하면 404 페이지가 뜨니 링크도 새 ID로 갱신해줘야 한다.

그리고 타인이 내 기존 ID를 사용하게 되면 redict entry를 override해 에러가 날 수 있다 한다.   
그래서 기존에 있던 로컬 repository URL을 업데이트하라 권고한다.   

설마 다른 사람이 쓸까 싶은 ID지만, 만약을 대비해 업데이트하기로 했다.

<https://help.github.com/en/github/using-git/changing-a-remotes-url>   
여기에 URL 업데이트로 사용하는 명령어 `git remote set-URL`와 설명이 들어 있다.

### 실행
git bash를 이용해 바꿔보자.

먼저, 현재 환경 상태를 체크한다.
```bash
$ git config --list
```

이러면 14번째 줄 즈음에 나의 `user.name`과 `user.email`이 뜬다. 여전히 예전 ID를 쓰고 있다.

혹은, 직접적으로 물어봐서 볼 수도 있다.
```bash
$ git config user.username
$ git config user.email
```

변경하려면 위의 명령어 뒤에 "새 ID"를 쓰면 된다.
나는 전체적으로 바꾼 ID로 사용할 거라 global까지 넣어줬다.

```bash
$ git config --global user.name  "heejahn"
$ git config --global user.email "jackieahn1@gmail.com"
```
다시 git config로 물어보면 사용자명과 이메일이 변경된 걸 확인할 수 있다.

이제 각 로컬 저장소의 URL을 바꿔 주자. mvc 패턴 프로젝트가 있는 로컬 저장소를 바꿔 보기로 했다.
로컬 저장소가 있는 디렉토리로 이동해서 원격 저장소의 remote를 새 ID로 해주고 확인해 본다.

```bash
$ git remote set-url origin https://github.com/heejahn/web_mvc-pattern.git
$ git remote -v
```

그리고 혹시나 GitHub Desktop을 쓰는 사람이라면, 이 과정 후에 push를 시도하면 안 된다는 에러 메시지가 뜰 수 있다.   
그럴 땐 당황하지 말고 에러 메시지의 option에서 계정 sign out을 하면 된다.   
다시 로그인하면 잘 작동한다.

### 앞으로
나중에 추가적 문제가 발생한다면 후에 서술할 예정이다.
