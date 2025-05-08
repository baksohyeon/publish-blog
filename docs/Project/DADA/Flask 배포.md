---
category: Project/DADA
share: true
---
  
  
[How To Serve Flask Applications with Gunicorn and Nginx on Ubuntu 22.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-gunicorn-and-nginx-on-ubuntu-22-04)  
  
---  
  
[https://medium.com/@yogesh.choudhry.9/deploying-a-flask-application-on-aws-ec2-instance-free-tier-for-pose-estimation-494331b6827](https://medium.com/@yogesh.choudhry.9/deploying-a-flask-application-on-aws-ec2-instance-free-tier-for-pose-estimation-494331b6827)  
  
```  
sudo su  
sudo yum update  
sudo yum install python3-libselinux  
  
pip install -U pip   
```  
  
/usr/lib/python3.9/site-packages  
  
```  
[ec2-user@ip-172-31-10-10 dada-crawler-server]$ python3 api.module.py   
 * Serving Flask app 'api.module'  
 * Debug mode: off  
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.  
 * Running on all addresses (0.0.0.0)  
 * Running on http://127.0.0.1:5000  
 * Running on http://172.31.10.10:5000  
```  
  
Gunicorn은 파이썬 애플리케이션을 위한 인기 있는 WSGI HTTP 서버입니다. Flask 애플리케이션과 인터넷 사이의 다리 역할을 합니다.  
  
플라스크 애플리케이션을 서비스하기 위해 Gunicorn WSGI 서버 실행하기 플라스크를 "실행"하면 실제로는 웹 서버의 요청을 전달하는 Werkzeug의 개발 WSGI 서버를 실행하는 것입니다. Werkzeug는 개발 전용이므로 애플리케이션을 서비스하려면 프로덕션용 WSGI 서버인 Gunicorn을 사용해야 합니다.  
  
아래 명령어를 사용하여 Gunicorn을 설치합니다:  
  
![Pasted image 20240325173633.png](../../attachments/Pasted%20image%2020240325173633.png)  
  
  
```  
gunicorn -b 0.0.0.0:5001 naver-api:app  
gunicorn  naver_api:app -b 0.0.0.0:8000 --preload  
  
web:app : web.py 의 플라스크 객체 이름 app 입니다.  
  
-b 0.0.0.0:8000 : 모든 아이피에 대해 8000 포트로 브로드캐스트하겠다는 얘기입니다.  
  
-w 2 : 하위 worker 를 2개 더 만들어 멀티 유저에 대응하겠다. (보통 cpu * 2 개로 지정 한다고 합니다.)  
  
--timeout=10 : timeout 시간을 지정합니다.  
  
-k gevent : 이부분을 추가하지 않으면 1클라이언트 처리후 하위 worker 가 재시작하는 문제가 있어서 추가하였습니다.  
```  
  
[gunicorn을 이용한 flask 서버 서비스 : 네이버 블로그 (naver.com)](https://m.blog.naver.com/pareko/221918441176)  
  
### systemctl service 등록  
  
[Deploying a Flask Application on EC2 (plainenglish.io)](https://plainenglish.io/community/deploying-a-flask-application-on-ec2-dab5d3#step-6-set-up-gunicorn)  
  
```  
sudo vim /etc/systemd/system/naver-api.service  
```  
  
```  
[Unit]  
Description=Gunicorn instance to serve application  
After=network.target  
  
[Service]  
User=ec2-user  
Group=www-data  
WorkingDirectory=/path/to/your/app  
Environment="PATH=/path/to/venv/bin"  
ExecStart=/path/to/venv/bin/gunicorn --workers 3 --bind 0.0.0.0:5003 web_dynamic.2-hbnb:app  
ExecReload=/bin/kill -s HUP $MAINPID  
KillMode=mixed  
TimeoutStopSec=5  
PrivateTmp=true  
  
[Install]  
WantedBy=multi-user.target  
```  
  
```  
[Unit]  
Description=Gunicorn instance for a naver place map api  
After=network.target  
[Service]  
User=ec2-user  
Group=www-data  
WorkingDirectory=/home/ec2-user/dada-crawler-server  
ExecStart= -b 0.0.0.0:8000 naver_api:app --preload  
Restart=always  
[Install]  
WantedBy=multi-user.target  
```  
  
```  
gunicorn  naver_api:app -b 0.0.0.0:8000 --preload  
```  
  
```  
sudo systemctl daemon-reload  
sudo systemctl start naver-api  
sudo systemctl enable naver-api  
sudo systemctl status naver-api  
  
systemctl status naver-api.service  
journalctl -xeu naver-api.service  
systemctl daemon-reload  
sudo systemctl restart naver-api  
```  
  
```  
(python3-virtualenv) [ec2-user@ip-172-31-10-10 dada-crawler-server]$   
gunicorn --preload naver_api:app   
gunicorn --preload naver_api:app -b 0.0.0.0:5001  
[2024-03-11 09:00:10 +0000] [540584] [INFO] Starting gunicorn 21.2.0  
[2024-03-11 09:00:10 +0000] [540584] [INFO] Listening at: http://127.0.0.1:8000 (540584)  
[2024-03-11 09:00:10 +0000] [540584] [INFO] Using worker: sync  
[2024-03-11 09:00:10 +0000] [540585] [INFO] Booting worker with pid: 540585  
```  
  
[python gunicorn 가동 실패 이유 찾기 (tistory.com)](https://18vs1004.tistory.com/11)  
  
# venv python  
  
/home/ec2-user/code-repo/dada-crawler-server/python3-virtualenv/bin/python  
  
[How to Set Up A Python Virtual Environment On CentOS - Liquid Web](https://www.liquidweb.com/kb/how-to-setup-a-python-virtual-environment-on-centos/)  
  
```  
python -m venv python3-venv  
source python3-venv/bin/activate  
pip install -U pip requests  
deactivate  
  
/home/ec2-user/code-repo/dada-crawler-server/python3-venv/bin/python3 -m pip install --upgrade pip  
```  
  
```  
[ec2-user@ip-172-31-10-10 ~]$ pip3 freeze  
appdirs==1.4.4  
attrs==20.3.0  
aws-cfn-bootstrap==2.0  
awscli==2.14.5  
awscrt==0.19.19  
Babel==2.9.1  
beautifulsoup4==4.12.3  
blinker==1.7.0  
bs4==0.0.2  
certifi==2024.2.2  
cffi==1.14.5  
chardet==4.0.0  
chevron==0.13.1  
click==8.1.7  
cloud-init==22.2.2  
colorama==0.4.4  
configobj==5.0.6  
cryptography==36.0.1  
cssselect==1.2.0  
dbus-python==1.2.18  
distro==1.5.0  
docutils==0.16  
ec2-hibinit-agent==1.0.8  
fake-useragent==1.5.0  
Flask==3.0.2  
gevent==24.2.1  
gpg==1.15.1  
greenlet==3.0.3  
gunicorn==21.2.0  
idna==2.10  
importlib_metadata==7.0.2  
importlib_resources==6.1.3  
itsdangerous==2.1.2  
Jinja2==3.1.3  
jmespath==0.10.0  
jsonpatch==1.21  
jsonpointer==2.0  
jsonschema==3.2.0  
libcomps==0.1.18  
lockfile==0.12.2  
lxml==5.1.0  
MarkupSafe==2.1.5  
netifaces==0.10.6  
numpy==1.26.4  
oauthlib==3.0.2  
packaging==24.0  
pandas==2.2.1  
parse==1.20.1  
ply==3.11  
prettytable==0.7.2  
prompt-toolkit==3.0.24  
pycparser==2.20  
pyee==11.1.0  
pyppeteer==2.0.0  
pyquery==2.0.0  
pyrsistent==0.17.3  
pyserial==3.4  
PySocks==1.7.1  
python-daemon==2.3.0  
python-dateutil==2.9.0.post0  
pytz==2022.7.1  
PyYAML==5.4.1  
release-notification==1.2  
requests==2.25.1  
requests-html==0.10.0  
rpm==4.16.1.3  
ruamel.yaml==0.16.6  
ruamel.yaml.clib==0.1.2  
selinux==3.4  
sepolicy==3.4  
setools==4.4.1  
six==1.15.0  
soupsieve==2.5  
support-info==1.0  
tqdm==4.66.2  
typing_extensions==4.10.0  
tzdata==2024.1  
urllib3==1.25.10  
w3lib==2.1.2  
wcwidth==0.2.5  
websockets==10.4  
Werkzeug==3.0.1  
zipp==3.17.0  
zope.event==5.0  
zope.interface==6.2  
```  
  
# ~~yum 에서 dnf 패키지로 변경~~  
  
→ 절대 하면 안됨 ㄱ-  
  
[How to install Python 3.8 on Amazon Linux 2023 - Linux Shout (how2shout.com)](https://linux.how2shout.com/how-to-install-python-3-8-on-amazon-linux-2023/)  
  
```  
sudo dnf update -y  
```  
  
> sudo ln -s /usr/bin/python3.9 /usr/bin/python  
  
[python - How to install python3.6 on aws ec2 instance - Stack Overflow](https://stackoverflow.com/questions/59582741/how-to-install-python3-6-on-aws-ec2-instance)  
  
ou can just download and build from [source](https://www.python.org/downloads/source/).  
  
The steps are as follows:  
  
1. Install gcc and sqlite-devel `yum install gcc sqlite-devel`  
      
2. Extract the downloaded archive and cd to the directory  
      
3. Configure with `./configure --enable-optimizations`  
      
4. And finally `make altinstall`.  
      
  
`altinstall` avoid replacing the default `/usr/bin/python`.  
  
# Deploying Gunicorn  
  
[Deploying Gunicorn — Gunicorn 21.2.0 documentation](https://docs.gunicorn.org/en/stable/deploy.html)  
  
![Pasted image 20240325173657.png](../../attachments/Pasted%20image%2020240325173657.png)  
# AWS symbolic link restore  
  
[Amazon Linux AMI FAQ](https://aws.amazon.com/ko/amazon-linux-ami/faqs/)  
  
[18.04 - How do I get /usr/bin/python3 back after deleting it? - Ask Ubuntu](https://askubuntu.com/questions/1227930/how-do-i-get-usr-bin-python3-back-after-deleting-it)  
  
```  
[ec2-user@ip-172-31-10-10 code-repo]$ ls /usr/bin/python3*  
  
/usr/bin/python3  /usr/bin/python3.9  
  
sudo ln -sf    /usr/bin/python3.9 /usr/bin/python3  
sudo ln -s /usr/bin/python3 /usr/bin/python  
  
```  
  
[ubuntu - Python symbolic links mixed up - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/511510/python-symbolic-links-mixed-up)  
  
```  
sudo update-alternatives --remove-all python  
cd /usr/bin  
sudo ln -sf python2.7 python  
sudo ln -sf python3.5 python3  
```  
  
# 파이썬 버전 망함  
  
```  
[ec2-user@ip-172-31-10-10 code-repo]$ pip install -r temp.txt  
Python path configuration:  
  PYTHONHOME = '/usr/bin/python3'  
  PYTHONPATH = (not set)  
  program name = '/usr/bin/python3'  
  isolated = 0  
  environment = 1  
  user site = 1  
  import site = 1  
  sys._base_executable = '/usr/bin/python3'  
  sys.base_prefix = '/usr/bin/python3'  
  sys.base_exec_prefix = '/usr/bin/python3'  
  sys.executable = '/usr/bin/python3'  
  sys.prefix = '/usr/bin/python3'  
  sys.exec_prefix = '/usr/bin/python3'  
  sys.path = [  
    '/usr/bin/python3/lib/python38.zip',  
    '/usr/bin/python3/lib/python3.8',  
    '/usr/bin/python3/lib/python3.8/lib-dynload',  
  ]  
Fatal Python error: init_fs_encoding: failed to get the Python codec of the filesystem encoding  
Python runtime state: core initialized  
ModuleNotFoundError: No module named 'encodings'  
  
Current thread 0x00007fd5a3242740 (most recent call first):  
<no Python frame>  
```  
  
```  
[ec2-user@ip-172-31-10-10 bin]$ python --version  
Python 3.9.16  
[ec2-user@ip-172-31-10-10 bin]$ python3 --version  
Python 3.8.12  
```  
  
# 해결:? sudo update-alternatives 이거 심볼릭 링크 변경됨 ㅠ  
  
```  
[ec2-user@ip-172-31-10-10 code-repo]$ sudo update-alternatives --config python3  
  
There is 1 program that provides 'python3'.  
  
  Selection    Command  
-----------------------------------------------  
*+ 1           /usr/local/bin/python3.8  
  
Enter to keep the current selection[+], or type selection number: 1  
[ec2-user@ip-172-31-10-10 code-repo]$   
```  
  
# Python in AL2023  
  
[**PDF**](https://docs.aws.amazon.com/pdfs/linux/al2023/ug/al2023-ug.pdf#python)[**RSS**](https://docs.aws.amazon.com/linux/al2023/ug/linux-ug.rss)  
  
AL2023 completely removed Python 2.7 and any components requiring Python are now written to work with Python 3.  
  
AL2023 makes Python 3 available as `/usr/bin/python3` and in order to retain compatibility with customer code, as well as Python code shipped with AL2023, this will remain as Python 3.9 for the life of AL2023.  
  
The version of python that `/usr/bin/python3` points to is considered the "system Python", and for AL2023 this is Python 3.9.  
  
Newer versions of Python, such as Python 3.11, are made available as packages in AL2023, and are supported for the lifetime of the upstream versions. See [AL2023 Support Statements](https://docs.aws.amazon.com/linux/al2023/release-notes/support-info-by-support-statement.html) for details on how long each AL2023 package is supported for, including [Python 3.11](https://docs.aws.amazon.com/linux/al2023/release-notes/support-info-by-support-statement.html#support-info-by-support-statement-eol_python3.11).  
  
Multiple versions of Python can be installed simultaneously on AL2023. While `/usr/bin/python3` will always be Python 3.9, each version of python is namespaced and can be found by its version number. For example, if `python3.11` is installed, then `/usr/bin/python3.11` will exist alongside `/usr/bin/python3.9` and the `/usr/bin/python3` symlink to `/usr/bin/python3.9`.  
  
###### Note  
  
Do not change what the `/usr/bin/python3` symlink points to. Doing so may break core functionality of AL2023.  
  
# AWS 2023  
  
These answers are now out of date as of Amazon Linux 2023. [Amazon Linux 2023 FAQ](https://aws.amazon.com/linux/amazon-linux-2023/faqs/)  
  
**Q: Does AL2023 have Amazon-Linux-Extras like AL2?**  
  
A: No, AL2023 does not have extras. For higher-level software packages like language runtimes, we will use the quarterly release where we will add major/minor updates to packages as separate namespaced packages in addition to the default package provided in the repository. For example, default Python version in Amazon Linux 2023 may be 3.8, but we will add Python 3.9 (python39) as a separate namespaced package whenever it is made available. These additional packages will closely follow their upstream release cadence and support model and their support policies can be accessed by the package manager for compliance and security use cases. Default packages will continue to be supported throughout the life of AL2023.  
  
Python is installed by default as `python3` exact version is managed by Amazon. It is possible to pick a different version, but I have not found the instructions, since the currently install 3.9 works for my needs.  
  
python3 --version    
Python 3.9.16  
  
python3x --version    
always generated    
`-bash: python3x: command not found`    
regardless of choice of x  
  
---  
  
# 해결 1  
  
To create a symbolic link from `/usr/bin/python3` to `/usr/bin/python3.9`, you can use the `ln` command. Follow these steps:  
  
1. **Check Python 3 Version:**  
      
    Verify the installed version of Python 3. You can use the following command:  
      
    ```  
    python3 --version  
    ```  
      
    Ensure that Python 3.9 is installed.  
      
2. **Create the Symbolic Link:**  
      
    Use the `ln` command to create a symbolic link. Run the following command:  
      
    ```  
    sudo ln -sf /usr/bin/python3.9 /usr/bin/python3  
    ```  
      
    - `-s`: Create a symbolic link.  
          
    - `-f`: Force the creation of the link, overwriting the existing one if it exists.  
          
3. **Verify the Symbolic Link:**  
      
    Verify that the symbolic link has been created successfully:  
      
    ```  
    ls -l /usr/bin/python3  
    ```  
      
    The output should show the symbolic link pointing to Python 3.9.  
      
    ```  
    lrwxrwxrwx 1 root root 16 Mar 12 10:00 /usr/bin/python3 -> /usr/bin/python3.9  
    ```  
      
  
Now, `/usr/bin/python3` is a symbolic link pointing to Python 3.9. Keep in mind that changing the default Python version on a system may affect system tools and scripts that rely on the default Python version. Make sure that the applications and scripts on your system are compatible with Python 3.9 before making this change.  
  
---  
  
Using `sudo rm /usr/bin/python3` and `sudo ln -s /usr/bin/python3.9 /usr/bin/python3` seems to have done the trick. I realise that this may not be the best way to do this, but I don’t mind it for now, as I’m using fedora only for this one specific task.  
  
The pip packages are called pyvsc and pyboolector.  
  
My understanding is that `sudo dnf downgrade python --releasever=37` downgrades only python to that of fedora37’s version.    
It worked when I tried it, and for the record, I tried it before I tried `--releasever=36`.  
  
Thank you and [**@jakfrost**](https://discussion.fedoraproject.org/u/jakfrost) for your help and patience. I’ll mark one of your earlier answers suggesting symlinks as the solution.  
  
---  
  
# REAL 해결 python3 및 aws ec2  
  
```  
[root@ip-172-31-10-10 code-repo]# dnf reinstall python3  
Last metadata expiration check: 1 day, 1:19:48 ago on Mon Mar 11 08:36:56 2024.  
Dependencies resolved.  
=============================================================================================================================================  
 Package                      Architecture                Version                                     Repository                        Size  
=============================================================================================================================================  
Reinstalling:  
 python3                      x86_64                      3.9.16-1.amzn2023.0.6                       amazonlinux                       28 k  
  
Transaction Summary  
=============================================================================================================================================  
  
Total download size: 28 k  
Installed size: 33 k  
Is this ok [y/N]: u  
Is this ok [y/N]: y  
Downloading Packages:  
python3-3.9.16-1.amzn2023.0.6.x86_64.rpm                                                                     366 kB/s |  28 kB     00:00  
---------------------------------------------------------------------------------------------------------------------------------------------  
Total                                                                                                        195 kB/s |  28 kB     00:00  
Running transaction check  
Transaction check succeeded.  
Running transaction test  
Transaction test succeeded.  
Running transaction  
  Preparing        :                                                                                                                     1/1  
  Reinstalling     : python3-3.9.16-1.amzn2023.0.6.x86_64                                                                                1/2  
  Cleanup          : python3-3.9.16-1.amzn2023.0.6.x86_64                                                                                2/2  
  Running scriptlet: python3-3.9.16-1.amzn2023.0.6.x86_64                                                                                2/2  
  Verifying        : python3-3.9.16-1.amzn2023.0.6.x86_64                                                                                1/2  
  Verifying        : python3-3.9.16-1.amzn2023.0.6.x86_64                                                                                2/2  
=============================================================================================================================================  
WARNING:  
  A newer release of "Amazon Linux" is available.  
  
  Available Versions:  
  
  Version 2023.3.20240304:  
    Run the following command to upgrade to 2023.3.20240304:  
  
      dnf upgrade --releasever=2023.3.20240304  
  
    Release notes:  
     https://docs.aws.amazon.com/linux/al2023/release-notes/relnotes-2023.3.20240304.html  
  
=============================================================================================================================================  
  
Reinstalled:  
  python3-3.9.16-1.amzn2023.0.6.x86_64  
  
Complete!  
[root@ip-172-31-10-10 code-repo]# dnf upgrade --releasever=2023.3.20240304  
Amazon Linux 2023 repository                                                                                  47 MB/s |  25 MB     00:00  
Last metadata expiration check: 0:00:06 ago on Tue Mar 12 09:56:57 2024.  
Dependencies resolved.  
=============================================================================================================================================  
 Package                                 Architecture          Version                                      Repository                  Size  
=============================================================================================================================================  
Installing:  
 kernel                                  x86_64                6.1.79-99.164.amzn2023                       amazonlinux                 29 M  
Upgrading:  
 amazon-linux-repo-s3                    noarch                2023.3.20240304-0.amzn2023                   amazonlinux                 16 k  
 bind-libs                               x86_64                32:9.16.48-1.amzn2023.0.1                    amazonlinux                1.3 M  
 bind-license                            noarch                32:9.16.48-1.amzn2023.0.1                    amazonlinux                 14 k  
 bind-utils                              x86_64                32:9.16.48-1.amzn2023.0.1                    amazonlinux                207 k  
 cpio                                    x86_64                2.13-13.amzn2023.0.3                         amazonlinux                272 k  
 curl-minimal                            x86_64                8.5.0-1.amzn2023.0.2                         amazonlinux                160 k  
 docker                                  x86_64                25.0.3-1.amzn2023.0.1                        amazonlinux                 44 M  
 gnutls                                  x86_64                3.8.0-379.amzn2023.0.5                       amazonlinux                1.1 M  
 grub2-common                            noarch                1:2.06-61.amzn2023.0.11                      amazonlinux                1.8 M  
 grub2-efi-x64-ec2                       x86_64                1:2.06-61.amzn2023.0.11                      amazonlinux                269 k  
 grub2-pc-modules                        noarch                1:2.06-61.amzn2023.0.11                      amazonlinux                909 k  
 grub2-tools                             x86_64                1:2.06-61.amzn2023.0.11                      amazonlinux                1.8 M  
 grub2-tools-minimal                     x86_64                1:2.06-61.amzn2023.0.11                      amazonlinux                603 k  
 kernel-devel                            x86_64                6.1.79-99.164.amzn2023                       amazonlinux                 16 M  
 kernel-headers                          x86_64                6.1.79-99.164.amzn2023                       amazonlinux                1.4 M  
 kernel-livepatch-repo-s3                noarch                2023.3.20240304-0.amzn2023                   amazonlinux                 16 k  
 kernel-tools                            x86_64                6.1.79-99.164.amzn2023                       amazonlinux                155 k  
 libcurl-minimal                         x86_64                8.5.0-1.amzn2023.0.2                         amazonlinux                276 k  
 libuv                                   x86_64                1:1.47.0-1.amzn2023.0.2                      amazonlinux                250 k  
 ncurses                                 x86_64                6.2-4.20200222.amzn2023.0.6                  amazonlinux                394 k  
 ncurses-base                            noarch                6.2-4.20200222.amzn2023.0.6                  amazonlinux                 60 k  
 ncurses-libs                            x86_64                6.2-4.20200222.amzn2023.0.6                  amazonlinux                328 k  
 publicsuffix-list-dafsa                 noarch                20240212-61.amzn2023                         amazonlinux                 59 k  
 system-release                          noarch                2023.3.20240304-0.amzn2023                   amazonlinux                 27 k  
  
Transaction Summary  
=============================================================================================================================================  
Install   1 Package  
Upgrade  24 Packages  
  
Total download size: 99 M  
Is this ok [y/N]:  
```  
  
  
## 댓글  
  
# Nginx.conf 파일 백업  
  
```  
[ec2-user@ip-172-31-10-10 nginx]$ cat nginx.conf  
# For more information on configuration, see:  
#   * Official English Documentation: http://nginx.org/en/docs/  
#   * Official Russian Documentation: http://nginx.org/ru/docs/  
  
#user nginx;  
user root;  
worker_processes auto;  
error_log /var/log/nginx/error.log notice;  
pid /run/nginx.pid;  
  
# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.  
include /usr/share/nginx/modules/*.conf;  
  
events {  
    worker_connections 1024;  
}  
  
http {  
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '  
                      '$status $body_bytes_sent "$http_referer" '  
                      '"$http_user_agent" "$http_x_forwarded_for"';  
  
  
#    sendfile            on;  
#    tcp_nopush          on;  
#    keepalive_timeout   65;  
#    types_hash_max_size 4096;  
  
    include             /etc/nginx/mime.types;  
    default_type        application/octet-stream;  
  
    # Load modular configuration files from the /etc/nginx/conf.d directory.  
    # See http://nginx.org/en/docs/ngx_core_module.html#include  
    # for more information.  
  
    server {  
        server_name api.beta.muk-da.com;  
        access_log /var/log/nginx/api.beta.muk-da.com/access.log;  
        error_log /var/log/nginx/api.beta.muk-da.com/error.log;  
        include proxy.conf;  
        location / {  
                proxy_pass http://localhost:8080;  
       	  add_header 'Access-Control-Allow-Origin' '*';  
    add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';  
    add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range';  
    add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';  
	       	}  
	}  
  
  
	server {  
     	  listen 80 default_server;  
     	  listen [::]:80 default_server;  
     	  root /home/ec2-user/code-repo/dada-mukda-front;  
     	  index index.html index.htm;  
  
     	  server_name beta.muk-da.com www.beta.muk-da.com;  
  
     	  location / {  
#              try_files $uri $uri/ =404;  
			try_files $uri $uri/ /index.html;  
	  add_header 'Access-Control-Allow-Origin' '*';  
    add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';  
    add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range';  
    add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';  
			}  
  
	    include proxy.conf;  
   	    include /etc/nginx/conf.d/*.conf;  
    	    }  
 }  
[ec2-user@ip-172-31-10-10 nginx]$  
```  
  
# 추가한 nginx.conf  
  
```  
    upstream naver_place_api {  
    # fail_timeout=0 means we always retry an upstream even if it failed  
    # to return a good HTTP response  
  
    # for UNIX domain socket setups  
    # server unix:/tmp/gunicorn.sock fail_timeout=0;  
  
    # for a TCP configuration  
    server localhost:8000;  
    }  
```  
  
# Flask 정적 파일 배포하려면 wsgi 라는 키워드가 자주 나옴  
  
[How To Deploy A Flask App On AWS EC2 | by jQN | Medium](https://jqn.medium.com/deploy-a-flask-app-on-aws-ec2-1850ae4b0d41)  
  
```  
enable mod_wsgi 어쩌고 함  
```  
  
[Deploy Flask server to an EC2 instance | by Neel Ratan | Dev Genius](https://blog.devgenius.io/deploy-flask-server-to-an-ec2-instance-13e1a26bc7c9)  
  
문서에서는 flask + 정적 html 을 아파치 사용해서 웹서버 배포하는 방법 알려줌  
  
**app.py**  
  
```  
from flask import Flask  
app = Flask(__name__)@app.route('/')  
def hello_world():  
    return 'Hello, World!'if __name__ == "__main__":  
    app.run()  
```  
  
**app.wsgi**  
  
```  
activate_this = '/home/ubuntu/flaskapp/venv/bin/activate_this.py'with open(activate_this) as f:  
    exec(f.read(), dict(__file__=activate_this))import sys  
import logging  
logging.basicConfig(stream=sys.stderr) sys.path.insert(0,"/var/www/html/flaskapp/")from app import app as application  
```  
  
### Create wsgi file  
[Setting up Flask and Apache on AWS EC2 Instance (vishnut.me)](https://vishnut.me/blog/ec2-flask-apache-setup.html)    
Create a wsgi file.  
  
```  
$ vi app.wsgi  
```  
  
Paste the following code in.  
  
```  
activate_this = '/home/ubuntu/flaskproject/venv/bin/activate_this.py'  
with open(activate_this) as f:  
	exec(f.read(), dict(__file__=activate_this))  
  
import sys  
import logging  
  
logging.basicConfig(stream=sys.stderr)  
sys.path.insert(0,"/var/www/html/flaskproject/")  
  
from app import app as application  
```  
  
### **Open the page in a browser**  
  
Get the Public DNS from EC2 or just `exit` from EC2 and run `echo $EC2_DNS` to get the DNS. Paste it into a browser and it should display "Hello, World!". From this point on, refer to [Flask documentation](http://flask.pocoo.org/) to build your application.  
  
### **Debugging**  
  
**Internal Server Error**  
  
If anything goes wrong, first check the error logs using:  
  
```  
$ vi /var/log/apache2/error.log  
```  
  
**My changes are not showing up. The page still displays "Hello, World"**  
  
Try restarting your apache server  
  
```  
$ sudo apachectl restart  
```  
  
**I can't get out of vim!!**  
  
Sorry, can't help you there.    
[Trapped](http://www.commitstrip.com/en/2017/05/29/trapped/)    
[Stack Overflow: Helping One Million Developers Exit Vim](https://stackoverflow.blog/2017/05/23/stack-overflow-helping-one-million-developers-exit-vim/)  
  
**The error log is way too long**  
  
To clear the log, run  
  
```  
sudo bash -c 'echo > /var/log/apache2/error.log'  
```  
  
  
---  
  
```yaml  
[root@ip-172-31-10-10 sites-available]# nginx -t  
nginx: [warn] could not build optimal types_hash, you should increase either types_hash_max_size: 1024 or types_hash_bucket_size: 64; ignoring types_hash_bucket_size  
```  
  
# Python 문제 원인  
  
```  
[ec2-user@ip-172-31-10-10 ~]$ ls -la /usr/bin | grep 'python'  
lrwxrwxrwx.  1 root root          18 Mar 11 12:24 python -> /usr/bin/python3.9  
lrwxrwxrwx.  1 root root          25 Mar 12 09:12 python3 -> /etc/alternatives/python3  
-rwxr-xr-x.  1 root root       16168 Sep  8  2023 python3.9  
```  
  
[Comparing AL2 and Amazon Linux 2023 - Amazon Linux 2023](https://docs.aws.amazon.com/linux/al2023/ug/compare-with-al2.html)  
  
  
# 아예 새로 서버 만듬ㅎ  
  
기존 세팅 기록  
  
### python  
  
```  
[ec2-user@ip-172-31-11-106 dada-crawler-server]$ which python  
/usr/bin/which: no python in (/home/ec2-user/.local/bin:/home/ec2-user/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin)  
[ec2-user@ip-172-31-11-106 dada-crawler-server]$ whereis python  
python:  
```  
  
  
  
### python3  
  
```  
[ec2-user@ip-172-31-11-106 dada-crawler-server]$ which python3  
/usr/bin/python3  
```  
  
```  
[ec2-user@ip-172-31-11-106 dada-crawler-server]$ whereis python3  
python3: /usr/bin/python3 /usr/share/man/man1/python3.1.gz  
```  
  
### symbolic link  
  
```  
[ec2-user@ip-172-31-11-106 dada-crawler-server]$ ls -la /usr/bin | grep 'python'  
lrwxrwxrwx.  1 root root           9 Sep  8  2023 python3 -> python3.9  
-rwxr-xr-x.  1 root root       16168 Sep  8  2023 python3.9  
```  
  
  
[gunicorn 설정의 A to Z – 화해 블로그 | 기술 블로그 (hwahae.co.kr)](https://blog.hwahae.co.kr/all/tech/5567)  
  
[Gunicorn Documentation (readthedocs.org)](https://readthedocs.org/projects/gunicorn-docs/downloads/pdf/20.0.4/)  
  
  
# 공식문서 systemd 튜토리얼 문제  
  
[systemd - gunicorn daemon (active: failed) / curl(56) Recv Failure: Connection reset by peer - Stack Overflow](https://stackoverflow.com/questions/60348919/gunicorn-daemon-active-failed-curl56-recv-failure-connection-reset-by-pe)  
  
I had the exact same propblem following this tutorial. OP's answer did not help in my case but I found a solution [here](https://www.digitalocean.com/community/questions/gunicorn-service-no-such-file-or-directory). Maybe it helps others stubmling over this.  
  
Many thanks to [RussellMolimock](https://www.digitalocean.com/community/users/russellmolimock) for the following comment, which I found there!  
  
> "Go back into your virtualenv with source [your_project_env]/bin/activate and enter which gunicorn That will return the path to your gunicorn exectuable.  
>   
> Paste that into the path section of the ‘ExecStart’ value inside the ’/etc/systemd/system/gunicorn.service’ file, and run the ‘sudo systemctl daemon-reload’ and 'sudo systemctl restart gunicorn’ commands to restart your daemon and try curling again with curl –unix-socket /run/gunicorn.sock localhost  
>   
> I hope this helps!"  
  
  
# 가상환경에서 실행하면 nohup 처럼 켜지고 nginx 로 끝낼 수 있나?  
  
gunicorn 키워드 더 찾아보기  
  
```  
curl --unix-socket /home/ec2-user/code-repo/dada-crawler-server/naver_place/naver_place.sock http://localhost/  
```  
  
    
소켓도 된다~!!  
  
UNIX 도메인 소켓을 써라고함 공식문서에서는; 왜지  
  
![Pasted image 20240325174345.png](../../attachments/Pasted%20image%2020240325174345.png)  
소켓을 쓰면 정적 웹배포랑 같이 nginx 배포 가능함  
nginx user = root 로 수정  
  
  
# Gunicorn CPU 고용량 문제  
  
[nginx - Gunicorn CPU usage increasing to a very high value - Stack Overflow](https://stackoverflow.com/questions/47070509/gunicorn-cpu-usage-increasing-to-a-very-high-value)