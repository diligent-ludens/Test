### Day1. Docker로 MariaDB 설치와 기본 페이지 설정

목표        
단순 게시판 만들기
<br>
구조
- 리스트
- 뷰(읽기)
- 쓰기 / 수정 / 삭제

Server side    
- Spring framework
- iBatis

client side     
- JSP
- jQuery

DB     
- MariaDB

<br>

Docker를 설치하기 전에 먼저 확인해야 할 게 있다.     

<br>

##### Hyper V     
Docker는 리눅스 기반 프로그램이라 윈도우에서 사용하려면 가상 머신을 통해 사용해야 한다.   
이때 사용하는 게 Hyper V다. 다양한 운영체제를 Windows에서 가상 머신을 이용해 실행할 수 있게 해준다.    
리눅스 기반으로 Host 컴퓨터와 Guest 컴퓨터(Hyper V)가 Host의 전체 메모리와 CPU 용량을 공유하며, Guest가 실행하지 않으면 Host 컴퓨터가 full capacity로 사용할 수 있다.      
다만, Windows 버전마다 제공 유무가 다르며, Docker 실행 전 가상화 옵션이 지원되는지 확인해야 한다. 

<br>

##### Docker    
Image로 저장한 데이터를 container로 옮겨서 보관 및 불러오기 기능     
휘발성 프로그램이라 서버를 올렸다가 내리면 데이터가 날라간다. 그래서 로컬 컴퓨터에 파일을 저장할 path를 지정하는 게 정말 중요하다.   
Docker Hub 내에 있는 저장소 중에 MariaDB를 찾아서 로컬 컴퓨터에 다운로드한다.   
그리고 다운 받는 저장소는 되도록 OFFICIAL이 붙은 저장소를 선택하는 게 좋다.     


Docker 명령어는 Windows Powershell에서 작성한다. gui를 사용해도 되지만 터미널 명령어를 쓸 일이 많기 때문에 익숙해지는 게 좋을 것 같다.    

```
docker ps // 현재 활성화 된 컨테이너 목록 확인
cls // 커맨드 창 정리
docker ps -all // 컨테이너 목록 전체(비활성화 포함) 확인

docker search mysql // mysql이라는 단어로 저장소 찾기
docker pull maraidb // mariadb 이미지 다운로드
docker images // 이미지 확인하기

docker run --name board-mariadb -e MYSQL_ROOT_PASSWORD=my123 -d mariadb:latest
docker run --name board-mariadb -p 3306:3306 -v C:\Developer\data\docker\board-mariadb:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=my123 -d mariadb:latest
// 이름, 패스워드, 태그를 지정해 새로운 컨테이너 생성
// 두번째 줄 추가 : 로컬 컴퓨터에 저장 파일 경로 지정을 포함한 명령어
// -p는 로컬 컴퓨터와 도커의 포트 번호 지정

netstat -an // 실행 중인 포트 전체 보여주기

docker exec -it board-mariadb bash // 해당 이름(board-mariadb)의 도커에 접속
// -it : STDIN 표준 입력을 열고 가상 tty를 통해 접속하겠다는 의미
mysql -u root -p // mysql에 root로 들어가기. 후에 패스워드 입력해야 함. my123
\s / status // 현재 상황 확인
// status에 나오는 리스트 중에서 Client와 Conn. characterset을 utf-8로 바꿔야 함
// MariaDB로 들어오면 ;로 명령어 뒤에 붙여줘야 함
show databases; // 현재 설치된 모든 데이터베이스 목록 확인

history // 이전까지 썼던 명령어 보여주기
ctrl + c // 나가는 명령어

docker start board-mariadb
docker exec -it board0mariadb /bin/bash
docker stop board-mariadb
```

<br>

Docker의 컨테이너를 생성하고 exec를 한 다음 status로 상태를 확인할 수 있는데, client측의 인코딩이 LATIN으로 되어 있다.    
혹시 모를 일을 대비해 UTF-8로 바꿔주는 게 좋다.   
UTF-8로 바꾸려면 vim을 다운로드 하고 설정 라인을 넣어줘야 한다.    

```
// vim 설치 명령어
apt-get update
apt-get install vim

// UTF-8 인코딩 라인
cat << 'EOF' > /etc/mysql/conf.d/utf8.cnf
# for utf8 characterset
[client] default-character-set = utf8

[mysqld]
init_connect = SET collation_connection = utf8_general_ci
init_connect = SET NAMES utf8
character-set-server = utf8
collation-server = utf8_general_ci

[mysqldump]
default-character-set = utf8 [mysql] default-character-set = utf8
EOF
```

