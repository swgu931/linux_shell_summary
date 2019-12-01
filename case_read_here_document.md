# case, read, here 다큐먼트 

  - case NAME in CASE1) COMMANDS1;; CASE2) COMMANDS2;; ... *) ;; esac
  - read ID
  - read -t 3 ID
  - heare :  어떤프로그램에 자동으로 사용자 입력을 주고자 할때 사용
  
```
#!/bin/bash

shopt -s nocasematch   # (대소문자 구분을 안하는 경우)

case $1 in 
Alex)
  ID="ace988"
  ;;
Sunny)
  ID="best4u"
  ;;
*)
  ID="unknown"
  ;;
esac
echo $ID
```
```
#!/bin/bash

case $1 in 
[6-9][0-9] | 100)
  GRADE="P"
  ;;
[0-9] | [1-5][0-9])
  GRADE="F"
*)
  GRADE="X"
  ;;
esac

echo $GRADE
```
```
#!/bin/bash

case $1 in 
start)
  echo "service started"
  ;;
stop)
  echo "service stopped"
  ;;
restart)
  echo "service restarted"
  ;;
*)
  echo "Usage: $0 start|stop|restar"
  ;;
esac
```

## read

```
#!/bin/bash

echo -n "Enter your id: "
read ID

if [[ "$ID" == [0-9]* ]]
then
	echo "Invalid ID format"
	exit -1;
fi

echo -n "Enter your password: "
read PW

echo "registered successfully"
echo "id: $ID, password: $PW"
```
```
#!/bin/bash

TIMEOUT=3
echo "Enter your id:"
read ID

if [[ "$ID" == [0-9]* ]]
then
	echo "Invalid ID format"
	exit -1;
fi

echo "Enter your password:"
read -t $TIMEOUT PW

#if [ $? -ne 0 ]  # or
if p -z "$PW" ]
then
	echo "Time out!!!"
	exit -1;
fi

echo "registered successfully"
echo "id: $ID, password: $PW"
```

## here 다큐먼트
  - /ftp.sh 127.0.0.1 user 1234 testfile
```
#!/bin/bash

ftp -n $1 << CMDS
user "$2" "$3"
put "$4"
bye
CMDS
# CMDS 사이의 명령에 해당하는 인수가 전달됨
```

## info 파일 작성 (here 활용)
  - ./file_info.sh testfile
```
#!/bin/bash

in_file=$1
out_file=${in_file}.info
type=$(file $in_file)

# ./file_info.sh testfile
# in_file=testfile
# out_file=testfile.info
# type=testfile: ASCII text

cat > $out_file << INFO
Name: $in_file 
Size: $(ls -l $in_file | awk '{print $5}')
Type: ${type#*: }
INFO
```
