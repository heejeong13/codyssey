# codyssey
AI/SW 개발 워크스테이션 구축

## 1.프로젝트 개요
터미널로 작업 디렉토리와 권한을 정리한 뒤, Docker를 설치 및 점검하고 컨테이너를 실행/관리 합니다.  
간단한 웹서버를 컨테이너화하고, 포트 매핑으로 접속을 확인하며, 바인드 마운트와 볼륨으로 변경 반영과 데이터 영속성을 직접 검증합니다.  
같은 서비스를 다른 환경에서도 재현되는 방식을 경험하는 것이 목표입니다.

## 2.실행 환경
- OS : macOS 15.7.4
- Shell : zsh 5.9
- Docker : 28.5.2
- Git : 2.53.0

## 3. 수행 체크리스트
- [x] 터미널 조작 로그
- [x] Docker 운영/검증 로그
- [x] Dockerfile 기반 웹 서버 컨테이너
- [x] 포트 매핑 접속 증거
- [x] 바인드 마운트 반영 + 볼륨 영속성 증거
- [x] Git 설정 및 GitHub/VSCode 연동 증거

## 4. 수행로그
### 터미널 조작 로그 기록
**현재 위치 확인**
```bash
$ pwd
/Users/kosigi198323/Desktop/codyssey
```
**목록 확인(숨김파일 포함)**
```bash
$ ls -a
.		..		.git		README.md
```
**생성**
```bash
#폴더 생성
$ mkdir newfolder
$ ls
codyssey	newfolder

#빈 파일 생성
$ touch newfile
$ ls -l
total 0
-rw-r--r--  1 kosigi198323  kosigi198323  0  4  8 19:52 newfile
```
**복사**
```bash
# 파일 복사
$ cp test.txt test2.txt
$ ls
test.txt	test2.txt

# 폴더 복사
$ cp -r newfolder newfolder2
% ls
codyssey	newfolder	newfolder2
```

**이동/이름 변경**
```bash
#파일 이름 변경
$ ls
file1.text
$ mv file1.text file2.text
$ ls
file2.text

#폴더 이름 변경
$ ls
file2.text	oldfolder
$ mv oldfolder newfolder
$ ls
file2.text	newfolder

#파일 이동&이름 변경
$ mv file2.text newfolder/file0.txt
$ cd newfolder
$ ls
file0.txt

#폴더 이동&이름 변경
$ ls
codyssey	newfolder	test
$ mv test newfolder/rename
$ cd newfolder 
$ ls
file0.txt	rename
```
**삭제**
```bash
#파일 삭제
$ ls
codyssey	ls-a.png	pwd.png
$ rm ls-a.png 
$ ls         
codyssey	pwd.png

#폴더 삭제
$ ls
codyssey	newfolder	pwd.png
$ rm -rf newfolder 
$ ls
codyssey	pwd.png
```
**파일 내용 확인**
```bash
$ cat test.txt 
hello
```

### 권한 실습 및 증거 기록
>**[파일종류][소유자 권한][그룹 권한][기타 사용자 권한]**  
>*첫 번째 문자: 파일 종류*  
>\- : 일반 파일 (file)  
>d : 디렉토리 (directory)  
>l : 심볼릭 링크 (symbolic link)  
>*두 번째 ~ 네 번째 문자: 소유자 권한*  
>*다섯 번째 ~ 일곱 번째 문자: 그룹 권한*  
>*여덟 번째 ~ 열 번째 문자: 기타 사용자 권한*
```bash
$ ls -l
total 0
-rw-r--r--  1 kosigi198323  kosigi198323   0  4  8 20:09 file0.txt
drwxr-xr-x  2 kosigi198323  kosigi198323  64  4  8 20:16 rename

$ chmod 754 file0.txt 
$ chmod 644 rename 
$ ls -l
total 0
-rwxr-xr--  1 kosigi198323  kosigi198323   0  4  8 20:09 file0.txt
drw-r--r--  2 kosigi198323  kosigi198323  64  4  8 20:16 rename
```

