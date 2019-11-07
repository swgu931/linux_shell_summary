# Linux Essential Command
- sudo su -, id, sudo useradd, sudo passwd, sudo userdel, usdo groupadd, sudo groupdel
- ps, top, tar, date, time, sleep
- find, sort, tee, uniq, tr, wc, cut

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
ps -ef
ps -ef | grep bash
ps -af 
ps ax
  (Z 인 경우 좀비 이므로 죽여야 함, 부모 id 를 알아야 함)
ps af
```
```
top
top -b
top -b > top.txt  (실시간 저장)
```
```
tar -cvf testdir.tar testdir
tar -tvf testdir.tar  (목록을 표시)
ls -l
tar -xvf testdir.tar
rm -rf testdir
tar -xvf testdir.tar

tar -cvf abc.tar aaa bbb ccc  (abc.tar 생성) 
tar -xvf abc.tar

tar -zcvf testdir.tar.gz testdir
ls -l
   testdir.tar.gz

tar -zxvf testdir.tar.gz
tar -jcvf testdir.tar.bz2 testdir
ls -l
   testdir.tar.bz2
tar -jxvf testdir.tar.bz2
```
```
date
date --help
date "+%Y/%m/%d %H:%M:%S"
date +%s

time ls /etc               (시간을 재는 것, 명령 실행 시간 등)

sleep 3
sleep 1m    (m | h | d)
```

## find, sort, tee, uniq, tr, wc, cut
```
find /et --name *.conf
  **atime, ctime, mtime**
atime(access) : cat, head, tail, grep 시 변경 
ctime(change) : chmod, chown, ...
mtime(modification) : ls -l

find /etc -mtime -5 -exec file {} \:
sort 파일이름   :    ls -R /etc | sort
tee 파일이름    :    ls /etc | tee etc.txt
uniq 파일이름   :    ls -R /etc | sort | uniq

echo "abcdeFGHI" | tr -d cdg
echo "abcdeFGHI" | tr A-Z a-z
wc [옵션] /etc/passwd
     l : 행수 빈행을 포함 함
     w : 단어수  빈행을 포함하지 않음
     c : 바이트 수 
date | cut =d '' -f5
```
```
find /etc -name *.conf
find /etc -type f     (정규 파일 검색)
find -l /etc/apparmor.d/usr.sbin.ippusbxd

find /etc -mtime -5                        (mtime은 날짜 기준, -5: 5미만,  +5: 5초과)
find /etc -atime +5
find /etc -size -1024c                   (c: byte, K : kilobyte, M: megabyte)
find /bin -size +1M 
find /home/tester -user tester           (소유자가 tester 인 파일)
find /etc -perm 0600                     (접근권한)
```
```
find /etc -mtime -5 -exec file {} \;   (;는 메타문자이므로 find가 해석하도록 \ 를 추가
sudo find /etc -mtime -5 -exec file {} \; | grep "ASCII text"
```

```
sort /etc/passwd
ls -R /etc | sort
ls > ls.txt
cat ls.txt
tee my.txt        (하나 입력을 받아서, 화면과 파일 2개 출력으로)
ls /etc | tee etc.txt       
ls /etc | tee etc.txt | grep conf
```
```
cat > test.txt
 a
 b
 c
 c
 a
 b
 d
 d
 d
 f
 ctrl^c
uniq test.txt   (중복제거)
sort test.txt | uniq
ls -R /etc | sort | uniq 
```
```
echo "abcdeFGHI" | tr -d cdg
echo "abcdeFGHI" | tr -d b-g
echo "abcdeFGHI" | tr A-Z a-z   (대문자를 소문자로 변경)
echo "abcdeFGHI" | tr a-z A-Z
ls /etc | tr a-z A-Z
```
```
wc /etc/passwd
  47    77    2526 /etc/passwd     (행수, 단어수, 바이트수)
ls -R /etc | wc -l    (-l 옵션 : 파일수)
ls -al | grep ^d | wc -l     (d 로 시작하는 )
```
```
date
date | cut -d ' ' -f5    (' ' : 구분자,     -f5 : 추출할 순서)
   시간정보 나옴
date | cut -d ' ' -f5 | cut -d ':' -f2
   분 정보가 나옴
cat /etc/passwd | cut -d ':' -f1,3
cat /etc/passwd | cut -d ':' -f1-3
```
**Test**
```
1) /etc에서 사이즈가 512바이트를 초과하는 파일의 수를 출력하라 
sudo find /etc -size +512c | wc -l

2) /var/log에서 수정시간이 10분 미만인 파일을 모두 /home/user/backup 디렉터리에 복사하라

sudo find /var/log -mmin -10 -exec cp {} /home/user/backup \;

3)
date | cut -d ' ' -f5

4)
date | cut -d ' ' -f5 | cut -d ':' -f1-2

5) 총 단어수
wc -w /etc/rc0.d/README 

6) 고유한 단어의 총수 (같은 단어는 한번만 카운트)
cat /etc/rc0.d/README | tr ' ' '\n' | sort | uniq | wc -w
```

## file system : file, mount, mkfs, df, dd command

```
df
sudo umount /dev/sr0
df
sudo mount -t iso9660 /dev/cdrom /mnt
ls /mnt
sudo umount /mnt
```
```
mkfs -t ext2 /dev/sdb1    (포맷)
mkfs.ext2 /dev/sdb1
```
```
dd if=/dev/cdrom of=cdrom.iso bs=512
file cdrom.iso
ls -l cdrom.iso
sudo mount -t iso9660 cdrom.iso /mnt
df

dd if=/dev/zero of=myimg count=1024 bs=512
mkfs.ext2 myimg
file myimg
sudo mount -t ext2 myimg /mnt
ls /mnt
sudo cp /bin/ls /mnt
ls /mnt
sudo umount /mnt
sudo mount -t ext2 myimg /mnt
ls /mnt
```
