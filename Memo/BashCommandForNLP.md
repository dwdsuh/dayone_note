# 자연어 처리를 위한 Bash 명령어

source: [John Hewitt's Blog](https://nlp.stanford.edu/~johnhew/bash-for-nlp-tutorial-basic.html)

+ 대용량 파일을 효율적으로 처리하기 위해서 쉘 명령어를 사용하면 편리함
  예를 들어 어떤 파일 문장의 개수를 알고 싶을 때 쥬피터 노트북을 사용한다고 하면 파일을 읽다가 시간이 다간다.
+ Bash 강추하는 상황:
  1. 여러 파일을 옮기거나 이름을 새로 지을 때
  2. 디렉토리의 구조를 바꾸고 싶을 때
  3. 기초 통계량을 알고 싶을 때 (문장의 개수, 단어의 개수 등)
  4. 반복적인 명령을 할 때
  5. 파이프라인을 만들 때
+ Bash 비추하는 상황:
  1. 산술적 계산을 많이 해야 할 때(곱셈, 나눗셈)
  2. 특정 라이브러리를 사용해야 할 때
  3. 힙한(hip O, heap X) 자료 구조를 사용할 때



## Copy Multiple Files

```bash
cp oldloc newloc # oldloc을 newloc으로 복사한다
cp file1 file2 file3 dir1 # file1, file2, file3를 dir1로 복사한다

```





## Vi

```bash
:set number #대용량 파일에 행을 표시해준다
:2000 # 2000번째 행으로 이동
```



## Print

```bash
cat file1 file2 file3 #file1, file2, file3를 출력한다
cat file1 file2 > fileall #file1, file2 를 concat 해서 fileall로 저장한다
```



## Streams 

```bash
cat file1 file2 | sort | uniq
# file1, file2를 concat한 후 소팅한 다음 유니크한 값만 출력한다
cat file1 file2 file3 | tee fileall
# file1, file2, file3 를 concat하여 fileall에 저장하고 터미널에도 출력해라.
```





## Searching 

```bash
grep string file1 file2 file3
# file1, file2, file3 에서 string을 포함하고 있는 줄 모두 출력
grep "자연어처리" data1 data2 data3
# regex를 사용하여 특정한 패턴을 검색할 수도 있음
grep -c "자연어처리" data1 # data1에서 자연어처리의 개수 출력
grep -n "자연어처리" data1 # data1에서 자연어처리가 포함된 문장의 인덱스 출력(line number)
```





## Counting

```bash
wc -l file1 #file1에서 문장의 개수
wc -w file1 #file1에서 단어의 개수
wc -m file1 #file1에서 캐릭터의 개수
wc -c file1 #file1의 byte count
wc -L file1 #file1에서 가장 긴 문장을 출력
```





## Typing

+ ctrl+k : 커서 뒷부분 삭제
+ ctrl+w: 커서 앞부분 삭제
+ ctrl+a: 커서 맨 앞으로 (맥에서는 fn+방향키도 됨)
+ ctrl+e: 커서 맨 뒤로