### Docker 설치 및 기본 점검  
*Docker 버전 확인 결과*
```bash
$ docker --version
Docker version 28.5.2, build ecc6942
```
*Docker 데몬 동작 여부 확인 결과*
> 데몬이 실행되지 않으면 Cannot connect to the Docker daemon라고 출력 됨
```bash 
#데몬 구동 실패 시
Client:
 Version:    28.5.2
 Context:    orbstack
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.29.1
    Path:     /Users/kosigi198323/.docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.40.3
    Path:     /Users/kosigi198323/.docker/cli-plugins/docker-compose

Server:
Cannot connect to the Docker daemon at unix:///Users/kosigi198323/.orbstack/run/docker.sock. Is the docker daemon running?
```
```bash
#데몬 정상 구동 시
$ docker info
Client:
 Version:    28.5.2
 Context:    orbstack
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.29.1
    Path:     /Users/kosigi198323/.docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.40.3
    Path:     /Users/kosigi198323/.docker/cli-plugins/docker-compose

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 0
 Server Version: 28.5.2
 Storage Driver: overlay2
  Backing Filesystem: btrfs
  Supports d_type: true
  Using metacopy: false
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 CDI spec directories:
  /etc/cdi
  /var/run/cdi
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 1c4457e00facac03ce1d75f7b6777a7a851e5c41
 runc version: d842d7719497cc3b774fd71620278ac9e17710e0
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.17.8-orbstack-00308-g8f9c941121b1
 Operating System: OrbStack
 OSType: linux
 Architecture: x86_64
 CPUs: 6
 Total Memory: 15.67GiB
 Name: orbstack
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  ::1/128
  127.0.0.0/8
 Live Restore Enabled: false
 Product License: Community Engine
 Default Address Pools:
   Base: 192.168.97.0/24, Size: 24
   Base: 192.168.107.0/24, Size: 24
   Base: 192.168.117.0/24, Size: 24
   Base: 192.168.147.0/24, Size: 24
   Base: 192.168.148.0/24, Size: 24
   Base: 192.168.155.0/24, Size: 24
   Base: 192.168.156.0/24, Size: 24
   Base: 192.168.158.0/24, Size: 24
   Base: 192.168.163.0/24, Size: 24
   Base: 192.168.164.0/24, Size: 24
   Base: 192.168.165.0/24, Size: 24
   Base: 192.168.166.0/24, Size: 24
   Base: 192.168.167.0/24, Size: 24
   Base: 192.168.171.0/24, Size: 24
   Base: 192.168.172.0/24, Size: 24
   Base: 192.168.181.0/24, Size: 24
   Base: 192.168.183.0/24, Size: 24
   Base: 192.168.186.0/24, Size: 24
   Base: 192.168.207.0/24, Size: 24
   Base: 192.168.214.0/24, Size: 24
   Base: 192.168.215.0/24, Size: 24
   Base: 192.168.216.0/24, Size: 24
   Base: 192.168.223.0/24, Size: 24
   Base: 192.168.227.0/24, Size: 24
   Base: 192.168.228.0/24, Size: 24
   Base: 192.168.229.0/24, Size: 24
   Base: 192.168.237.0/24, Size: 24
   Base: 192.168.239.0/24, Size: 24
   Base: 192.168.242.0/24, Size: 24
   Base: 192.168.247.0/24, Size: 24
   Base: fd07:b51a:cc66:d000::/56, Size: 64

WARNING: DOCKER_INSECURE_NO_IPTABLES_RAW is set
```


### Docker 기본 운영 명령 수행
**이미지: 다운로드/목록 확인 (예: docker images)**
```bash
$ docker images         
REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
hello-world   latest    e2ac70e7319a   2 weeks ago   10.1kB

$ docker image rm hello-world
Error response from daemon: conflict: unable to remove repository reference "hello-world" (must force) - container a2fcfac74427 is using its referenced image e2ac70e7319a

$ docker image rm -f hello-world
Untagged: hello-world:latest
Untagged: hello-world@sha256:452a468a4bf985040037cb6d5392410206e47db9bf5b7278d281f94d1c2d0931
Deleted: sha256:e2ac70e7319a02c5a477f5825259bd118b94e8b02c279c67afa63adab6d8685b

$ docker image ls
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE

$ docker pull hello-world
Using default tag: latest
^[[Alatest: Pulling from library/hello-world
4f55086f7dd0: Already exists 
Digest: sha256:452a468a4bf985040037cb6d5392410206e47db9bf5b7278d281f94d1c2d0931
Status: Downloaded newer image for hello-world:latest
docker.io/library/hello-world:latest

$ docker images          
REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
hello-world   latest    e2ac70e7319a   2 weeks ago   10.1kB
```

