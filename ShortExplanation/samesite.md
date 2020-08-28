### samesite

samesite는 크롬의 쿠키 정책에 쓰이는 속성이다. 다른 도메인 간에 이뤄지는 쿠키 전송에 대한 보안을 설정한다.    
2020년 2월 4일에 출시된 크롬 80버전부터 samesite의 기본 속성값이 `None`에서 `Lax`로 변경되었다.   
None으로 설정하면 동일 사이트와 크로스 사이트 모두에서 쿠키를 사용할 수 있지만 그만큼 사이트 간 요청 위조(cross-site request forgery, XSRF)에 노출될 수 있는 위험이 있다.   
기본 속성값을 Strict로 설정하면 크로스 사이트 요청은 불가능하다. 하지만 가끔 유저 본인이 strict값을 설정하고 본인의 컴퓨터로 사이트에 접속해도 에러가 나는 경우가 가끔 발생한다고 한다.   
<br>
아래 내용은 samesite 에러가 났을 때 개발자 도구에서 찍히는 문구이다.   
 A cookie associated with a cross-site resource at http://localhost/ was set without the SameSite attribute. 
 It has been blocked, as Chrome now only delivers cookies with cross-site requests if they are set with SameSite=None and Secure.
<br>
위의 내용은 크롬이 크로스 사이트 요청의 경우 SameSite-None과 Secure를 같이 적용해야 쿠키 전송을 허용한다는 내용이다.    
따라서, 자신의 header에 쿠키에 대한 옵션을 적으면서 samesite=None; Secure;를 추가하면 된다.   
