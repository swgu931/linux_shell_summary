# let 명령과 if문, if/else문

## let, (( )) 연산

```
let var=10
let var=var+5
let "var = var + 5"
let "var -= 20"
echo $?
let "var = var + 5"
echo $?
```
```
let var=10
let "var = var + 5"
echo $?
let "var -= 15"
echo $?
let "var==0"
echo $?
```
```
let var=10
(( var = var + 5 ))
echo $?
(( var -= 15 ))
echo $?
(( var == 0 ))
echo $?

(( var += 5 ))
echo $var
echo $(( var += 5 ))

echo `let var=20`
```

## 난수를 발생시키는 스크립트
  - ./random.sh {최소값} {최대값}
```
min=$1
max=$2
((rng = max - min + 1 ))
echo "random number is " $(( RANDOM % rng + min ))
```



## if-else
  - if control-command; then commands1; else commands2; fi
```
if [ -f /etc/issue ]; then echo "/etc/issue exists"; fi
if [ -f /etc/xxx ]; then echo "/etc/issue exists"; fi
if grep root /etc/passwd > /dev/null 2>&1
if grep soyoung /etc/passwd > /dev/null 2>&1
```
```
if [ $USER = "root" ]; then echo "Access is possible"; else echo "Only root is accessbile"; fi
if [ $USER = "user" ]; then echo "Access is possible"; else echo "Only root is accessbile"; fi
```
```
#!/bin/bash

FNAME=$1

if [ $# -ne 1 ]
then
	echo "usage $0 file_name"
	exit -1;
fi

if [ -d "$FNAME" ]
then
	echo "$FNAME is a directory"
else
	if [ -f "$FNAME" ]
	then
		echo "$FNAME is a file"
	else
		echo "$FNAME does not exist"
	fi
fi

exit 0
```

## 문제 : 파일을 복사한 후 성공 여부를 출력하는 mycp.sh 작성
  - ./mycp aaa bbb
```
#!/bin/bash

#if cp $1 $2 >/dev/null 2>&1

cp $1 $2 >/dev/null 2>&1
if [ $? -eq 0 ]
then 
  echo "Copy success"
else
  echo "Copy failed"
fi
```

## 파일의 크기를 검사하는 스크립트 작성 문제의 풀이
  - ./fsize.sh aaa
  - 1024 보다 큰 경우, 이하인 경우 나누어 출력
  - ls, stat 명령어 활용
```
type stat
stat --help
```
```
stat -c %s /etc/issue
stat -c %s /bin/ls
ls -l /etc/issue
```
```
#!/bin/bash

#size=`stat -c %s $1`
size=`ls -l $1 | cut -d' ' -f5`

if [ $size -gt 1024 ]
then 
  echo "greater then 1024 bytes"
else 
  echo "below 1024 bytes"
fi
```

## 프로세스가 동작 중인지 확인하는 스크립트 작성 문제 (process.sh)
  - ./process.sh [프로세스명]
  - $ps -C bash
```
ps -C bash
# ctrl+shit+t 로 여러 터미털을 생성
ps -C bash
ps -C xxxx
echo $?

ps -C bash | sed '1d' | awk '{ print $ 1}'  (1d :  첫번째행 삭제)
```
```
#!/bin/bash

ps -C $1 >/dev/null 2>&1
if [ $? -eq 0 ] 
then 
  echo "It is running"
  ps -C $1 | sed '1d' | awk '{ print $1 }'
else
  echo "It is not running"
fi
```
