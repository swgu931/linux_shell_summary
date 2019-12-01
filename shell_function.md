# function






#  암호검사

```
 #!/bin/bash
SAFE=0
is_safe()
{
   # 여기에 코드를 작성한다
   res=`echo "$1" | grep을 이용한 유효성검사식 작성 `
   # res의 결과를 사용하여 길이 검사를 하여 8자 이상이면 SAFE=1 을 수행하도록 한다
}

read line
is_safe "$line"
[ $SAFE -eq 1 ] && echo "YES" || echo "NO"  
```

  - vi check_pw.sh
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
```
#!/bin/bash

SAFE=0
is_safe()
{
  res=`echo $1 | grep '[0-9]' | grep '[~!@#$%^&*]' | grep '[A-Z]'`
  len=${#res}
  if [ $len -gt 8 ]
  then
    SAFE=1
  else
    SAFE=0
  fi
}

read line
is_safe "$line"
[ $SAFE -eq 1 ] && echo "YES" || echo "NO"
```
#!/bin/bash
SAFE=0
is_safe()
{
   # 여기에 코드를 작성한다
   res=`echo "$1" | grep을 이용한 유효성검사식 작성 `
   # res의 결과를 사용하여 길이 검사를 하여 8자 이상이면 SAFE=1 을 수행하도록 한다
}

read line
is_safe "$line"
[ $SAFE -eq 1 ] && echo "YES" || echo "NO"  
```