**컨테이너: 실행/중지/목록 확인 (예: docker ps, docker ps -a)**
```bash
#실행 중인 컨테이너 출력
$ docker ps             
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

#실행 중, 정지된 컨테이너를 모두 출력
$ docker ps -a          
CONTAINER ID   IMAGE         COMMAND    CREATED              STATUS                          PORTS     NAMES
65111c842a30   hello-world   "/hello"   33 seconds ago       Exited (0) 33 seconds ago                 sweet_nobel
a2fcfac74427   hello-world   "/hello"   About a minute ago   Exited (0) About a minute ago             vibrant_mcnulty

#필터링된 컨테이너를 출력
$ docker ps -af 'name=vibrant_mcnulty'
CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS     NAMES
a2fcfac74427   hello-world   "/hello"   21 minutes ago   Exited (0) 21 minutes ago             vibrant_mcnulty

#컨테이너 실행/중지
$ docker start my-ubuntu
my-ubuntu
$ docker ps             
CONTAINER ID   IMAGE     COMMAND            CREATED         STATUS         PORTS     NAMES
09ab547de0b0   ubuntu    "sleep infinity"   2 minutes ago   Up 2 seconds             my-ubuntu
$ docker stop my-ubuntu
my-ubuntu
$ docker ps            
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

**운영: 로그 확인 (예: docker logs), 리소스 확인 (예: docker stats)**
```bash
$ docker logs 65111c842a30

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

$ docker stats
CONTAINER ID   NAME        CPU %     MEM USAGE / LIMIT   MEM %     NET I/O         BLOCK I/O     PIDS 
09ab547de0b0   my-ubuntu   0.00%     956KiB / 15.67GiB   0.01%     1.13kB / 126B   2.51MB / 0B   1 
```

### 컨테이너 실행 실습
**hello-wolrd 실행 성공 기록**
```bash
$ docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```
**ubuntu 컨테이너를 실행 및 동작**
```bash
$ docker run -it --name my-ubuntu ubuntu
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
689b91d88a0f: Pull complete 
Digest: sha256:84e77dee7d1bc93fb029a45e3c6cb9d8aa4831ccfcc7103d36e876938d28895b
Status: Downloaded newer image for ubuntu:latest
$ ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
$ echo "hello world"
hello world
$ echo "hello" > hello.txt
$ cat hello.txt 
hello
$ exit
```


**컨테이너 종료/유지(attach/exec 등)의 차이를 스스로 관찰하고 간단히 정리한다**
> *attach*
> - 실행된 컨테이너의 메인 프로세스의 표준 입력, 표준 출력, 표준 에러에 터미널을 직접 연결  
> - 메인 프로세스를 종료 시키면 컨테이너도 종료됨  

> *exec*
> - 실행 중인 컨테이너 안에 새로운 프로세스를 추가로 실행
> - exec로 실행한 명령이 끝나도 메인 프로세스는 살아 있으므로 컨테이너는 유지됨

```bash
$ docker ps
CONTAINER ID   IMAGE     COMMAND            CREATED          STATUS          PORTS     NAMES
f3a3518a4e98   ubuntu    "/bin/bash"        13 minutes ago   Up 10 minutes             my-ubuntu2
09ab547de0b0   ubuntu    "sleep infinity"   20 minutes ago   Up 7 minutes              my-ubuntu
$ docker exec -it my-ubuntu bash
root@09ab547de0b0:/# ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0   2704   132 ?        Ss   14:43   0:00 sleep infinit
root           7  0.0  0.0   4596  4116 pts/0    Ss   14:52   0:00 bash
root          15  0.0  0.0   7896  4196 pts/0    R+   14:52   0:00 ps aux
root@09ab547de0b0:/# exit
exit
$ docker ps                     
CONTAINER ID   IMAGE     COMMAND            CREATED          STATUS          PORTS     NAMES
f3a3518a4e98   ubuntu    "/bin/bash"        21 minutes ago   Up 19 minutes             my-ubuntu2
09ab547de0b0   ubuntu    "sleep infinity"   29 minutes ago   Up 16 minutes             my-ubuntu
```

### 기존 Dockerfile 기반 커스텀 이미지 제작 
*디렉토리 구조*  
```
my-web  
├── app  
│   └── index.html  
└── Dockerfile  
```

*Dockerfile*
```bash
FROM nginx:alpine
# ./app폴더의 내용을 Nginx의 기본 웹 루트 디렉토리로 복사
COPY ./app /usr/share/nginx/html
```
*Index.html*
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Docker 웹 서버</title>
    <style>
        body { font-family: sans-serif; text-align: center; margin-top: 50px; }
        h1 { color: #0db7ed; }
    </style>
</head>
<body>
    <h1> Hello, Docker</h1>
    <p> Welcome to the Codyssey!!</p>
</body>
</html>
```


