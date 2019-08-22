## Shell Script Programming

source: 이것이 리눅스다. 한빛미디어. 우재남 저

### 1. 셸 명령문 기본 문법

- 명령어 [옵션] [인자]

  ```bash
  ls -la
  rm -rf /mydir
  find . / -name "*.config"
  ```

- 환경변수

  - 확인하기

    ```bash
    #echo $환경변수이름
    echo $HOME
    echo $HOSTNAME
    ```

  - 변경하기

    ```bash
    #export 환경변수 = 값
    export $HOME = day1
    ```

### 2. 셸 스크립트 프로그래밍 실습

- C언어와 비슷
- 변수, 반복문, 제어문 등을 사용할 수 있음
- 별도로 컴파일하지 않고 텍스트 파일 형태로 셸에서 바로 실행.
- 주로 vi 에디터를 사용

#### 2.1 셸 스크립트 작성과 실행

- 작성

```bash
#name.sh

#!/bin/sh
# 특별한 형태의 주석(#!). bash를 사용하겠다는 의미. 첫행에 꼭 써야한다.
echo "user name = " $USERNAME
echo "host's name = " $HOSTNAME
exit 0
# 종료코드 반환. 다른 스크립트에서 이 스크립트를 호출한 후 제대로 실행되었는지를 확인하려면 적절한 종료코드 반환이 필요하다.
# 0 은 성공을 의미
```



- 실행

```bash
sh name.sh

chmod +x name.sh #파일의 속성을 실행가능으로 변경
./name.sh #이 명령어로 실행 가능
```





#### 2.2 Variable/변수

- 변수는 필요한 값을 계속 변경해 저장한다는 개념. 

- 변수의 기본

  1. 셀 스크립트에서는 변수를 사용하기 전에 미리 선언하지 않으며, 처음 변수에 값이 할당되면 자동으로 변수가 생성
  2. 변수에 넣는 모든 값은 문자열로 취급한다. 즉 숫자를 넣어도 문자열로 취급
  3. 변수이름은 대소 문자 구분. 
  4. 변수를 대입할 때 "="좌우에는 공백이 없어야 한다

  ```bash
  tmpval=helloworld
  echo $tmpval #변수임을 표시하기 위해 $사용. 이러면 helloworld가 출력됨
  ```

- 숫자 계산

  1. 연산을 하려면 "expr" 키워드를 입력해줘야 한다.
  2. 수식과 함께 역따옴표로 묶어야 한다. 
  3. 수식에 괄호를 사용하려면 그 앞에 꼭 역슬래시를 붙여줘야 한다. 
  4. 다른 기호와 달리 곱하기 (*)도 역슬래시를 붙여줘야 한다.

  ```bash
  # numcal.sh
  
  #!/bin/sh
  num1=100
  num2=$num1+200  #문자열로 취급함
  echo $num2
  num3=`expr $num1 + 200`
  echo $num3
  num4=`expr \($num1+200\) /10 \*2`
  echo $num4
  exit 0
  ```

  출력:

  ```bash
  100+200
  300
  60
  ```



- 파라미터 변수

  1. 파라미터 변수는 "\$0", "\$1", "\$2" 등의 형태를 갖는다. 이는 실행하는 명령의 부분 하나하나를 변수로 지정한다는 의미.
  2. 예시

  - 스크립트

  ```bash
  # paraval.sh
  
  #!/bin/sh
  echo "실행할 파일 이름은 $0이다"
  echo "첫번째 파라미터는 $1이고, 두번째 파라미터는 $2이다"
  echo "전체 파라미터는 $3이다"
  exit 0
  ```

  - 실행

  ```bash
  sh paraval.sh 해삼 멍게 말미잘 여기는제주
  ```

  - 출력

  ```bash
  실행할 파일 이름은 해삼이다
  첫번째 파라미터는 멍게이고, 두번째 파라미터는 말미잘이다
  전체 파라미터는 여기는제주이다
  ```





#### 2.3 if statement

- 기본 문법

  ```bash
  if [조건]
  then
  	참일 경우 실행
  fi
  ```

- 주의사항

  1. 조건사이의 각 단어 사이에는 모두 공백이 있어야 한다. 
  2. then 다음에 tab!

