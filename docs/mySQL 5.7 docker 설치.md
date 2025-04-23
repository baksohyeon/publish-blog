---
title: mySQL 5.7 docker 설치
share: true
---
# mySQL 5.7 docker 설치  
> docker-compose.xml 을 작성하여 arm이 지원되지 않는 MySQL 버전을 설치하고 mySQL 에서 user 계정을 관리할 수 있다.  
  
## 설치 방법 정리  
- docker-compose.yaml  
```bash  
version: '3'  
  
services:  
  local-db:  
    image: library/mysql:5.7  
    container_name: mysql-container  
    restart: always  
    ports:  
      - 13306:3306  
    environment:  
      MYSQL_ROOT_PASSWORD: password  
      TZ: Asia/Seoul  
    volumes:  
      - db-data:/db/mysql/data:/var/lib/mysql  
      - db-data:/db/mysql/init:/docker-entrypoint-initdb.d  
    platform: linux/amd64  
  
volumes:  
  db-data:  
```  
  
- 컨테이너 생성 및 실행  
```bash  
docker-compose -f docker-compose.yml up -d   
```  
  
- 도커 컨테이너 접속  
```bash  
docker exec -it mysql-container /bin/bash  
```  
  
- mySql 종료  
```bash  
mysql> exit;  
```  
  
- 도커 bash 종료  
```shell  
$ exit 0  
```  
  
- 도커 내 DB 접속  
```shell  
$ mysql -u root -p  
$ Enter password:   
```  
  
yaml 에 적어둔 비밀번호 입력 (`password`)  
  
- DB 사용자 추가하기  
```shell  
// localhost 에서만 접속 가능한 계정 생성  
mysql> USE mysql;  
mysql> CREATE USER 'YOUR_SYSTEM_USER'@'localhost' IDENTIFIED BY 'YOUR_PASSWORD';  
mysql> GRANT ALL PRIVILEGES ON *.* TO 'YOUR_SYSTEM_USER'@'localhost';  
mysql> FLUSH PRIVILEGES;  
  
  
// 모든 DB, 테이블에 접속 가능한 계정 생성  
mysql> USE mysql;  
mysql> CREATE USER 'YOUR_SYSTEM_USER'@'%' IDENTIFIED BY 'YOUR_PASSWORD';  
mysql> GRANT ALL PRIVILEGES ON *.* TO 'YOUR_SYSTEM_USER'@'%';  
mysql> FLUSH PRIVILEGES;  
  
// 예시  
mysql> USE mysql;  
mysql> CREATE USER 'sa'@'%' IDENTIFIED BY 'password';  
mysql> GRANT ALL PRIVILEGES ON *.* TO 'sa'@'%';  
mysql> FLUSH PRIVILEGES;  
```  
  
---  
## 참고용: 상우님 가이드  
  
