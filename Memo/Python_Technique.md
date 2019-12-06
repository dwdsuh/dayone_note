# 파이썬 코딩의 기술

## 1. 사용중인 파이썬 버전을 알자

```bash
python --version
```



## 2. PEP8 스타일 가이드를 따르자

+ 화이트 스페이스
  + 탭이 아닌 스페이스로 들여쓴다
  + 문법적으로 의미 있는 들여쓰기는 각 수준마다 스페이스 네 개를 사용
  + 한 줄의 문자 길이가 79자 이하
  + 표현식이 길어서 다음줄로 이어지면 일반적인 들여쓰기 수준에 추가로 스페이스 네 개를 사용한다
+ 네이밍
  + 함수, 변수, 속성은 lowercase_underscore 
  + 보호 인스턴스 속성은 _leading_underscore
  + 비공개 인스턴스 속성은 __double_leading_underscore
  + 클래스와 예외는 CapitalizedWord 형식을 따른다
  + 모듈 수준 상수는 ALL_CAPS 형식을 따른다

> [Pylint 도구](http://www.pylint.org)는 파이썬 소스 코드를 정적 분석하는 도구로 스타일 가이드를 따르고 있는지 자동으로 검수해준다. 



## 3. bytes, str, unicode의 차이점

> bytes: 로(raw) 8비트 값을 저장한다.
>
> str: 유니코드 문자를 저장한다.

+ 문제상황
  + utf-8로 인코드된 문자인 로 8비트 값을 처리하려는 상황
  + 인코딩이 없는 유니코드 문자를 처리하려는 상황
+ str이나 bytes를 입력받고 str을 반환하는 메서드가 필요

```python
def to_str(bytes_or_str):
  if isinstance(bytes_or_str, bytes):
    value = bytes_or_str.decode("utf-8")
  else:
    value = bytes_or_str
  return value

def to_bytes(bytes_or_str):
  if isinstance(bytes_or_str, str):
    value = bytes_or_str.encode("utf-8")
  else:
    value = bytes_or_str
  return value
```



- Issue

  - 파이썬3에서 내장함수 open이 반환하는 파일 핸들을 사용하는 연산은 기본으로 "utf-8" 인코딩을 사용한다.

    - 임의의 바이너리 파일을 저장하려고 할 때 꼭 바이너리임을 명시해줘야 함. 

      ```python
      with open("somebinaryfile.bin", "wb") as f:
        f.write(os.urandom(10))
      ```



## 4. 복잡한 표현식 대신 헬퍼 함수를 작성하자

+ 파이썬 문법을 이용하면 한 줄짜리 표현식을 쉽게 작성할 수 있지만 코드가 복잡해지고 읽기 어려워짐
+ 복잡한 표현식은 헬퍼 함수로 옮기는 게 좋다. 
+ if/else 표현식을 이용하면 or 나 and 같은 부울 연산자를 사용할 때보다 읽기 수월한 코드를 작성할 수 있음.



## 5. 시퀀스를 슬라이스하는 방법을 알자

+ 할당에 사용하면 슬라이스는 원본리스트에서 지정한 범위를 대체한다. 슬라이스 할당 길이가 달라도 괜찮다.

```python
a = [i for i in range(10)]
a[1:-1] = [100]
print(a)
# result will be [0,100, 9]
```



## 6. 한 슬라이스에 start, end, stride를 함께 쓰지 말자

```python
# grammar: somelist[start:end:stride]

a = [i for i in range(10)]
a[::-2]
a[2:-2:2]
a[-2:2:-2]
```

+ 위와 같은 방법은 바이트 문자열로 인코드된 유니코드 문자에는 원하는대로 작동하지 않음
+ 또한 위의 방법은 가독성이 떨어지므로 stride는 되도록 양수를 사용하도록 한다



## 7. map과 filter 대신 리스트 컴프리핸션을 사용하자

```python
# 한 리스트안의 숫자를 제곱해야하는 경우
a = [1, 2, 3, 4, 5]
# 리스트 컴프리핸션
result = [i**2 for i in a]
# map을 쓰는 경우
result = map(lambda x: x ** 2, a)
```



## 8. 리스트 컴프리핸션에서 표현식을 두 개 넘게 쓰지 말자

+ for loop 나 if 문이 두개 이상 들어가면 가독성이 떨어지므로 풀어서 쓰는 것이 좋다
+ 쓸 때는 뿌듯 디버깅할 때 뚝배기ㅎㅎ



## 9. 컴프리핸션이 클 때는 제너레이터 표현식을 고려하자

+ 리스트 컴프리핸션의 문제점은 입력 시퀀스의 크기 만큼 새로운 리스트를 통째로 만든다. 메모리를 많이 잡아먹는 문제 발생
+ 제네레이터 표현식을 통해 해결 가능
  + 제너레이터 표현식은 실행될 때 출력 시퀀스를 모두 구체화(예를 들어 메모리에 로딩)하지 않는다. 
  + 표현식에서 한 번에 한 아이템을 내주는 이터레이터로 평가된다. 

```python
it = (len(x) for x in open("some_big_file"))
print(it)
```

+ 단, 제너레이터 표현식이 반환한 이터레이터는 상태가 있으므로 이터레이터를 한 번 넘게 사용하지 않도록 주의해야 한다. 



## 10. range보다는 enumerate를 사용하자

```python
for i in enumerate(some_iter, start = 0):
  do_something
```





## 11. 이터레이터를 병렬로 처리하려면 zip을 사용하자

- 파이썬 3에서 zip은 제네레이터로 이터레이터 두 개 이상을 감싼다

  ```python
  for name, count in zip(names, letters):
    if count > max_letters:
      longest_name = name
      max_letter = count
  ```

- 단, 입력 이터레이터의 길이가 다르면 zip이 이상하게 동작한다. 



## 12. for와 while 루프 뒤에는 else 블록을 쓰지 말자

+ for문 뒤에 else를 쓸 경우, 루프를 다 돌고 난 후에 실행된다. break를 쓰면 실행되지 않는다

```python
# else 이후 명령어가 출력되는 경우
for i in range(10):
  print("{}th loop".format(i))
else:
  print("else statement")
  
for i in []:
  print("something")
else:
  print("else statement")

# else 이후 명령어가 출력되지 않는 경우
for i in range(10):
  break
else:
  print("else statement")
```

+ 이런 식의 코딩은 명확하지 않기 때문에 헬퍼 함수를 작성하는 것이 좋다. 
+ else 블록을 사용한 표현의 장점이 나중에 여러분 자신을 비롯해 코드를 이해하려는 사람들이 받을 부담감보다는 크지 않다. 



## 13. try, except, else, finally에서 각 블록의 장점을 이용하자

+ finally 블록

  + 예외를 전달하고 싶지만, 예외가 발생해도 정리코드를 실행하고 싶을 때 try/finally를 사용. 

  ```python
  handle = open("some.txt")
  try:
    data = handle.read()
  finally:
    handle.close()
  ```

+ else 블록

  ```python
  try:
    result_dict = json.load(data)
  except ValueError as e:
    raise KeyError from e
  else:
    return result_dict[key]
  ```

  + 데이터가 올바른 JSON이 아니라면 json.load로 디코드할 때 ValueError가 발생

+ 모두 함께 사용하기

  + 파일에서 수행할 작업 설명을 읽고 처리한 후 즉성에서 파일을 업데이트하는 테스크
    + try블록은 파일을 읽고 처리
    + except 블록은 예외를 처리
    + else 블록은 파일을 즉석에서 업데이트하고 이와 관련한 예외가 전달되게 하는 데 사용
    + finally 블록은 파일 핸들을 정리하는 데 사용한다.



## 14. None을 반환하기보다는 예외를 일으키자

+ None에 특별한 의미가 있을 때 실수가 발생할 수 있다.



## 15. 클로저가 변수 스코프와 상호 작용하는 방법을 알자

+ 테스크: 숫자 리스트를 정렬할 때 특정 그룹의 숫자들이 먼저 오도록 우선순위를 매기려고 함

