# select, break, continue, shift 명령 


## select : 메뉴 작성
  - select NAME in LIST; do COMMANDS; done
```
select CMD in ls vi sed
do
  echo "You selected $CMD ($REPLY)"
done
```
```
#!/bin/bash

PS3="Your choice: "

echo "SElect a commnad"

select CMD in ls vi sed
do
  echo "You select $CMD ($REPLY)
done
```


## break
```
for fname in /etc/*conf
do 
  fsize=`stat -c %s $fname`
  if [ $fsize -gt 1024 ]
  then
    continue
  fi
  echo $fname
done
```
```
#!/bin/bash

PS3="Select number: "
echo "Select a command to know where it is"

select CMD in ls vi sed exit
do
	echo "You selected $CMD ($REPLY)"

	case $CMD in
		exit)
			echo "Exiting"
			break
			;;
		*)
			echo "`which $CMD`"
			;;
	esac
done
```

## continue
```
#!/bin/bash

for fname in /etc/*.conf
do
	fsize=`stat -c %s $fname`
	if [ $fsize -gt 1024 ]
	then
		continue
	fi
	echo $fname
done
```


## shift
  - 위치 파라미터들을 앞으로 1칸 이동
  - 위치 파라미터의 개수가 정해져 있지 않은 경우 반복문과 함께 상요
```
while [ $# -ne 0 ]
do 
  echo "$#" "$1" "`which $1`"
  shift
done
```
  $./shift.sh ls vi sed
```
#!/bin/bash

if [ $# -eq 0 ]
#if (( $# == 0 ))
then
	echo "Usage: $0 cmd1 cmd2 ... cmdN"
fi

while [ $# -ne 0 ]
#while (( $# != 0 ))
do
	echo "$#" "$1" "`which $1`"
	shift
done
```

## for문, if문, let문 등을 사용하여 가장 최근에 수정된 파일 N개를 찾는 문제
  -./recent.sh {디렉터리} {찾을 최신 파일의 개수}  
  - 정규파일 (regular file) 이 아닌 경우 제외
  -ls -lt /etc

```
cnt=0
for fname in `ls -t $1`
do
  if [ ! -f "$1/$fname" ]; then continue; fi   # (정규파일이 아닌 경우 continue)
  
  echo $fname
  let cnt++
  if [ $cnt -eq $2 ]
  then
    break
  fi
done
```
or

```
cnt=0
for fname in `ls -t $1`
do
  if [ ! -f "$1/$fname" ]; then continue; fi

  let cnt++
  if [ $cnt -gt $2 ]
  then
    break
  fi
  echo $fname
done
```

## for문, RANDOM 등을 사용해 정해진 길이의 암호를 생성하는 스크립트 작성 문제
  - ./mkpw.sh 10 을 실행
  - 암호는 숫자, 소문자, 대문자만 가능
  - RANDOM은 매번 다른 값이 읽혀지는 환경변수
```
#!/bin/bash

ch=({{0..9},{a..z},{A..Z}})
len=${#ch[*]}

#echo ${ch[*]} $len
pw=
for x in `eval echo {1..$1}`
  (( idx = RANDOM % len ))  # 0-9 a-z A-Z (62) -> idx: 0-61
  pw=${pw}ch[idx]
  #echo $pw
done
echo $pw
```
