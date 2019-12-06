# Logging Basic





## 5 levels of logging

1. DEBUG
2. INFO
3. WARNING
4. ERROR
5. CRITICAL



log record attributes



### Using Logger

```python
import logging

logger = logging.getLogger(__name__)
logger.setLevel(logging.INFO)

formatter = logging.Formatter('%(asctime)s:%(levelname)s:%(name)s:%(message)s')

file_handler = logging.FileHandler('sample.log')
file_handler.setFormatter(formatter)

logger.addHandler(file_handler)

# logging.basicConfig(filename="sample.log", level=logging.DEBUG,
#                     format='%(asctime)s:%(levelname)s:%(name)s:%(message)s')

def add(x, y):
    return x + y

num_1 = 10
num_2 = 30

add_result = add(num_1, num_2)
logger.info("add: %s + %s = %s" % (num_1, num_2, add_result))
```

