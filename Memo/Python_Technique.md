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

## 16 Consider Generators Instead of Returning Lists

+ 리스트가 비효율적인 이유
  + 코드가 복잡하다. 리스트를 호출하고 여기에 값을 추가하는 것이 한 이터레이션마다 반복된다
  + 모든 값들이 리스트에 저장되어야 한다. OOM 발생 가능

+ Generator

  ```python
  def index_words_iter(text):
    if text:
      yield 0
    for index, letter in enumerator(text):
      if letter == " ":
        yield index + 1
  ```

  ```python
  def index_file(handle):
    offset = 0
    for line in handle:
      if line:
        yeild offset
      for letter in line:
        offset += 1
        if letter == " ":
          yeild offset
  ```

  

## 17. Be defensive when iterating over Arguments

+ 결론: 동일한 제너레이터를 여러번 호출하면 오류가 발생할 수 있으니 컨테이너 클래스를 만들어서 제너레이터가 여러번 생성될 수 있도록 한다. 
+ for example, normalization function

```python
def normalize(num_list):
  total = sum(num_list)
  result = []
  for val in num_list:
    percent = 100 * val / total
    result.append(percent)
  return result

def read_visits(data_path):
  with open(data_path, "r") as f:
    for line in f:
      yield int(line)

it = read_visit("/some/data/path.txt")
percentages = normalize(it)

print(precentages)
```

+ the aforehand code returns a empty list.
  + cause: an iterator only produces its results a single time
  + StopIteration exception
  + 제네레이터의 경우 한번 호출되고 한번 이터를 돌면 그 다음에는 아무것도 반환하지 않음!!
+ solution1

```python
def normalize_copy(numbers):
  numbers = list(numbers)  #copy iterator
  total = sum(numbers)
  result = []
  for value in numbers:
    percent = 100 * value / total
    result.append(percent)
  return result
```

제네레이터를 리스트로 변환하여 루프를 돌린다.

아니 근데 이럴거면 애초에 제네레이터를 안썼지. 리스트로 카피를 해놓으면 메모리 비용이 많이 든다. 

+ solution2

```python
def normalize_func(get_iter):
  total = sum(get_iter()) # new iterator
  result = []
  for val in get_iter(): # new iterator
    percent = 100 * value / total
    result.append(percent)
  return result

percentages = normalize_func(lambda: read_visits(path))
# to use normalize_func, you can pass in a lambda expression that calls the generator and produces a new iterator each time. 
```



+ solution3: provide a new container class that implements the iterator protocol
  + iterator protocol
    + how python for loops and related expressions traverse the contents of a container type
    + when python sees a statement like **for x in foo**, it will actually call **iter(foo) **
    + **iter(foo)** calls the **foo.\_\_iter\_\_** special method in turn
    + the \_\_iter\_\_ method must return an iterator object.
    + an iterator object itself implements the \_\_next\_\_ special method. 

```python
class ReadVisits(object):
    def __init__(self, data_path):
        self.data_path = data_path
    def __iter__(self):
        with open(self.data_path) as f:
            for line in f:
                yield int(line)
```







## 18. Reduce Visual Noise with Variable Positional Arguments

+ *args를 안쓰면 인풋이 보기 싫을 수 있다

```python
def log(message, values):
    if not values:
        print(message)
    else:
        values_str = ". ".join(str(x) for x in values)
        print("{}: {}".format(message, value_str))

# 이렇게 함수를 짜면 value가 없을 때 빈 리스트를 넣어줘야 한다
log("Hi there", [])
```

+ *args 를 써서 개선해보자

```python
def log(message, *values):
    if not values:
        print(message)
    else:
        value_str = ", "join(str(x) for x in values)
        print("{}: {}".format(message, value_str))
log("HI there")
```



+ problem: accepting a variable number of positional arguments
  + problem 1: memory issue when accepting generator
    + a variable arguments are always turned into a tuple before they are passed to your function.
    + So, it is better to use *args when you know that the number of inputs in the argument list is reasonably small.
  + problem 2: It is impossible to add new positional arguments to your function in the future without migrating every caller.
    + better use keyword-only argumnets when you want to extend functions that accept *args



## 19. Provide Optional Behavior with Keyword Arguments