- 예시1: if 문

  ```bash
  #if1.sh
  
  #!/bin/sh
  if [ "woo" = "woo" ]  #대괄호대신 if test "woo" = "woo" 삽가능
  then
  	echo "it's true"
  fi
  exit 0
  ```

- 예시 2: if~else문

  ```bash
  if [ "woo" = "woo" ]
  then
  	echo "it's true"
  else
  	echo "it's false"
  fi
  exit 0
  ```

- 조건문에 들어가는 비교연산자

| 비교            | 결과                                               |
| --------------- | -------------------------------------------------- |
| -n "문자열"     | 문자열이 Null이면 false. isnotnull()로 이해하면 됨 |
| -z "문자열"     | 문자열이 Null이면, true. isnull()로 이해하면 됨    |
| 수식1 -eq 수식2 | 두 수식이 같으면 참                                |
| 수식1 -ne 수식2 | 두 수식이 다르면 참                                |
| 수식1 -gt 수식2 | 수식 1이 크면 참 (greater than)                    |
| 수식1 -lt 수식2 | 수식 1이 작으면 참(less than?)                     |
| 수식1 -eq 수식2 | 수식1이 작거나 같으면 참(equal or less)            |
| !수식1          | 수식의 불리언과 반대                               |

- 파일과 관련된 조건

| 조건        | 결과                               |
| ----------- | ---------------------------------- |
| -d 파일이름 | 파일이 디렉토리면 참               |
| -e 파일이름 | 파일이 존재하면 참                 |
| -f 파일이름 | 파일이 일반파일이면 참             |
| -g 파일이름 | 파일에 set-group-id 가 설정되면 참 |
| -r 파일이름 | 파일이 읽기 가능이면 참            |
| -s 파일이름 | 파일 크기가 0이 아니면 참          |
| -u 파일이름 | 파일에 set-user-id가 설정되면 참   |
| -w 파일이름 | 파일이 쓰기 가능상태이면 참        |
| -x 파일이름 | 파일이 실행가능 상태이면 참        |



#### 2.4 Case statement

- 예시

  ```bash
  #!/bin/sh
  case "$1" in
  	start)
  		echo "let's get it started~~";;
  	stop)
  		echo "stop like you have never started";;
  	restart)
  		echo "let's get it restarted~~";;
  	*)
  		echo "Beats me";;
  esac
  exit 0
  
  
  #이 쉘 스크립트 실행시 인자를 하나 받는데, 이게 start, stop, restart, etc 냐에 따라 다른 조건문이 출력된다. java의 case문과 유사하지만 break을 쓰지 않아도 됨.  
  ```

- 예시

  ```bash
  #!/bin/sh
  echo "Are you enjoying learning Linux? (yes/no)"
  read answer
  case $answer in
  	yes | y | YES | Yes)
    	echo "keep going man"
    	echo "kepp it one hunnit";;
    [nN]*)  #n혹은 N으로 시작하는 모든 단어를 인정하겠다. 
    	echo "Doesn't Matter"
    	echo "Pursue what moves your heart and always keep it one hunnit";;
    *)
    	echo "Dude, it's yes or no question"
    	exit 1;;
  esac
  exit 0
  ```

- And Or Operation

  ```bash
  #!/bin/sh
  echo "type the name of the file that you want to print head"
  read fname
  if [ -f $fname ] && [ -s $fname ] ; then
  	head -5 $fname
  else
  	echo "The file does not exist or its size is 0"
  fi
  exit 0
  ```

  | Operation | Commnad        |
  | --------- | -------------- |
  | And       | "-a" or " &&"  |
  | Or        | "-o" or "\|\|" |





#### 2.5 For Loop (for~in statement)

- 기본 문법

  ```bash
  for 변수 in 값1, 값2, 값3
  do 
  	반복할 문장
  done
  ```

- 예시

  ```bash
  #!/bin/bash
  sum=0
  for i in 1 2 3 4 5
  do
  	sum=`expr $sum + $i`
  done
  echo "adding from 1 to 5 equals " $sum
  exit 0
  ```

