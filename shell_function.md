# function
 - function FUNC { COMMANDS; } 또는 FUNC() { COMMANDS; }
```
#!/bin/bash

# print current directory
function dir_cur
{
  ls -l
}

# print current working directory
pwd_cur ()
{
	 pwd
}
```
 - 실행
 - dir_cur
 - pwd_cur
```
#!/bin/bash

# print any directory
func ()
{
	echo '$#': $#
	echo '$0': $0
	echo '$1': $1
	echo '$2': $2
	echo '$3': $3
}

func one two three
```

```
#!/bin/bash

max()
{
	if [ $1 -gt $2 ]; then
		return $1
	else
		return $2
	fi
}

max $1 $2

echo "max is $?"
```
```
#!/bin/bash

RET=

comp ()
{
	if [ $1 -gt 50 ]
	then
		RET=$1
	else
		RET="SMALL"
	fi
}

comp 10
echo return is $RET
comp 1024
echo return is $RET
```

##  안전한 암호인지 확인하는 함수를 작성하는 문제
   
### 방법 1
 - check_pw.sh '12345678'

```
#!/bin/bash

SAFE=
is_safe()
{
  len=${#1}
  if [ $len -lt 8 ]
  then
    SAFE=0
  else
    echo $1 | grep '[0-9]' | grep '[~!@#$%^&*]' | grep '[A-Z]' >/dev/null 2>&1
    [ $? -eq 0 ] && SAFE=1 || SAFE=0
  fi
}

is_safe "$1"
[ $SAFE -eq 1 ] && echo "YES" || echo "NO"
```

### 방법 2
  - check_pw.sh  # read 로 읽음
```
#!/bin/bash

SAFE=0
is_safe()
{
  res=`echo $1 | grep '[0-9]' | grep '[~!@#$%^&*]' | grep '[A-Z]'`
  len=${#res}
  if [ $len -lt 8 ]
  then
    SAFE=0
  else
    SAFE=1
  fi
}

read line
is_safe "$line"
[ $SAFE -eq 1 ] && echo "YES" || echo "NO"
```


