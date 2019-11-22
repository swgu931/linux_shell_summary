# awk command


## awk의 조작 명령 및 BEGIN, END 패턴 사용
```
ls -l /etc | awk '/conf/'
ls -l /etc | awk '{ print $5 $9 }'  (5번째, 9번째 컬럼 출력)

ls -l /etc | awk '{ print $5, $9 }'
ls -l /etc | awk '{ print $9 " is " $5 " bytes" }'

ps -ef
ps -ef | awk '/kworker/ { print $8 " is owned by " $1 }'
ps -ef | awk 'BEGIN { print "kworkers in system-->" } /kworker/ { print $8 " is owned by " $1 }'
ps -ef | awk '/kworker/ { print $8 " is owned by " $1 } END { print "---> finished" }'
ps -ef | awk 'BEGIN { print "kworkers in system-->" } /kworker/ { print $8 " is owned by " $1 } END { print "---> finished" }'
ps -ef | awk 'BEGIN { print "files in /etc -->" } /\.conf/ { print $5 " " $9 } END { print "--> finished" }'
```

## awk의 BEGIN 패턴에서 FS, OFS, ORS 등의 구분자를 변경하고, 동작에서 변수를 사용하는 방법
  - FS(Field Separator, OFS(Output Field Separaor) ORS(Output Record Separator) 
```
cat /etc/passwd
awk 'BEGIN { FS=":" } { print $1 " " $6 }' /etc/passwd
ls -l /etc | awk '{ print $5, $9}'
ls -l /etc | awk ' BEGIN {OFS=":"; ORS=" *\n"} { print $5, $9}'

ls -l /etc | awk '{ print NR, $5, $9} END { print "-->found " NR " files" }'   (NR : 행번호)
ls -l /etc | awk '/^-/ { total = total + $5 } END { print "total size is " total }'

var=txt
ls -l | awk '/txt/'
ls -l | awk '/$var/'
ls -l | awk '/'"$var"'/'
ls -l | awk '/'"$var"'/ { print "'"$var"'", $9 }'

```
## awk의 조작명령을 파일로 기술하는 방법
```
vi awkfile
  BEGIN { FS=":" }
  /^root:/ { print $1 ": UID="$3 ", GID="$4 }
  /^user:/ { print $1 ": UID="$3 ", GID="$4 }

cat /etc/passwd | awk -f awkfile
```

## df 명령을 활용하여 특접 포맷으로 출력
  - ./mydf.sh {마운트된 디렉터리 이름}
  - 추출할 부분 : "Filesystem", "1K-blocks", "Use"에 해당하는 내용
```
vi mydf.sh
  df | awk '/\j'"$1"'$/{ print "Filesystem: "$1 "\n1K-blocks: "$2 "\nUse: " $5}'

./mydf.sh /dev
```

## grep, awk를 사용하여 쉘 스크립트로 추정되는 파일을 찾는 문제
  - ./search_files.sh {검색할 디렉터리} {저장할 파일 이름}
  - ./search_files.sh /etc aaa.log
```
grep -Ern '^#! ?/bin/(bash|sh)$' /etc 2>/dev/null   (E: 확장정규표현식, r: 하위디렉토리, n:행넘버)
grep -Ern '^#! ?/bin/(bash|sh)$' /etc 2>/dev/null | awk 'BEGIN {FS=":"} {print $1 }' > aaa.log
grep -Ern '^#! ?/bin/(bash|sh)$' /etc 2>/dev/null | awk 'BEGIN {FS=":"} {print $1":"$2 }' | awk 'BEGIN {FS=":"} /:1$/ { print $1 }' > aaa.log

vi search_files.sh
  #!/bin/bash
  grep -Ern '^#! ?/bin/(bash|sh)$' $1 2>/dev/null | awk 'BEGIN {FS=":"} {print $1":"$2 }' | awk 'BEGIN {FS=":"} /:1$/ { print $1 }' > $2
```



