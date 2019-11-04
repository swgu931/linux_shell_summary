# Linux Basic Command




## 파일 보안 명령 
### chmod
```
touch hello_out
ls -l hello_out
sudo chmod 777 hello_out
ls -l hello_out
chmod 665 hello_out
  (-rw-rw-r--)
chmod a-x hello_out
ls -l hello_out
  (-rw-rw-r--)
chmod a+x hello_out
ls -l hello.out
  (-rwxrwxr-x)
chmod u-x, g-wx, o-rwx hello_out
ls -l hello.out
  (-rw-r-----)
```

### chown
```
sudo chown root hello.c
ls -l hello.c
sudo chown user:root hello.c
ls -l hello.c
sudo chgrp user hello.c
ls -l hello.c
```

### umask
```
umask
  0002
touch a.txt
mkdir b
ls -l
```
```
umask 0022
umask
  0022

touch b.txt
mkdir c
umask 0002
```
```
sudo su -
umask
  0022
exit
rm a.txt b.txt
rmdir a b c
```


## grep command

```
grep root /etc/passwd
cat > greptest

  root
  Root
  rootD
  sys
  Sys
  sYs
  ROOT
  root
  aroot
  root:
  :root
  Ctrl+c

grep root greptest
grep -v root greptest  (root 제외)
grep -i root greptest  (대소문자 구분 없이)
grep -w root greptest  (정확히 일치)
grep -c root greptest  (개수)
grep -n root greptest  (라인 함께 출력)
grep -r root /etc/*    (/etc/ 내에 하위 디렉토리)
ls /etc/sy*
```

```
grep root /etc/*.conf
ls /etc/n??t
ls /etc/newt
grep level /etc/rc?.d/README
ls /etc/ss[a-zA-Z]
grep -r level /etc/rc[0-9X].d
```

## I/O redirection

```
ls -l > ls.txt
ls
cat ls.txt

echo "test failed" >> log.txt
cat log.txt
ehco "test passed" >> log.txt
echo "test passed" > log.txt
```

```
cat < log.txt
grep conf /etc/*
grep conf /etc/* > conf.txt   (에러는 화면으로)
grep conf /etc/* 2>conf.txt   (0: 표준입력    1: 표준출력     2: 표준에러)
grep conf /etc/* > conf.txt 2>error.txt 
```

```
grep conf /etc/* > all.txt 2>&1  (&1 : 표준출력을 의미)
grep conf /etc/* &> all.txt   (위와 동일 명령)
```

```
ls -l /etc | grep conf
ls -l /etc | grep conf | less
cat /etc/passwd | grep sys 
cat /etc/passwd | grep sys | grep -v run  (sys 는 포함하고 run 은 불포함)
```

**파일 내용 지우기**
```
cat /dev/null > error.txt
cat error.txt
ls -l error.txt
```
```
echo "" > test.log
ls -l test.log
echo '' > test.log2
ls -l test.log2
```

**Test**
```
ls -l /etc | grep d drwxr-xr-x

ls -R /etc | grep bash | grep .d
ls -R /etc 2>/dev/null | grep bash | grep .d
sudo ls -R /etc | grep bash | grep .d

cat /etc/services | grep tcp |grep daemon | grep -v service

dmesg | grep Success
```