[MySQL :: Supported Platforms: MySQL Database](https://www.mysql.com/support/supportedplatforms/database.html) 를 보면 해당 버전은 (5.7) arm 지원 안하기 때문에 따로 도커로 셋팅해주어야함.  
  
```yaml  
version: '3'  
  
services:  
  local-db:  
    image: library/mysql:5.7  
    container_name: mysql-container  
    restart: always  
    ports:  
      - 13306:3306  
    environment:  
      MYSQL_ROOT_PASSWORD: password  
      TZ: Asia/Seoul  
    volumes:  
      - ./db/mysql/data:/var/lib/mysql  
      - ./db/mysql/init:/docker-entrypoint-initdb.d  
    platform: linux/amd64  
```  
  
이때 포트 번호 3306은 개인적으로 많이 쓰이는 포트라서, 임의적으로 13306으로 설정  
  
- 참고 사이트  
[[Database] docker-compose 로 MySQL 설치하기](https://wecandev.tistory.com/107)  
[[Mysql] Docker mysql 5.7 버전 설치 (MacbookPro M1)](https://jaynamm.tistory.com/entry/Mysql-Docker-mysql-57-%EB%B2%84%EC%A0%84-%EC%84%A4%EC%B9%98#1.%20%EB%8F%84%EC%BB%A4%EC%97%90%EC%84%9C%20mysql%20%EC%9D%B4%EB%AF%B8%EC%A7%80%20%EB%B2%84%EC%A0%84%20%ED%99%95%EC%9D%B8)  
  
  
---  
  
## 에러 및 해결 방법  
==에러==   
컨테이너 생성 및 실행 커맨드를 실행했을 때 다음과 같은 문제가 발생한다.  
> "Error response from daemon: invalid volume specification"  
  
- 에러 로그  
```shell  
[+] Running 0/0  
 ⠋ Container mysql-container  Creating  
  
# error message  
Error response from daemon: invalid volume specification: '/host_mnt/Users/dorito/dev/mysql:5.7-docker-compose/db/mysql/data:/var/lib/mysql:rw'  
```  
  
  
---  
==해결 방법==  
volumes 설정을 추가하고 절대 경로로 바꾸어 준다.  
```yaml  
version: '3'  
services:  
  local-db:  
    image: library/mysql:5.7  
    container_name: mysql-container  
    restart: always  
    ports:  
      - 13306:3306  
    environment:  
      MYSQL_ROOT_PASSWORD: password  
      TZ: Asia/Seoul  
    volumes:  
      - db-data:/db/mysql/data:/var/lib/mysql  
      - db-data:/db/mysql/init:/docker-entrypoint-initdb.d  
    platform: linux/amd64  
  
volumes:  
  db-data:  
```  
  
  
---  
### 발생 이유  
도커 디렉토리 마운트 설정 에러 문제라고 한다.  
  
컨테이너 디렉토리 경로를 절대경로로 입력하지 않았을 때 발생한다.  
  
도커에서 호스트OS 와 컨테이너 간 디렉토리 마운트 설정할때엔 반드시 **컨테이너 디렉토리의 경로를 절대경로로 입력**하면 된다.  [^1]  
  
  
### 문제 접근  
1. yaml 파일에 적힌 volumes가 상대경로라서 오류가 나는 것 같은데 yaml 파일에 명시된 상대경로 위치에 어떤 파일이 있는지 확인 (로컬)  
  
상우님께 여쭈어보니 터미널 위치가 도커인건지 확인하셨음.  
  
내 로컬 환경에서 mySQL 을 설치하는 과정에서 뭔가 오류가 있는건지 확인해보고 싶은거였는데, 반응을 보니 상우님은 상대 경로로 설정하셨는데도 해당 디렉터리를 따로 확인해보신 적 없는 것 같음.  
  
그리고 구체적으로 이 부분에 대해서 어떻게 접근해야할지 모르겠고 해당 이슈는 로컬 환경과는 연관이 없을 것 같아서 일단 넘어감.  
  
  
2. 파일에 명시된 해당 경로가 어떻게 쓰이는지 관련 키워드로 구글링  
  
> 키워드  
> - ./db/mysql/data:/var/lib/mysql absolute path  
> - **db/mysql/data:/var/lib/mysql**   
  
결정적으로 도움 된 곳:   
- [[docker] volume container 추가하기 | 기록은 기억의 연장선](https://joont92.github.io/docker/volume-container-%EC%B6%94%EA%B0%80%ED%95%98%EA%B8%B0/)  
- [docker - Where is a mysql volume db-data:/var/lib/mysql/data stored? - Stack Overflow](https://stackoverflow.com/questions/58090668/where-is-a-mysql-volume-db-data-var-lib-mysql-data-stored)  
  
 질문자가 적어둔 코드를 보고 volumes 관련 설정을 추가해주었더니 해결 되었다.  
  
![Pasted image 20230427143404.png](./attachments/Pasted%20image%2020230427143404.png)  
  
  
### 에러 해결 관련 필요 개념  
  
내 docker compose 파일 위치는 다음과 같다.  
```  
/Users/dorito/dev/mysql:5.7-docker-compose/docker-compose.yml  
```  
  
일단 추측이지만 도커 파일 위치가 달라서 아닐까  
  
  
-  volumes 관련 개념  
- docker 와 docker compose 차이  
  
북마크  
[Volumes | Docker Documentation](https://docs.docker.com/storage/volumes/)  
[docker compose에서 서로 다른 container가 같은 volume을 공유하는 방법 | by Su Bak | Medium](https://medium.com/@su_bak/docker-compose%EC%97%90%EC%84%9C-%EC%84%9C%EB%A1%9C-%EB%8B%A4%EB%A5%B8-container%EA%B0%80-%EA%B0%99%EC%9D%80-volume%EC%9D%84-%EA%B3%B5%EC%9C%A0%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-5e49430c5282)  
[[docker] volume container 추가하기 | 기록은 기억의 연장선](https://joont92.github.io/docker/volume-container-%EC%B6%94%EA%B0%80%ED%95%98%EA%B8%B0/)  
[Docker 컨테이너에 데이터 저장 (볼륨/바인드 마운트) | Engineering Blog by Dale Seo](https://www.daleseo.com/docker-volumes-bind-mounts/)  
  
==이후 공부하기==  
  
- [>] 도커 볼륨 개념 공부하기  
  
---  
[^1]: [[Docker CE] 도커 디렉토리 마운트 설정 에러 (Error response from daemon: invalid volume specification) :: Nirsa](https://nirsa.tistory.com/54)  
[^2]: [docker compose에서 서로 다른 container가 같은 volume을 공유하는 방법 | by Su Bak | Medium](https://medium.com/@su_bak/docker-compose%EC%97%90%EC%84%9C-%EC%84%9C%EB%A1%9C-%EB%8B%A4%EB%A5%B8-container%EA%B0%80-%EA%B0%99%EC%9D%80-volume%EC%9D%84-%EA%B3%B5%EC%9C%A0%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-5e49430c5282)