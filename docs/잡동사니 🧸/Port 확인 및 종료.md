---  
category: 잡동사니 🧸  
share: true  
date created: Monday, January 16th 2023, 3:20:49 pm  
date modified: Wednesday, January 18th 2023, 4:41:28 pm  
---  
특정 포트를 찾아 포트를 닫고 싶으면 다음과 같이 쳐서 PID를 알아낸다.  
  
```shell  
sudo lsof -i :3000  
```  
  
여기서 3000이 포트번호이다.  
  
위에서 나온 PID를 다음 명령어에 넣으면 포트가 닫힌다.  
  
```shell  
sudo kill -9 PID  
```  
![[Pasted image 20230118153011.png]]