참고로, 리눅스는 커널이라는 기본 코어가 있지만 굉장히 다양한 버전들이 존재한다.    
제일 유명한 계열로는 centOS로 유명한 Redhat 계열과 Ubuntu로 유명한 Debian 계열이 있다.    
centOS는 yum 명령어를 사용하고, Ubuntu는 apt-get 명령어를 사용한다.    

<br>

##### MariaDB
데이터베이스 생성과 사용자 추가를 해줘야 한다.
기본적으로 생성된 사용자는 3개인데, 비밀번호까지 적용된 localhost 1개 빼고 나머지 2개는 삭제해야 한다.   
비밀번호가 없는 경우 나중에 운영 차원에서 보안에 위협이 될 수 있다.   

사용자를 생성하고 그 사용자에게 db 권한을 줘야 한다.

```
create user 'board'@'%' identified by 'board123';

$ grant all privileges on board.* to 'board'@'%'; // % : 전체를 표현 <- 이거 쓴다.

$ grant all privileges on 특정DB.* to '사용자'@'localhost'; // 특정 db만 적용할 때

```

여기서 중요하다!!! grant 키워드를 사용하고 나면 꼭 `flush previleges;`를 해줘야 한다. 안 그럼 적용이 제대로 안 된다.     

DB 사용을 위해 DBeaver에 MariaDB를 연결 시키고 새로운 테이블 board와 comments를 만들었다.    
board는 게시판 메인 화면을 보여주고, comments는 각 게시판 아래에 쓰는 댓글 기능을 구현한다.   

```
CREATE table board(
	bid int not null auto_increment, // auto_increment 자동으로 숫자 증가
	title varchar(200) not null,
	contents mediumtext, // 큰 크기의 텍스트 제공
	reg_dt datetime, // 날짜 지정
	read_count int,
	primary key(bid) // 모든 테이블에는 primary key 존재. 여러 개 있을 수 있음
);

CREATE table comments(
	cid int not null auto_increment,
	bid int not null,
	contents varchar(200) not null,
	reg_dt datetime,
	primary key(cid, bid),
	foreign key(bid) References board(bid) // fk : board에 있는 bid를 참조
);

select now(); // 현재 시간을 나타내는 함수

insert into board(title, contents, reg_dt, read_count)
values('테스트용1', '테스트 내용1', now(), 0);


select * from board;


insert into comments(bid, contents, reg_dt)
values(1, '댓글1', now());

select * from comments;
```

<br>

##### IntelliJ로 스프링/메이븐 프로젝트 구성
설정     
스프링 프로젝트를 생성함에 있어 제일 중요한 파일은 web.xml과 pom.xml이다.    
web.xml은 요청이 들어왔을 때 가장 먼저 마주하게 되는 파일로 Dispatcher Servlet과 들어온 정보를 걸러내는 filter에 대한 설정을 한다.   
pom.xml은 프로젝트에서 쓰이는 모든 기술과 버전을 작성한다. 최신 버전이나 호환성을 체크하고 설정하는 게 중요하다.    

그 외 application-context.xml, board-servlet.xml, mybatis-config.xml, database.properties 설정을 한다.

<br>

클래스 및 구현 페이지들    
게시판을 만들기 위해서는 데이터가 어떤 방향으로 흘러가는지 알아야 한다.      
1. mapper xml 파일에 MariaDB를 통해 생성한 테이블을 호출하는 쿼리문을 작성한다.      
2. mapper.java 파일에 데이터를 불러오기 위한 인터페이스를 설정한다.     
3. Dto.java 파일에서 자바 객체로 가져오기 위해 필드 선언, 생성자 호출, getter/setter 메서드와 toString을 생성한다.    
4. service.java 파일로 dto에 정의된 데이터를 가져온다.
5. controller.java 파일에서 요청했던 데이터가 들어오면 모델 객체를 생성해 데이터를 담아 지정한 jsp 페이지로 보낸다.     
6. jsp 페이지에 화면과 필요한 정보를 보여준다.

<br>

##### 오늘 느낀 점
스프링과 프로젝트 구성에 대한 기본 지식이 없어 많이 헤맨 것 같다.     
공부하면서 원리를 파악하는데 집중해보자.
