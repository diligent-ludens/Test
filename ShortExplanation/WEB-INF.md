### WEB-INF

Web Information의 약자로 web에 관련된 정보를 표시한다.    
웹 관련된 파일은 webapp 안에서 진행되는데, 브라우저는 Context Root 하위 정보는 접근할 수 있지만 WEB-INF에는 접근할 수 없다.    
브라우저 url로 jsp를 직접 요청하지 못하기에 jsp 파일은 WEB-INF 폴더에 있고, 브라우저가 반드시 참조해야 하는 css, 이미지, jquery는 Context Root 하위에 static 폴더를 만들어 관리한다.    
톰캣이 필수로 읽어들이는 항목이라 WEB-INF 폴더는 무조건 존재해야 한다.
