# Decorators for Functions and Methods

[source](such file or directory: '/tmp//data/private/translate/Wiki_DATA/kr/*.paragraph_info'
evaluation completed)

## Abstract

+ transforming functions and methods is awkward and can lead to code that is difficult to understand. 
+ the transformation should be made at the same point in the code where the declaration itself is made



## First-class function

+ 함수 자체를 인자(argument)로서 다른 함수에 전달하거나 다른 함수의 결과값으로 리턴할 수도 있고, 함수를 변수에 할당하거나 데이터구조안에 저장할 수 있는 함수를 뜻함

## Closure (클로저)

+ techniques for implementing lexically scoped name binding in languages with first-class functions. 

+ a closure is a record stroing a function together with an environment. 

+ a closure is a mapping associating each free variable of the function with the value or reference to which the name was bound when the closure was created.

  ```python
  def outer_func():
    message = "Hi"
    def inner_func():
      print(message)
    return inner_func()
  
  outer_func()
  ```

  
