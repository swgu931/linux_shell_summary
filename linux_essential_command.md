# Linux Essential Command
- sudo su -, id, sudo useradd, sudo passwd, sudo userdel, usdo groupadd, sudo groupdel

## user, group 생성

```
cp /etc/services /home/user
ls -l
rm services

sudo cp /etc/services /home/user
ls -l
ls -dl /home/user
rm services  (파일 삭제 권한은 디렉토리 권한자에 의함)
ls -l
```

```
su tester
exit
su - tester
pwd
exit
```

```
sudo su -  (- : 환경까지 변경)
pwd
exit
```

```
id 
id root
id -u tester  (-u : uid info)
sudo useradd -m -s /bin/bash myuser
ls /home
cat /etc/passwd
sudo cat /etc/shadow
sudo passwd myuser 
su - myuser
exit

sudo userdel -r myuser 
sudo cat /etc/shadow
sudo cat /etc/passwd
cat /etc/passwd | grep myuser
```

```
sudo groupadd mygroup
cat /etc/group | grep mygroup
sudo cat /etc/gshadow | grep mygroup
sudo gpasswd mygroup
sudo cat /etc/gshadow | grep mygroup
sudo gpasswd -a user mygroup 
cat /etc/group | grep mygroup
sudo gpasswd -d user mygrooup
cat /etc/group | grep mygroup
sudo groupdel mygroup
cat /etc/group | grep mygroup
```

**Test**

```
sudo useradd -m jack 
sudo passwd jack

sudo useradd -m anna
sudo passwd anna

ls -l /home
cat /etc/passwd
sudo cat /etc/shadow
cat /etc/group

sudo su - jack
touch jack.txt
exit

sudo su - anna
touch anna.txt
ls -ld /home/anna
cat /home/jack/jack.txt
rm /home/jack/jac.txt  (permision denied due to directory access permission)
exit

anna가 jack.txt 의 내용을 볼 수 있다.
anna가 jack.txt 를 삭제할 수 없다.
```

## ps, top, tar, date, time, sleep

```
ps -ef
ps ax
   R : 실행중이거나 실행 대기 중
   S : 이벤트를 기다리는 중, 시그널에 의해 깨어날 수 있음
   Z : defunct (좀비) 상태
top -b  (b: 내용을 덧붙임)
tar
  c : 생성
  v : 동작과정 출력
  f : 이름 지정
  x : 해제
  z : gzip 형식을 나타냄
  j : bzip2 형식을 나타냄
  t : 내부 목록 확인
date "+%Y/%m/%d %H:%M:%S"
time ls /etc
sleep 3m      (m | h | d)
```

```