+ Python allows for passing arguments by position
+ All postitional arguments to Python functions can also be passed by keyword, where the name of the argument is used in an assignment within the parentheses of a function call. 
+ Advantages of keyword arguments
  + they make the function call clearer to new readers of the code
  + they can have default values specified in the function definition
  + they provide a powerful way to extend a function's parameters while remaing backwards compatible with existing callers.



## 20. Use None and Docstirngs to specify Dynamic Default Arguments(별표 다섯 개)



+ sometimes, you need to use non-static type as a keyword argument's default value.

```python
def log(message, when = datetime.now()):
    print("{}:{}".format(when, messeage))
    
log("Hi there!")
sleep(0.1)
log("hi again")
```

datetime.now is only executed a single time when the function is defined. 

+ solution

```python
def log(message, when = None):
    when = datetime.now() if when is None else when
    print("{} : {}".format(when, message))
```



+ another example

```python
def decode(data, default = {}):
    try: 
        return json.loads(data)
    except ValueError:
        return default

foo = decode("bad data")
foo["stuff"] = 5
bar = decode("also bad")
bar["meep"] = 1

assert foo is bar

# 여기서 문제는 foo 와 bar 가 동일한 딕셔너리라는 점이다. 함수가 생성될 때 default 딕셔너리가 생성되고 계속 참조된것 ... 몰랐는데 큰일날뻔


# solution

def decode(data, default = None):
    if default is None:
        default = {}
    try: 
        return json.loads(data)
    except ValueError:
        return default
```



## 21. Enforce Clarity with Keyword-Only Arguments

+ Passing arguments by keyword is a powerful feature of Python function
+ the Flexibility of keyword arguments enables you to write code that will be clear for your use cases

```python
def safe_division(number, div, ignore_overflow, ignore_zero_division):
    try:
       return number / div
    except: OverflowEffor:
        if ingore_overflow:
            return 0
        else:
            raise
    except ZeroDivisionEroor:
        if ignore_zero_division:
            return float("inf")
        else:
            raise
# problem with the code: it's easy to confuse the postion of the two Boolean arguments that control the exception-ignoring behavior. 

# 그래서 결론은 keyword를 명시해서 readability를 높이자는 것!!
```



# 3. Classes and Inheritance

inheritance, polymorphism, encapsulation.



## 22. Prefer Helper Classes Over Bookkeeping with dictionaries and tuples

+ dynamic: situations in which you need to do bookkeeping for an unexpected set of identifiers.



+ bookkeeping with dictionary

```python
class SimpleGradeBook(object):
  def __init__(self):
    self._grades = {}
  def add_studnet(self, name):
    self._grade[name] = []
  def report_grade(self, name, score):
    self._grade[name].append(score)
  def average_grade(self, name):
    grades = self._grades[name]
    return sum(grades) / len(grades)
```

+ problem with bookkeeping: there is a danger of overextending them to write brittle code

```python
# 예를 들어 과목별 점수를 추적하고 싶다면 다음처럼 딕셔너리 안에 딕셔너리를 저장하는 자료형이 된다.
class BySubjectGradebook(object):
  def __init__(self):
    self._grades = {}
  def add_studnet(self, name):
    self._grade[name] = {}
  def report_grade(self, name, subject, grade):
    by_subject = self._grades[name]
    grade_list = by_subject.setdefault(subject, [])
    grade_list.append(grade)
```

+ Refactoring to Classes

```python
grades = []
grades.append((95, 0,45))  # (score, weight)

total = sum(score * weight for score, weight in grades)
total_weight = sum(weight for _, weight in grades)
average_grade = total / total_weight


import collections
Grade = collections.namedtuple("Grade", ("score", "weight"))
# Grade라는 하나의 자료형을 만들수 있게 되는 것

# namedtuple의 한계
# 1. 기본값을 지정할 수 없다
# 2. unintentional usage that makes it harder to move to a real class later.
```

+ write a class