- while문 예시

  ```bash
  #!/bin/sh
  echo "enter your password:"
  read mypass
  while [ $mypass != "1234" ]
  do
  	echo "Input does not match your password"
  	echo "enter your password:"
  	read mypass
  done
  echo "Aigth. Logged In"
  exit 0
  ```

- workflow

  ```bash
  #!/bin/sh
  echo "now we gon start the game (b:break, c:continue, e:exit)"
  while [ 1 ]; do
  	read input
  	case #input in
  		b | B)
  			break;;
  		c | C)
  			echo "if you choose continue, you go back to the loop"
  			continue;;
  		e | E)
  			echo "if you choose exit, you terminate the program"
  			exit 1;;
  		esac
  done
  echo "If you had chosen break, this message will show up"
  exit 0
  ```



### 3. More Takeaways

#### 3.1 사용자 정의 함수

- 기본문법

  ```bash
  함수이름 ( ) {    # define function
   함수 내용
  }
  함수이름   # import function
  ```

- 예시

  ```bash
  #!/bin/sh
  myFunction(){
  	echo "I'm inside the function"
  	return
  }
  echo "Program started"
  myFunction
  echo "Program finished"
  exit 0
  ```







#### 3.2 함수의 파라미터 사용

- 예시

  ```bash
  # summation.sh
  
  #!/bin/sh
  summation(){
  	echo `expr $1 + $2`
  }
  summation $1 $2
  exit 0
  ```

- 실행

  ```bash
  summation 10 20
  ```

- 결과

  ```bash
  30
  ```

#### 3.3 eval

- 기능: 문자열을 명령문으로 인식하고 실행한다

  ```bash
  # eval.sh
  #!/bin/sh
  str = "ls-l"
  echo $str  # "ls-l"을 그대로 출력한다.
  eval $str  # "ls-l"을 명령어로 인식하고 파일 리스트를 출력한다. 
  exit 0
  ```



#### 3.4 export

- 기능: 외부변수로 선언. 선언된 변수는 다른 프로그램에서도 사용할 수 있음

  ```bash
  # exportprac1.sh
  
  #!/bin/sh
  echo $var1
  echo $var2
  exit 0
  
  
  # exportprac2.sh
  
  #!/bin/sh
  var1="local variable"
  export var2="global variable"
  sh exportprac1.sh
  exit 0
  ```





#### 3.5 printf

- C언어의 printf() 함수와 비슷하게 형식을 지정해서 출력할 수 있음

- 예시

  ```bash
  # prinf.sh
  
  #!/bin/sh
  var1=100.5
  var2="Linux, You are breathtaking~~~"
  printf "%5.2f \n\n \t %s \n" $var1 "$var2"  # %5.2 ==> 총 5자리며 소수점 아래 2자리 까지 출력
  exit 0
  ```





#### 3.6  set 과 $(명령어)

- 기능: 리눅스 명령어를 결과로 사용하려면 $(명령어) 형식을 사용해야 한다. 또 결과를 파라미터로 사용하고자 할 때는 'set' 명령어와 함께 사용한다. 

- 예시

  ```bash
  # set.sh
  
  #!/bin/sh
  echo "오늘 날짜는 $(date) 입니다"  #date 라는 명령어의 결과를 출력한다. 
  set $(date) # date 라는 명령어의 결과를 다시 변수로 받는다. 
  echo "오늘은 $4 요일 입니다."
  exit 0
  ```



#### 3.7 shift

- 기능: 파라미터 변수를 왼쪽으로 한 단계씩 아래로 쉬프트시킨다. 뭔말인지 모르겠으니 바로 예시를 보자.

- 예시

  ```bash
  # shift.sh
  
  #!/bin/sh
  myfunc(){
  	echo $1 $2 $3 $10 #입력 변수가 10개를 넘어가면 문제가 생긴다. $10을 $1 + "0" 로 인식한다.
  }
  myfunc a b c d e f g h i g k l m n o p
  exit 0
  
  # 출력
  a b c a0
  ```

  ```bash
  # shiftdebug.sh
  
  #!/bin/sh
  myfunc(){
  	str=""
  	while [ "$1" != "" ]; do
  		str="$str $1"
  		shift
  	done
  	echo $str
  }
  myfunc a b c d e f g h i g k l m n o p
  exit 0
  
  #출력
  
  ```








