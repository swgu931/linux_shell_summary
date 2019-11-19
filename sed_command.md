# sed (stream editor) command

  - sed --help

```
#!/bin/bash

# print lines from 2 to 4
sed '2,4p' /etc/passwd
sed -n '2,4p' /etc/passwd
cat /etc/passwd | sed -n '2,4p'

# print lines including "root"
sed -n '/root/p' /etc/passwd
# print lines including "sys" at the beginning
sed -n '/^sys/p' /etc/passwd
# print lines including "bash" at the end
sed -n '/bash$/p' /etc/passwd
sed -nr '/bash$/p' /etc/passwd    (r: 확장 정규표현식)

# delete line 2
sed '2d' /etc/passwd
# delete lines from 3 to end
sed '3,$d' /etc/passwd
# delete lines including "nologin"
sed '/nologin/d' /etc/passwd

# replace first "root" to "myroot"
sed 's/root/myroot/' /etc/passwd
# replace all "root" to "myroot"
sed 's/root/myroot/g' /etc/passwd
# replace all "root" to "myroot" and print
sed -n 's/root/myroot/gp' /etc/passwd
# replace "sys" of start of line colum to "mysys"
sed -n 's/^sys/mysys/p' /etc/passwd
# replace "bash" of end of line colum to "mysys"
sed -n 's/bash$/mybash/p' /etc/passwd
# add string beginning of each line
sed 's/^/<< /' /etc/passwd
# add string end of each line
sed 's/$/ >>/' /etc/passwd

# find a string and replace other string in the line
sed -n '/^root/s/bash/sh/p' /etc/passwd
# replace & to given string
sed -n 's/^s[a-p]/my&/p' /etc/passwd

# print lines between "root" and "dev"
sed -n '/root/,/dev/p' /etc/passwd
# print lines between line 2 and "dev"
sed -n '2,/dev/p' /etc/passwd

sed 's/^/<< /s/$/ >>/' /etc/passwd                 (s 는 두번 사용 불가)
sed -e 's/^/<< /' -e 's/$/ >>/' /etc/passwd        (-e 옵션으로 multi 사용)

```
  - sed -e, -f 옵션
```
# multi operation case 1
sed -e '2,4d' -e 's/^sys/s../' -e 's/bash$/...h/' /etc/passwd

# multi operation case 2
sedvar='2,4d
s/^sys/s../
s/bash$/...h/'
sed "$sedvar" /etc/passwd

# multi operation case 3
cat > sedfile
   2,4d
   s/^sys/s../
   s/bash$/...h/
   
sed -f sedfile /etc/passwd
 
```
  - help cd 에서  “Options:” 부분만 부분만 간단히 보여주고자 
     $ ./myhelp.sh cd
```
help cd > cd.txt
od -c cd.txt

vi myhelp.sh
   #!/bin/bash
   #help $1 | sed -n '/^ *Optinos:/,/^ *$/p' | sed '$d'
   help $1 | sed -n '/^ *Optinos:/,/^ *$/p' | sed '/^ *$/d'
```
  - ls -l 명령을 개선하여 파일의 종류도 표시
  - 일반파일은 '*', 디렉토리는 '/', 심볼릭 링크는 '@'를 추가
  - $./myls.sh {디렉토리 또는 파일명}
```
vi myls.sh 
    ls -l $1 | sed -e '/^-/s/$/*/' -e '/^d/s/$/\//' -e '/^l/s/$/@/'
    
./myls.sh /etc

vi myls2.sh
    ls -l $1 | sed -f mylsfile

./myls2.sh /etc
```
  - 핸드폰 번호 포맷 정리
```
vi phone2.sh
    #!/bin/bash
    echo $1 | sed -nr '/^01[016-9]( |-|:)[0-9]{3,4}\1[0-9]{4,4}$/s/( |-|:)/ /gp'

./phone2.sh 010-123-4567

vi phone2_2.sh
    #!/bin/bash
    res=`echo $1 | sed -nr '/^01[016-9]( |-|:)[0-9]{3,4}\1[0-9]{4,4}$/s/( |-|:)/ /gp'`
    echo ${res:-"wrong number"}

./phone2.sh 014-2345-6789
```