```python
import collections
Grade = collections.namedtuple("Grade", ("score", "weight"))

class Subject(object):
  def __init__(self):
    self._grades = list()
  def report_grade(self, score, weight):
    self._grades.append(Grade(score, weight))
  def average_grade(self):
    total, total_weight = 0, 0
    for grade in self._grades:
      total += grade.score * grade.weight
      total_weight += grade.weight
    return total / total_weight
  
  
class Student(object):
  def __init__(self):
    self._subjects = dict()
  def subject(self, name):
    if name not in self._subjects:
      self._subjects[name] = Subjects()
    return self._subjects[name]
  def average_grade(self):
    total, count = 0, 0
    for subject in self._subjects.values():
      total += subject.average._grade()
      count += 1
    return total / count
  
class Gradebook(object):
  def __init__(self):
    self._students = dict()
  def student(self, name):
    if name not in self._studnets:
      self._studnets[name] = Studnet()
    return self._studnets[name]
  
  
book = Gradebook()
albert = book.studnet("Albert Einstein")
math = albert.subject("Math")
math.report_grade(80, 0.10)
print(albert.average_grade())
```



## 23. Accept functions for Simple Interfaces Instead of Classes / \_\_call\_\_ 쓰는 법



+ hooks: many of python's built-in APIs allow you to customize behavior by passing in a function

```python
# example
nmaes = # assume there is a list of names
names.sort(key = lambda x: len(x))
```

+ functions are ideal for hooks because they are easier to describe and simpler to define than classes. 
+ Functions work as hooks becasue Python has first-class functions: Functions and methods can be passed around and referenced like any other value in the language.

```python
# example
def log_missing():
  print("Key added")
  return 0

current = {"green" : 12,
           "blue" : 3}
increments = [("red", 5),
             ("blue", 5),
             ("orange", 9)]
result = defaultdict(log_missing, current)

for key, amount in increments:
  result[key] += amount
  
# 잘 이해가 되지 않으니 46 장을 기대해 보자 --> 설명을 보니 이해가 됨
defaultdict() 에 들어가는 첫번째 argument는 해당 키값이 없을 때 디폴트로 지정되는 값을 반환하는 함수.
```



```python
def increment_with_report(current, increments):
  added_cnt = 0
  
  def missing():
    nonlocal added_count # statefule closure
    added_count += 1
    return 0
  result = defaultdict(missing, current)
  for key, amount in increments:
    result[key] += amount
  
  return result, added_count

# stateful means the computer or program keeps track of the state of interaction, usually by setting values in a storage field designated for that purpose. 

# stateless means there is no record of previous interactions and each interaction request has to be handled based entirely on information that comes with it. 
```



+ problem with defining a closure for stateful hooks is that it's harder to read than the stateless function example

```python
# solution
class CountMissing(object):
  def __init__(self):
    self.added = 0
  def missing(self):
    self.added += 1
    return 0
  
counter = CountMissing()
result = defaultdict(counter.missing, current)

for key,amount in increments:
  result[key] += amount
```



+ still, it's not obvious what the purpose of the CountMossing class. 

```python
# solution: use __call__ special method
# __call__ allows an object to be called just like a function. 
# __call__ causes the callable built-in function to return True for such an instance

class BetterCountMissing(object):
  def __init__(self):
    self.added = 0
    
  def __call__(self):
    self.added += 1
    return 0

counter = BetterCountMissing()
counter()
assert callable(counter)

result = defaultdict(counter, current) # relies on __call__
for key, amount in increments:
  result[key] +=  amount
assert counter.added == 2

# __call__ indicates that a class's instances will be used somewhere a function argument would also be suitable.
```





## 24. Use @classmethod Polymorphism to Construct Objects Generically

>polymorphism: a feature of a programming language that allows routines to use variables of different types at different times.
>
>polymorphism is a way for multiple classes in a hierarchy to implement their own unique versions of a method



```python
# you're writing a mapreduce implementation and you want a common class to represent the input data. 

class InputData(object):
  def read(self):
    raise NotImplementedError
    
class PathInputData(InputData):
  def __init__(self, path):
    super().__init__()
    self.path = path
  def read(self):
    return open(self.path).read()
  
 
# apply the logic above to mapreduce
class Worker(object):
  def __init__(self, input_data):
    self.input_data = input_data
    self.result = None
  def map(self):
    raise NotImplementedError
  def reduce(self, other):
    raise NotImplementedError
    
class LineCountWorker(Worker):
  def map(self):
    data = self.input_data.read()
    self.result = data.count("\n")
  def reduce(self, other):
    self.result += other.result # other.result가 어디에 정의되어 있는가? other 역시 LineCountWorker의 인스턴스인가? oo
```



