### PoolableConnectionFactory

서버와 db를 연결시키려 할 때 해당 문구를 만났다.    
정확한 문구로는,      
<b>Error message: Unable to obtain connection : Cannot create PoolableConnectionFactory<b/>
였다.   

PoolableConnectionFactory는 로컬과 원격 서버에서 db를 연결해주는 역할을 한다.    
안 되는 이유는 간단하다. 서버와 db가 연결이 안 되기 때문이다.   
원인으로 네트워크 연결이 잘 안 되어 있거나, `방화벽`이 방해하는 경우다.   
보통은 방화벽 설정을 바꾸면서 내부 구조가 바뀌어 갑자기 서버와 연결이 에러가 나는 경우가 왕왕 있단다.    
그래서 방화벽을 잠시 내리거나 설정을 다시 바꿔주면 에러가 해결된다고 한다. 
