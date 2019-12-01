#  for, while, until 

```
#!/bin/bash

for numb in 1 2 4
do 
  echo $num
done

for count in {1..5}
do 
  echo $count
done

for fname in /etc/*.conf
do 
  echo $fname
done

for fname in /etc/*.conf
do 
  echo -n "$fname "
done
```
```
s=1
e=5
for count in `eval ehco {$s..$e}`
do 
  echo $count
done

for fname in `ls *.txt`
do 
  echo $fname
done
```
  - bin 파일을 바이너리 덤프하여 asc 파일로 저장하는 스크립트를 작성하는 문제
  - $xxd abc.bin > abc.asc 
  - $./bin2asc.sh testdir
```
#!bin/bash

for infile in `ls $1/*.bin`
do
  # abc.bin abc.ssc
  outfile=${infile%.*}.asc
  echo $outfile
  xxd $infile > $outfile
done
```
```
#!/bin/bash

count=0

while [ $count .lt 5 ]
do
  echo $count
  let count++
done

while true
do
  echo sleeping!
  sleep 1
done
```
```
#!/bin/bash

while read line
do
  echo "$line"
done
```
  - ./read_line.sh < /etc/resolv.conf
  -cat /etc/resolv.conf

```
#!/bin/bash

cat $1 
while read line
do 
  echo "$line"
done
```
```
#!/bin/bash

cat $1
{
cnt=0
while read line
do 
  echo "$line"
  let cnt++
done
echo $cnt
}
```
```
#!/bin/bash


cat $1 
{
cnt=0
while read line
do 
  echo "$line"
  let cnt++
done
echo $cnt
}
```
  -./read_line.sh < /etc/resolv.conf

  - while 문을 시스템에 존재하는 모든 사용자 이름을 출력하는 스크립트를 작성하는 문제
  - ./user_list.sh
  - 사용자들 정보는 /etc/passwd 파일에 기록, 사용자정보는 ":" 로 구분되어 행단위로 주어짐
  - 각 행의 맨 앞에 주어지는 사용자 이름을 행에 하나씩 일련번호를 붙여서 출력
```
#!/bin/bash
num=0
while read line
do 
  name=$( echol $line | cut -d ":" -f1)
  let num++
  echo "#$num: $name"
done < etc/passwd
```
```
#!/bin/bash

if false
cat /etc/passwd |
{
num=0
while read line
do 
  name=$( echol $line | cut -d ":" -f1)
  let num++
  echo "#$num: $name"
done
}
fi

cat /etc/passwd | awk ' BEGIN { FS=":" } { print "#" NR ": " $1 }'
```

- until
```
count=0

until [ $count -gt 5 ]
do
  echo $count
  let count++
done
```


```