```python
# to connect all of these pieces: manually build and connect the objects with some helper functions.

def generate_inputs(data_dir):
  for name in os.listdir(data_dir):
    yield PathInputData(os.path.join(data_dir, name))
    
def create_workers(input_list):
  workers = list()
  for input_data in input_list:
    workers.append(LineCountWorker(input_data))
  return workers

# execute these Worker instances by fanning out the map step to multiple thread (item37)

def execute(workers):
  threads = [Thread(target = w.map) for w in workers]
  for thread in threads: thread.start()
  for thread in threads: thread.join()
  
  first, rest = workers[0], workers[1:]
  for worker in rest:
    first.reduce(worker)
  return first.result

def mapreduce(data_dir):
  inputs = generate_inputs(data_dir)
  workers = create_workers(inputs)
  return execute(workers)
```



+ 위의 방법은 generic 하지 않다. InputData 나 Worker가 바뀌게 되면 그에 맞게 generate_inputs, create_workers, mapreduce도 다 바뀌어야 한다. 
+ 다른 언어에서는 consturctor polymorphism을 통해 문제를 해결한다.  하지만 파이썬의 생성자는 \_\_init\_\_ 하나 뿐이다.
+ @classmethod polymorphism을 사용하자. 

```python
class GenericInputData(object):
  def read(self):
    raise NotImplementedError
  @classmethod
  def generate_inputs(cls, config):
    raise NotImplemetedError

# here i use the config to fine the directory to list for input files

class PathInputData(GenericInputData):
  def read(self):
    return open(self.path).read()
  @classmethod
  def generate_inputs(cls, config):
    data_dir = config['data_dir']
    for name in os.listdir(data_dir):
      yield cls(os.path.join(data_dir, name))
```



## 25. Initialize Parent Classes with Super

+ old way

```python
class MyBaseClass(object):
  def __init__(self, value):
    self.value = value
  
class MyChildClass(MyBaseClass):
  def __init__(self):
    MyBaseClass.__init__(self, 5)
    
# this is fine for simple hierarchies
```



+ If your class is affected by multiple inheritance, calling the superclasses' \_\_init\__ methods directly can lead to unpredictable behavior
+ One problem is that \_\_init__ call order isn't specified across all subclasses.

```python
class TimesTwo(object):
  def __init__(self):
    self.value *= 2
    
class PlusFive(object):
  def __init__(self):
    self.value +=2
       
## 문제가 되는 상황
class OneWay(MyBaseClass, TimesTwo, PlusFive):
  def __init__(self, value):
    MyBaseClass.__init__(self, value)
    TimesTwo.__init__(self)
    PlusFive.__init__(self)
    
class AnotherWay(MyBaseClass, PlusFive, TimesTwo):
  def __init__(self, value):
    MyBaseClass.__init__(self, value)
    TimesTwo.__init__(self)
    PlusFive.__init__(self)
    
# 상속 클래스의 순서가 바뀌었지만 생성 함수 안의 순서가 안바뀌면 동일하다
```



+ diamond inheritance

  + when a subclass inherits from two separate classes that have the same superclass somewhere in the hierarchy.
  + diamond inheritance causes the common superclass's \_\_init__ method to run multiple imtes, causing unexpected behavior

  ```python
  # define two classes that inherit from MyBaseClass
  class TimesFive(MyBaseClass):
    def __init__(self, value):
      MyBaseClass.__init__(self, value)
      self.value *= 5
      
  class PlusTwo(MyBaseClass):
    def __init__(self, value):
      MyBaseClass.__init__(self, value)
      self.value += 2
      
  # define a child class that inherits from both of these classes
  
  class Thisway(TimesFive, PlusTwo):
    def __init__(self, value):
      TimesFive.__init__(self, value)
      PlusTwo.__init__(self, value)
      
  foo = ThisWay(5)
  # this should be 27 but it's 7
  # the second constructor reset self.value to 5 when MyBaseClass.__init__ gets called a second time.
  ```

+ super built-in function

  + MRO: method resolution order
    + it standardizes which superclasses are initialized before others.





