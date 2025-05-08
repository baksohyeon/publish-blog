---
category: Project/DADA
share: true
---
  
---  
# docker 사용해서 CI CD 구축하기  
  
### 도커 허브 사용  
  
[[Spring] 깃허브 액션(github action)과 도커(docker)를 사용하여 스프링 부트 서버 CI/CD 구축하기 (tistory.com)](https://growth-coder.tistory.com/224)  
  
  
---  
  
# 에러 로그 확인하기  
  
```  
/etc/nginx  
sudo systemctl reload nginx  
```  
  
```  
tail -f /var/log/nginx/beta.muk-da.com/error.log  
tail -f /var/log/nginx/error.log  
tail -f /var/log/nginx/access.log  
```  
  
---  
## 에러 해결 관련 북마크  
[Nginx 403 (13 Permission denied) 해결하기 (tistory.com)](https://rnokhs.tistory.com/entry/Nginx-403-13-Permission-denied-%ED%95%B4%EA%B2%B0%ED%95%98%EA%B8%B0)  
  
  
---  
  
  
## 플러터 Nginx 설정 추가하기  
ec2 접속하기  
  
```  
mssh -r ap-northeast-2 -d -u "dada-dorito" ec2-user@i-0b5cb525a46fb21c0  
```  
  
nginx 설치 (리눅스 기준)  
  
```  
sudo yum install nginx  
```  
  
```  
aws s3 cp s3://beta.muk-da.com/web dada-mukda-front --recursive  
```  
  
# Nginx 설정하기  
  
[NGINX 다중 서버 upstream 설정 - 현구막 기술 블로그 (hyeon9mak.github.io)](https://hyeon9mak.github.io/nginx-upstream-multi-server/)  
  
- /etc/nginx/proxy.conf  
      
  
```  
proxy_redirect                  off;  
proxy_http_version              1.1;  
proxy_pass_request_headers      on;  
proxy_set_header                Upgrade                 $http_upgrade;  
proxy_set_header                Connection              "upgrade";  
proxy_set_header                Host                    $http_host;  
proxy_set_header                                X-Real-IP                               $remote_addr;  
proxy_set_header                                X-Forwarded-For                 $proxy_add_x_forwarded_for;  
proxy_set_header                                X-Forwarded-Proto               $scheme;  
# socket.io ping timeout 보다 커야 함. 25초 기본  
# self.sio = socketio(server, {  
#       pingInterval : 10000,  
#       pingTimeout  : 60000# });  
proxy_connect_timeout           30s;proxy_send_timeout              60s;  
proxy_read_timeout              60s;proxy_buffers                   32 4000K;  
```  
  
- /etc/nginx/**nginx.conf**  
      
  
[DEV-79](https://linear.app/dada-company/issue/DEV-79/플러터-nginx-설정-추가하기#comment-7daf1e82) 기본 세팅 백업  
  
```  
# For more information on configuration, see:  
#   * Official English Documentation: http://nginx.org/en/docs/  
worker_processes auto;error_log  error.log notice;pid /run/nginx.pid;  
# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.include /usr/share/nginx/modules/*.conf;  
  
include /etc/nginx/modules-enabled/*.conf;  
  
events {  
    worker_connections 1024;  
}  
  
http {  
        upstream mydomain {  
          least_conn;  
          server localhost:5601;  
        }  
  
  include /etc/nginx/conf.d/*.conf;  
  include /etc/nginx/sites-enabled/*;  
}  
```  
  
`mukda-beta-server`  
  
```  
    upstream mukda_beta_server {  
        least_conn;  
        server localhost:8080;  
    }  
```  
  
- sites-enabled, sites-available 폴더 생성  
      
  
```  
$ mkdir /etc/nginx/sites-available  
$ mkdir /etc/nginx/sites-enabled  
```  
  
- /etc/nginx/**sites-enabled**  
      
  
```  
$ sudo vim /etc/nginx/sites-enabled/sub.mydomain.co.kr  
# 아래 내용 저장  
server {  
        server_name sub.mydomain.co.kr;  
        access_log /var/log/nginx/sub.mydomain.co.kr/access.log;  
        error_log /var/log/nginx/sub.mydomain.co.kr/error.log;  
        include proxy.conf;  
        location / {  
                proxy_pass http://mukda-server/;  
                # root 어쩌고 가능  
        }  
}  
```  
  
```  
$ sudo vim /etc/nginx/sites-enabled/sub.mydomain.co.kr  
server {  
        server_name api.beta.muk-da.com;  
        access_log /var/log/nginx/api.beta.muk-da.com/access.log;  
        error_log /var/log/nginx/api.beta.muk-da.com/error.log;  
        include proxy.conf;  
        location / {  
                proxy_pass http://mukda-beta-server/;  
        }  
}  
```  
  
- etc/nginx/**sites-available**  
      
  
```  
$ sudo vim /etc/nginx/sites-available/sub.mydomain.co.kr  
server {  
        server_name sub.mydomain.co.kr;  
        access_log /var/log/nginx/sub.mydomain.co.kr/access.log;  
        error_log /var/log/nginx/sub.mydomain.co.kr/error.log;  
        include proxy.conf;  
        location / {  
                proxy_pass http://mydomain/;  
        }  
}  
```  
  
```  
$ sudo vim /etc/nginx/sites-available/api.beta.muk-da.com  
server {  
        server_name api.beta.muk-da.com;  
        access_log /var/log/nginx/api.beta.muk-da.com/access.log;  
        error_log /var/log/nginx/api.beta.muk-da.com/error.log;  
        include proxy.conf;  
        location / {  
                proxy_pass http://mukda-beta-server/;  
        }  
}  
```  
  
- /etc/nginx/**proxy.conf**  
      
  
```  
$ vim /etc/nginx/proxy.conf  
# 아래 내용 저장  
proxy_redirect                  off;  
proxy_http_version              1.1;  
proxy_pass_request_headers      on;  
proxy_set_header                Upgrade                 $http_upgrade;  
proxy_set_header                Connection              "upgrade";  
proxy_set_header                Host                    $http_host;  
proxy_set_header                                X-Real-IP                       $remote_addr;  
proxy_set_header                                X-Forwarded-For                 $proxy_add_x_forwarded_for;  
proxy_set_header                                X-Forwarded-Proto               $scheme;  
# socket.io ping timeout 보다 커야 함. 25초 기본  
# self.sio = socketio(server, {  
#       pingInterval : 10000,  
#       pingTimeout  : 60000# });  
proxy_connect_timeout           30s;proxy_send_timeout              60s;  
proxy_read_timeout              60s;proxy_buffers                   32 4000K;  
```  
  
- 로그 파일 저장할 디렉터리 생성  
      
  
```  
mkdir /var/log/nginx/sub.mydomain.co.kr  
```  
  
- 구동 테스트  
      
  
```  
$ nginx -t  
```  
  
- 설정 적용  
      
  
```  
$ systemctl reload nginx.service  
```  
  
```  
flutter build web --release --base-href="/"  
```  
  
  
  
```  
댓글  
```  
  
```  
include /etc/nginx/modules-enabled/*.conf;  
  
events {  
    worker_connections 1024;  
}  
  
http {  
  server {  
    listen 80;  
    server_name localhost;  
  
    location / {  
        root /usr/share/nginx/html;  
        index index.html index.htm;  
        try_files $uri $uri/ /index.html;  
    }  
  }  
}  
```  
  
[[Flutter web] 배포하기 (nginx 설정+EC2 인스턴스 생성) (velog.io)](https://velog.io/@513sojin/Flutter-web-%EB%B0%91%EB%B0%94%EB%8B%A5%EB%B6%80%ED%84%B0-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0-nginx-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0) 참고  
[Easily Hosting Your Flutter Web App on a VPS Using Nginx | by Yawar Osman | Medium](https://yawarosman.medium.com/easily-hosting-your-flutter-web-app-on-a-vps-using-nginx-46576d03b30a)