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

