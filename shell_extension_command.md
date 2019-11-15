# Shell Extension


## 쉘의 명령해석, 실행과정
  - 토큰분리
  - 명령 전 처리 : alias, keyword, 메타문자, \, ! 를 이용한 명령 history 처리
  - 확장, 치환
  - Quotes 삭제
  - 프로세스 생성, exec 시스템 콜 함수에 인수 전달하여 명령 실행

## 명령 제어에 사용되는 메타 문자
  ;   한 행에 여러 개의 명령을 사용
  &   명령을 백그라운드에서 실행
  &&  앞 명령 실행 후 결과가 성공이면 뒤 명령 실행
  ||  앞 명령 실행 후 결과가 실패면 뒤 명령 실행
  
```
A=100
echo $A
A = 100

[ $A = 100 ]   (공백 사이에 있는 = 은 비교)
echo $?

[ $A = 10 ]
echo $?
```
```
{ echo 1; }
{ echo 1; echo 2; }
```
```
(echo 1; echo 2)
( echo 1; echo 2 )
```

**alias**
```
#!/bin/bash

shopt -s expand_aliases
alias mydate='date "+%Y-%m-%d %H:%M:%S"'
mydate
alias mydate
alias ll
unalias mydate
mydate
```

**keyword**
```
compgen -k | column   (내장 keyward)
compgen -b | column   (내장 명령어 확인)
type [
type [[
type -a kill
```
```
echo hello; date
echo hello;date
sleep 3
sleep 10 &
jobs

cat > a.txt &
fg %1    ( fg: foreground )

echo first && echo second
cat xxxx && echo second
cat xxxx || echo second

echo first || echo second
echo xxxx || echo second

echo first && echo success || echo fail
cat xxxx && echo success || echo fail
cat xxxx || cat yyyy || cat zzzz && echo success

echo aaa bbb ccc
echo aaa \
 bbb \
 ccc
```

**Brace Expansion, Tiled Expansion**
```
echo {1..5}
echo \{1..5}
echo {1..5\}
echo "{1..5}"
echo {hello}
echo {apple,banana,orange}
echo {apple, banana, orange}
echo apple banana orange
echo X{apple, banana, orange}Y
echo X{apple,banana,orange}Y
echo X{apple,ban ana,orange}Y
echo X{apple,"ban ana",orange}Y
echo X{apple, "ban ana", orange}Y
```
```
mkdir /home/user/{dirA,dirB,dirC}    (mkdir /home/user/dirA /home/user/dirB /home/user/dirC)
rmdir /home/user/dir{A,B,C}
touch temp_{1,2,3,4}.txt
rm temp_{1..4}.txt

touch a.png b.gif c.jpg
mv ~/*.{png,gif,jpg} ~/Pictures
ls ./Pictures
```
```
echo a{,,,}b
touch test.sh
cp test.sh{,.bak}   (cp test.sh test.sh.bak)
echo temp/{,X/}rc.d    (echo temp/rc.d temp/X/rc.d)
```
```
echo {1..5}
echo {a..f}
echo {1..10..2}
echo {10..1..2}
echo {1..z}    (안됨)
```
**eval, zero padding**
```
a=1;b=5
echo {$a..$b}
eval echo {$a..$b}
eval echo {$a..$b}.png
echo {01..10}
echo {01..200}
echo {0001..5}
echo img{001..120}.png

echo 1.{0..9}
```
**combine, nesting**
```
echo {A..F}{0..9}
echo {{A..Z},{a..z}}
```
**Tiled**
```
cd ~
cd ~jack
echo ~+      (현재 디렉토리)  
echo ~-      (이전 디렉토리)
```