```bash
#Dockerfile을 이용해 my-server 이미지 생성
$ docker build -t my-server .
[+] Building 2.3s (7/7) FINISHED                                                  docker:orbstack
 => [internal] load build definition from Dockerfile                                         0.1s
 => => transferring dockerfile: 167B                                                         0.0s
 => [internal] load metadata for docker.io/library/nginx:alpine                              1.6s
 => [internal] load .dockerignore                                                            0.1s
 => => transferring context: 2B                                                              0.0s
 => [internal] load build context                                                            0.1s
 => => transferring context: 59B                                                             0.0s
 => [1/2] FROM docker.io/library/nginx:alpine@sha256:645eda1c2477aaa9b879f73909b9222c6f1979  0.0s
 => CACHED [2/2] COPY ./app /usr/share/nginx/html                                            0.0s
 => exporting to image                                                                       0.1s
 => => exporting layers                                                                      0.0s
 => => writing image sha256:2c15f38f234aff5cbe344e585f88deb0a9d44affe166c8e28bd6ed781362da0  0.0s
 => => naming to docker.io/library/my-server                                                 0.0s

#컨테이너를 백그라운드에서 실행
$ docker run -d -p 8080:80 --name my-app my-server    
2617e98a2b6c257928d052935eb052dd8eb87d47c4f8328aa24e8f8aca4101f4
```

### 포트 매핑 및 접속 증거
<img width="778" height="360" alt="Image" src="https://github.com/user-attachments/assets/cd89de5e-0741-4d00-97fa-6693553e294a" />  

### Docker 바인드 마운트 검증
*코드를 수정하고 저장하는 즉시 실행 중인 컨테이너에 반영, 이미지를 다시 빌드하고 컨테이너를 재시작할 필요가 없음*
```
$ docker run -d -p 8080:80 --name my-app -v $(pwd)/app:/usr/share/nginx/html my-server
d8802aa6a86d1d136e05fa2caa23f2bb1c8925d89b2ad26a0d466243c10b1147
$ cd app 
$ vi index.html 
$ curl http://localhost:8080
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Docker 웹 서버</title>
    <style>
        body { font-family: sans-serif; text-align: center; margin-top: 50px; }
        h1 { color: #0db7ed; }
    </style>
</head>
<body>
    <h1> Hello, Docker</h1>
    <p> Welcome to the Codyssey!!</p>
    <h1>Version 1.1</h1>
</body>
</html>
```

