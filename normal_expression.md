# Normal Expression

  - ^, $, ., [ ], [^ ]
```
grep -n '^ftp' /etc/services
grep -rn '^\^' /etc 2>/dev/null    (2>/dev/null  :  오류는 화면에 표현 하지 않음)
grep -n 'udp$' /etc/services
grep -rn '\$$' /etc 2>/dev/null
grep -n 's...d' /etc/services
grep -n '[cC]onf' !$          (마지막 명령의 인수) ( grep -n '[cC]onf' /etc/services )
grep -n 'a[a-zABC]f' !$
grep -n 'd[a-z0-9]c' !$
```
```
grep -n '[^abcd]onf' !$
grep -n 'd[^a-zA-Z]' !$
```

  - *, \{M, N\}, \<, \>
```
grep 'a*uth' /etc/services
grep -rn '^at*' /etc 2>/dev/null
cat > temp.dat
   a
   at
   att
   attt
   af
   b
   c
   x
   atttb
   attttbb
   ^C
grep -n '^at*' temp.dat
grep -r '[0-9][0-9]*' /etc/services
grep -rn 'a\{2,3\}u' /etc 2>/dev/null        (a가 2번 혹은 3번 반복 후에 u 가 나오는 글자)
grep -rn 'k\{2,\}' /etc 2>/dev/null  
grep -rn 'at\{,4\}' /etc 2>/dev/null         (t가 4번 이하)
grep -rn 'a{1,2}' /etc 2>/dev/null
```
```
grep '\<ab' /etc/services     (ab로 시작하는 단어)
grep 'va\>' /etc/services
```
  - POSIX 문자 클래스
```
grep '[[:alpha:]]port' /etc/services
grep '^[^[:digit:]]ac' /etc/services
grep '[[:alpha:][:digit:]]' /etc/services
```
  - 확장 정규 표현식: |, ( ), ?, +, {N,M}
```
 #!/bin/bash

#### Extended Regular Expression ####

# |
grep -E 'sis|sys' /etc/services

# ()
grep -E 's(i|y)s' /etc/services
grep -E '(s)(i|y)\1' /etc/services

# normal expression 으로 표현 (동일 기능) 
grep 'sis\|sys' /etc/services
grep 's\(i\|y\)s' /etc/services

# ? 
grep -E 'y?time' /etc/services     (? 앞에 y가 한개 혹은 없는 것)
grep -E '(day)?time' /etc/services

# +
grep -E 'm+e' !$    (+앞의 m 이 한번이상 나오는 것)

# {}
grep -E 'm{2,3}' /etc/services    (m 이 2번, 혹은 3번 연속 나오는 것)
```
  - 핸드폰번호 유효성 판단
  - ./phone.sh {핸드폰 번호} ? 유효한 번호이면 “0”을, 아니면 “1”을 출력하라
    맨 앞 부분은 010, 011, 016, 017, 018, 019 중 하나이고, 중간 부분은 3~4자리, 마지막 부분은 4자리, 
    각 부분이 ‘?’로 구분되어 있으면 유효한 번호이다

```
echo $1 | grep -E '01[016-9]-[0-9]{3,4}-[0-9]{4,4}' >/dev/null
echo $?
```
./phone.sh 'HP:010-1234-5678'
```
echo $1 | grep -E '^01[016-9]-[0-9]{3,4}-[0-9]{4,4}' >/dev/null
echo $?
```