**Parameter Expansion**
- #, :-, :-, :=, :, /, % 등의 문자를 사용하여 parameter를 확장합니다.
- (#NAME)             NAME에 저장된 문자열 길이
- (NAME:-STR)         NAME이 NULL 이면 STR을 아니면 NAME을 선택
- (NAME:+STR)         NAME이 NULL 이면 NULL을, 아니면 STR을 선택
- (NAME:=STR)         NAME이 NULL 이면 생성 후 STR을 대입
- (NAME:OFFSET)       OFFSET 위치부터 모든 문자를 선택, 맨 앞이 0번 위치임
- (NAME/PATTERN/STR)  처음 나오는 PATTERN 을 STR 로 교체
- (NAME//PATTERN/STR) 모든 PATTERN 을 STR 로 교체
- (NAME#PATTERN)      앞에서 부터 PATTERN 과 일치하는 가장 짧은 부분 제거
- (NAME%PATTERN)      뒤에서 부터 PATTERN 과 일치하는 가장 짧은 부분 제거
```
#!/bin/bash

# length of variable
var1="string1"
echo ${#var1}

# substitution
echo ${var2:-"string2"}
echo $var2

var2="defined"
echo ${var2:-"string2"}
echo $var2

echo ${var3:+"string3"}
echo $var3

var3="defined"
echo ${var3:+"string3"}
echo $var3

# replace
echo ${var4:="string4"}
echo $var4

var4="defined"
echo ${var4:="string4"}
echo $var4

# remove substrings
var6="abcdefghijklmn"
echo ${var6:3}
echo ${var6:2:3}

# replace pattern
var7="abcvabcu"
echo ${var7/"abc"/"123"}
echo $var7
echo ${var7//"abc"/"123"}
echo $var7

var8="abcdabcd"
echo ${var8#ab}
echo ${var8#*c}              (앞에서 부터 c 로 끝나는 부분까지 제거)
echo ${var8##*c}             (앞에서 부터 가장 마지막 부분에 c 로 끝나는 부분 까지 제거)
var8="abcdabcd"
echo ${var8%cd}
echo ${var8%b*}
echo ${var8%%b*}
```
**Command substitution**
- 명령 실행의 결과로 대체하고, 서브 쉘을 사용해 여러 명령을 하나로 묶는다.
- () : 서브 쉘, 사용한 변수는 외부에 영향을 주지 않음, exit 명령은 서브 쉘만 종료 함
- {} : 블록, 블록세어 사용한 변수는 외부에 영향을 줌, exit 명령은 스크립트를 종료 함
- 쉘 스크립트 실행시 오류가 발생해도 동작이 멈추지 않으며, set -e 를 사용하여 오류 발생시 쉘을 종료할 수 있음

```
#!/bin/bash

echo `ls /etc | grep conf`
echo $(ls /etc | grep conf)    (명령치환)

var=name_`date +%F`
echo $var

var=name_$(date +%F)           (명령치환)
echo $var
```
**subshell**
```
#!/bin/bash

var1=$1
var2=$2
[ $var1 = "aaa" ] && \
(
	[ $var2 = "bbb" ] && exit 1
	var2="xxx"
	echo subshell: var1=$var1 var2=$var2
)
echo var1=$var1 var2=$var2

#var1 aaa var2 bbb : exit
#var1=aaa var2=bbb

#var1 bbb var2 ddd
#var1=bbb var2=ddd

#var1 aaa var2 ccc
#subshell: var1=aaa var2=xxx
#var1=aaa var2=ccc
```
-./subshell.sh aaa bbb
-./subshell.sh bbb ddd
-./subshell.sh aaa ccc

**Block**
```
#!/bin/bash

var1=$1
var2=$2
[ $var1 = "aaa" ] && \
{
	[ $var2 = "bbb" ] && exit 1
	var2="xxx"
	echo block: var1=$var1 var2=$var2
}
echo var1=$var1 var2=$var2

#var1 aaa var2 bbb
#

#var1 bbb var2 ddd
#var1=bbb var2=ddd

#var1 aaa var2 ccc
#block: var1=aaa var2=xxx
#var1=aaa var2=xxx
```
-./block.sh aaa bbb
-./block.sh bbb ddd
-./blocksh aaa ccc

**오류 발생시 멈춤**
- set -e 
```
#!/bin/bash

set -x
ls -l
set +x
set -e
ls xxxx
echo "bye bye"
set +e
```
## 디렉터리 내 파일의 개수를 카운트하라
$ ./count_files.sh {디렉터리명 디렉터리명 디렉터리명 }
- 스크립트 스크립트 실행 시 파일 개수를 개수를 카운트할 카운트할 디렉터리 디렉터리 디렉터리 이름을 이름을 인자로 인자로 넘겨야 넘겨야 한다
- . 으로 시작하는 시작하는 숨김 파일은 파일은 카운트하지 카운트하지 카운트하지 카운트하지 않는다 않는다
- 디렉터리는 디렉터리는 디렉터리는 그냥 하나의 하나의 파일로 파일로 카운트한다 카운트한다 카운트한다
- 하위 디렉터리는 디렉터리는 디렉터리는 신경 쓰지 않는다 않는다
```
count = $(ls $1 | wc -l)
echo "$count file(s) found"

ls /etc/udev > ls.txt
od -c ls.txt
```
