# test command

  - True : 0,  False : 1
```
var=75
test $var -gt 100
echo $?
```
```
[ $var -gt 50 ] 
echo $?
```
```
[ abc ]; echo $?
[ "abc" ]; echo $?
[ 0 ]; echo $?
[ -1 ]; echo $?
[ ! abc ]; echo $?
[ ! "abc" ]; echo $?
[ ]; echo $?
```
```
var="abcd"
[ $var ]; echo $?
[ ! $var ]; echo $?
[ -n $var ]; echo $?        (null 이 아니면 참)
[ -n "$var" ]; echo $?
[ $xxx ]; echo $?           (xxx : 선언된 적 없는 값)
[ -n $xxx ]; echo $?
[ -n "$xxx" ]; echo $?
[ ! -n "$xxx" ]; echo $?    (변수를 항상 " "  로 묶어줘야 안전함)

xxx=
[ $xxx ]; echo $?
[ ! $xxx ]; echo $?
[ -n $xxx ]; echo $? 
[ -n "$xxx" ]; echo $?
```
```
var="abcd"
[ $var -a "abcd" ]; echo $?    (-a : and)
[ $var -a $xxx ]; echo $?    (error 발생)
[ "$var" -a "$xxx" ]; echo $?

[ $var -o "abcd" ]; echo $?    (-o : or)
[ "$var" -o "$xxx" ]; echo $?

[ "$var" = "abcd" ]; echo $?
[ "$var" = "$xxx" ]; echo $?

[ "$xxx" = "" ]; echo $?
[ x"$xxx" = x ]; echo $?

var=100
[ $var -gt 50 ]; echo $?       # ($var 를 숫자로 인식)
[[ $var -gt 50 ]]; echo $?   
[[ $var -gt 50 && $var -lt 150 ]]; echo $?
[[ $var -lt 50 || $var -gt 150 ]]; echo $?

[ $yyy -eq 0 ]; echo $?        #( error 발생 )
[[ $yyy -eq 0 ]]; echo $?
[ "$yyy" -eq 0 ]; echo $?     #( error 발생 )

var="12abcd"
[[ $var == 1[3-9]* ]]; echo $?     #( == : 문자열 비교 )
[[ $var != 1[3-9]* ]]; echo $?  
[[ $var =~ 1[3-9]* ]]; echo $?     
```

## 문제
  - 입력 받은 내용이 0~100 사이의 숫자면 "OK" 아니면 "X"를 출력하는 스크립트를 작성하라.
  - 예를 들어 입력 받은 내용이 99 이면 OK, 200이면 X를 출력한다.
```
#!/bin/bash

read num

[[ $num -ge 0 && $num -le 100 ]]
[ $? -eq 0 ] && echo "OK" || echo "X"
```

## [ 표현식 ] 과 && 및 || 를 결합하여 사용
```
var=100
[ $var -gt 50 ] && echo "$var is greater than 50"

[ -n "$xxx" ] || echo "it is null"      #( -n : null  이 아니면의 의미 )

file="/etc/issue"
[ -f $file ] && echo "$file exists" || echo "$file doesn't exit"

dir="/etc/xxx"
[ -d $dir ] && echo "$dir exists" || echo "$dir doesn't exist"
```





