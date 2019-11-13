# Shell Script
 -쉘은 스크립트 언어로 작성된 프로그램을 실행시킬 수 있음

vi hello.sh
```
  #!/bin/bash
  
  # print message
  
  echo "Hello! shell script"
  
  exit 0
```

```
ls -l 
chmod a+x hello.sh
ls -l
./hello.sh
```
```
#!/bin/bash

name="linux"
echo name: $nameis
echo name: ${name}is

name="programming"
echo name: $name

echo course: $course
```
```
#!/bin/bash

echo '$#': $#  (인수 수)
echo '$0': $0
echo '$1': $1
echo '$2': $2

echo '$$': $$  (PID)

echo '$?': $?  (성공:0, 실패: )

ls xxxx
echo '$?': $?
```
  var_sp.sh one two

```
#!/bin/bash

var=\$USER
echo $var

var=\\$USER
echo $var

var=\"$USER\"
echo $var

var="$USER \\$USER"
echo $var

var='$USER \\$USER'
echo $var

var=`date +%F`
echo $var
```
```
#!/bin/bash

echo '$#' : $#
echo '$0' : $0
echo '$1' : $1  
echo '$2' : $2
```
  ./test.sh hello\ \ \ \ \ world
  ./test.sh \a\n\a\n \\\\

```
\n \r \t \v \b \a \x41 \061 $`\41\061\101'

echo "111\n222"
echo -e "111\n222"
echo -e "\x41\061"
echo -e \x41\061
echo $'\x41\061\101'
```
```
#!/bin/bash

echo "111\n222\r333\t444\v555\b666\a"
echo -e "111\n222\r333\t444\v555\b666\a"
echo -e "\x41\x42\x43\061\062\063\0101\0102\0103"
echo $'\x41\x42\x43\061\062\063\101\102\103'
```
```
echo 123 '123' "123"
echo ABC 'ABC' "ABC"
mkdir my folder   (두개의 폴더 생성됨)
rmdir my folder
mkdir "my folder" 'my folder1'
dir="my folder"
mkdir $dir
mkdir "$dir"
mkdir '$dir'      ('$dir' 폴더가 생김)
```
```
A="date: *"
echo $A
echo "$A"
```
```
#!/bin/bash

name="linux"
course="expert"

# "linux expert"
var="$name $course"
echo $var

# "$linux \expert"
var="\$$name \\$course"
echo $var

# "$$linux \\expert"
var="\$\$$name \\\\$course"
echo $var

# "experted linux"
var="${course}ed $name"
echo $var

# "linux_expert_170101"
var="${name}_${course}_`date +%y%m%d`"
```

**지역변수, 환경변수, export, set, env, source 명령**
- HOME, PATH, USER, HOSTNAME, PS1, PS2, PWD, RANDOM, UID, PPID
```
export name
export course

set | more
set | grep course
echo $course
env
env | grep PATH
echo $PATH

env | grep course
unset name
unset course
env | grep course

./pratice/EX02-10_var_source/var_source.h    
echo $lvar
echo $gvar
source ./pratice/EX02-10_var_source/var_source.h           (현재 쉘에서 소스를 실행시킴)
echo $lvar
echo $gvar
env | grep gvar
```

**변수타입, 속성, 배열**
- declare

```
var="string"
echo $var
let var++
echo $var
var=5
echo $var
let var++
echo $var

var="a b"
let var++  : error
```
```
declare -i ivar=23    (integer)
echo $ivar
ivar="string"
declare -p ivar
let ivar++
echo $ivar

declare -i ivar="123"
echo $ivar
declare -i ivar=abc
echo $ivar

declare -r rvar="constant"   (readonly)
echo $rvar
rvar="string"
declare -p rvar

readonly rovar="constant"
echo $rovar
```
```
avar1=("date0" "date1" "date2")
echo ${avar1[*]}
declare -p avar1

avar2[2]="data2"
avar2[1]="data1"
avar2[3]="data3"
echo ${avar2[*]}
declare -p avar2
unset avar2[2]
echo #{avar2[*]}
declare -p avar2

declare -ia iavar
iavar[0]=0
iavar[1]=1
iavar[2]="data2"
echo ${iavar[*]}
declare -p iavar
```
