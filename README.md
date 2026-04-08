# codyssey
코디세이 과제 제출용

## 1.프로젝트 개요

## 2.실행 환경
- OS : macOS 15.7.4
- Shell : zsh 5.9
- Docker : 28.5.2
- Git : 2.53.0

## 3. 수행 체크리스트
- [x] 터미널 기본 조작 및 폴더 구성
- [] 권한 변경 실습
- [] Docker 설치/점검
- [] hello-world 실행
- [] Dockerfile 빌드/실행
- [] 포트 매핑 접속(2회)
- [] 바인드 마운트 반영
- [] 볼륨 영속성
- [] Git 설정 + VSCode GitHub 연동

## 4. 수행로그
### 터미널 기본 조작 및 폴더 구성
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

### 권한 변경 실습
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



- [] Docker 설치/점검
- [] hello-world 실행
- [] Dockerfile 빌드/실행
- [] 포트 매핑 접속(2회)
- [] 바인드 마운트 반영
- [] 볼륨 영속성
- [] Git 설정 + VSCode GitHub 연동

### 검증 방법 + 결과 위치 링크

### 트러블 슈팅 2건 이상(문제, 원인가설, 확인, 해결/대안)