### Docker  볼륨 영속성 검증
*Docker 볼륨: 생성/연결/검증 명령 + 컨테이너 삭제 전/후 비교*
```bash
#볼륨 생성
$ docker volume create new-volume
#볼륨 연결
$ docker run -d -p 8080:80 -v new-volume:/usr/share/nginx/html --name my-app my-server
#볼륨 검증
#호스트의 index.html 파일을 컨테이너의 웹 루트 폴더로 복사
$ docker cp ./index.html my-app:/usr/share/nginx/html/
#index.html 직접 수정(컨테이너의 웹 루트 파일을 수정하는 또 다른 방법임)
$ docker exec -it my-app /bin/sh -c "echo '<h1>hello, Docker from exec</h1>' > /usr/share/nginx/html/index.html"
#컨테이너 삭제 전 index.html
$ curl http://localhost:8080
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Docker 웹 서버</title>
    <style>
        body { font-family: sans-serif; text-align: center; margin-top: 50px; }
        h1 { color: #0db7ed; }
    </style>
</head>
<body>
    <h1> Hello, Docker</h1>
    <p> Welcome to the Codyssey!!</p>
    <p> volume test</p>
</body>
</html>

#컨테이너 삭제
$ docker rm -f my-app
#컨테이너 재실행
$ docker run -d -p 8080:80 -v new-volume:/usr/share/nginx/html --name my-app my-server
#컨테이너 삭제 후 재실행 시 index.html
$ curl http://localhost:8080
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Docker 웹 서버</title>
    <style>
        body { font-family: sans-serif; text-align: center; margin-top: 50px; }
        h1 { color: #0db7ed; }
    </style>
</head>
<body>
    <h1> Hello, Docker</h1>
    <p> Welcome to the Codyssey!!</p>
    <p> volume test</p>
</body>
</html>
```

### Git 설정 및 GitHub 연동
```bash
#git 설정
$ git config --global user.name "heejeong"
$ git config --global user.email "kosigi19@gmail.com"
$ git config --global init.defaultBranch main
$ git config -l                           
user.name=heejeong
user.email=kosigi19@gmail.com
init.defaultbranch=main

#git 연동 확인
$ git add .
$ git commit -m "docs: 수정"
$ git push origin       
오브젝트 나열하는 중: 5, 완료.
오브젝트 개수 세는 중: 100% (5/5), 완료.
Delta compression using up to 6 threads
오브젝트 압축하는 중: 100% (2/2), 완료.
오브젝트 쓰는 중: 100% (3/3), 280 bytes | 280.00 KiB/s, 완료.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/heejeong13/codyssey.git
   bd5ddaf..7091160  main -> main
```
*GitHub - VSCode 연동 확인*  
<img width="447" height="505" alt="Image" src="https://github.com/user-attachments/assets/1a593e8b-98d7-43a2-92e1-567e4cc620fb" />

### 트러블 슈팅
**문제**) 웹 서버 구동 시 작성한 index.html 페이지가 보이지 않음
<img width="777" height="362" alt="Image" src="https://github.com/user-attachments/assets/80625bde-9297-4fec-8c68-64f932bb1370" />

> 작성한 index.html 파일의 내용을 정상적으로 불러오지 못함.  
> 원인 가설1) 문법 오류 -> html 오류 없음   
> 원인 가설2) 파일 위치 오류

**해결**  
> 파일이 app 폴더 하위에 없어서 발생한 오류로 파일 위치 변경 후 문제 해결  
<img width="778" height="360" alt="Image" src="https://github.com/user-attachments/assets/cd89de5e-0741-4d00-97fa-6693553e294a" />


**문제**) GitHub에서 git clone으로 가져온 프로젝트에서 작업했다.
git add와 git commit으로 내 컴퓨터(로컬 저장소)에는 변경사항을 성공적으로 저장했다.
하지만 git push 명령어로 GitHub(원격 저장소)에 올리려고 하면, 에러가 발생하며 실패한다.

> 가설 1) 해당 저장소에 쓰기 권한 없음  
> 가설 2) 로컬 저장소와 원격 저장소의 상태가 다름  
> 가설 3) 인증 정보 오류  

**해결**
> Make sure you configure your "user.name" and "user.email" in git.  
메시지를 확인하였고, 변경사항을 기록할때는 이 기록에 사용자 정보가 필요한데 설정을 하지 않아 발생하는 오류 메세지였다.
git config --global user.name
git config --global user.email
위 커맨드를 사용하여 개인 정보를 입력하였고 정상동작하였음.






