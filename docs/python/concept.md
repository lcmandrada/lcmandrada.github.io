# Concept

## Data Types
```py
var = 7         # integer
var = 3.14      # float

var = True      # boolean
var = False

var = "hi"      # string
```

## Comment
```py
# single line

"""multi-line

Lorem ipsum dolor sit amet, consectetur adipiscing elit.
Aenean efficitur tristique neque in facilisis. Donec commodo varius sem,
eu pellentesque tortor congue varius. Nullam et felis convallis,
ornare ipsum vitae, tincidunt mi.

Duis eu laoreet leo.
    Vestibulum ante ipsum primis in faucibus orci luctus
        et ultrices posuere cubilia curae;
    Maecenas id sapien et arcu interdum dictum.
"""
```

## Operators

### Math
```py
+               # add
-               # subtract
*               # multiply
/               # divide
**              # exponent
%               # modulo
```

### Comparison
```py
==              # equal to
!=              # not equal
<               # less than
<=              # less than or equal
>               # greater than
>=              # greater than or equal
```

### Logical
```py
and
or
not
```

### Bitwise
```py
>>              # shift right
<<              # shift left

&               # bitwise and
|               # bitwise or
^               # bitwise xor
~               # bitwise not

0b              # binary format

bin()           # return binary
oct()           # return octal
hex()           # return hexadecimal
```

### Walrus
Assign variables in the middle of expressions  
Helps avoid repetitive code

```py
if (var := x % 3) != 0:
    print(var)
```

## String
```py
# escape character
"\n"

var = "test"

# access by index
var[index]
var[start:end:step]

# return length
len(var)

# methods
str.lower()                     # return lowercase
str.upper()                     # return uppercase
str.isalpha()                   # return True if letters only

str.startswith(substr)          # does str start with substr?
str.endswith(substr)            # does str end with substr?
str.removeprefix(substr)        # remove prefix
str.removesuffix(substr)        # remove suffix

# cast
str(non_str)

# concatenation
var = "str1" + "str2"

# formatting
"Hi, {}. You are {}.".format(name, age)     # format
f"Hi, {name}. You are {age}."               # f-strings, faster

# input
var = input(prompt_str)
```

## Datetime
```py
from datetime import datetime

var = datetime.now()        # current date and time
var = datetime.utcnow()     # current date and time in UTC
var.year                    # year
var.month                   # month
var.day                     # day
var.hour                    # hour
var.minute                  # minute
var.second                  # second
```

## Control Flow

### If
```py
if condition:
    ...
elif condition:
    ...
else:
    ...
```

## Function
```py
# definition
def function(parameters):
    ...

# execution
function(parameters)
```

### Lambda
Anonymous function
```py
# syntax
lambda x: expression

# example
filter(lambda x: expression, data)
```

## Modules
```py
# generic import
import module
module.function()

# function import
from module import function

# universal import
from module import *

# print functions and variables
var = dir(module)
print(var)
```

## Built-in Methods
```py
# iterables
max(iterable)
min(iterable)
sum(iterable)

# absolute value
abs(number)

# data type
type(data)

# cast
int(data, base)
float(data)

# generator to count from start to end at step interval
range(start, end, step)
```

## List
```py
# syntax
list1 = [item1, item2]

# access
list1[index] = item
list1[start:stop:step]

# add item
list1.append(item)

# concatenate
list3 = list1 + list2

# list length
len(list1)

# methods
list1.index(item)                # return index where item is
list1.insert(index, item)        # insert item at index
list1.sort()                     # sort list
list1.remove(item)               # delete first occurrence of item
list1.pop(index)                 # delete and return item at index

# delete item at index
del(list1[index])

# populate a list
list1 = [items] * multiplier

# join a list into a string separated by the delimiter
delimiter.join(list)

# condition where item x is not in list y
if x not in y:
    ...

# list comprehension
list1 = [expression for i in iterable if condition]
```

## Dictionary
```py
# syntax
dict = {key: value}
dict[key] = value

# delete a key-value pair
del dict[key]
dict.pop(key, None)

dict.items()            # iterable of key-value pairs
dict.keys()             # iterable of keys
dict.values()           # iterable of values

# update with another dictionary
dict1.update(dict2)

# combine dictionaries
dict3 = dict1 | dict2
dict1 |= dict2

# dictionary comprehension
dict = {key: value for i in iterable if condition}
```

## Loops

### For
```py
for var in iterable:
    ...
# only executed if the loop isn't terminated by a break or return
else:
    ...

# create a range
for num in range(start, end, step):
    ...

# unpacking
for var1, var2 in zip(list1, list2):
    ... 

# add index
for index, item in enumerate(iterable):
    ...
```

### While
```py
while condition:
    ...
# only executed if the loop isn't terminated by a break or return
else:
    ...
```

### Statements
```py
break       # terminates a loop
continue    # skips an iteration
```

## Class
```py
class ClassName(BaseClass):
    def __init__(self, parameters):
        super().__init__(parameters)
        super().method()

        self.attribute = value

instance = ClassName(parameters)
```

## File
```py
# open
f = open("file", "mode")

# methods
f.write("str")
f.read()
f.readline()
f.close()
f.closed

# context manager
with open("file", "mode") as f:
    f.method()
```

## * and **
```py
# unpacks lists and tuples
list3 = [7, *list1, *list2]

# unpacks dictionaries
dict3 = {
    "key": "value",
    **dict1,
    **dict2
}

# args and kwargs parameters
def test(var, *args, **kwargs):
    ...
    # args is a tuple containing the rest of the positional arguments 
    # kwargs is a dictionary containing the rest of the keyword arguments
```

## Try-Catch
```py
try:
    ...
except (TypeError, ValueError) as e:
    ...
except ZeroDivisionError:
    ...
```

## Custom Exception
```py
class AppError(Exception):
    pass

raise AppError("error")
```

## Dynamic Object Getter/Setter
```py
var = getattr(object, property)
setattr(object, property, value)
```

## Decorator

### Basic
```py
import functools

def test(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        ...
        value = func(*args, **kwargs)
        ...
        return value
    return wrapper

@test
def check(...):
    ...
```

### Arguments
```py
def test(arg):
    def decorator_test(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
                ...
            value = func(*args, **kwargs)
            ...
            return value
        return wrapper
    return decorator_test

@test(arg)
def v(...):
    ...
```

### Flexible
```py
# decorator with or without arguments
# * forces the right side parameters to be keyword parameters only
# if no arguments were passed, the function is directly passed to `test`
# this is why `_func` is necessary to catch the decorated function
def test(_func=None, *, kwarg=7):
    def decorator_test(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            ...
            value = func(*args, **kwargs)
            ...
            return value
        return wrapper

    if _func is None:
        return decorator_test
    else:
        return decorator_test(_func)
```

### Stateful
```py
import functools

def count_calls(func):
    @functools.wraps(func)
    def wrapper_count_calls(*args, **kwargs):
        wrapper_count_calls.num_calls += 1
        print(f"Call {wrapper_count_calls.num_calls} of {func.__name__!r}")
        return func(*args, **kwargs)
    wrapper_count_calls.num_calls = 0
    return wrapper_count_calls

@count_calls
def say_whee():
    print("Whee!")
```

### Class
```py
import functools

class CountCalls:
    def __init__(self, func):
        functools.update_wrapper(self, func)
        self.func = func
        self.num_calls = 0

    def __call__(self, *args, **kwargs):
        self.num_calls += 1
        print(f"Call {self.num_calls} of {self.func.__name__!r}")
        return self.func(*args, **kwargs)

@CountCalls
def say_whee():
    print("Whee!")
```

### Class Arguments
```py
class CountCalls:
    def __init__(self, *args, **kwargs):
        self.num_calls = 0
        self.args = args
        self.kwargs = kwargs

    def __call__(self, func):
        functools.update_wrapper(self, func)
        self.func = func
        def wrapper(*args, **kwargs):
            self.num_calls += 1
            wrapper.num_calls = self.num_calls
            print(f"Call {self.num_calls} of {self.func.__name__!r}")
            print(self.args, self.kwargs)
            return self.func(*args, **kwargs)
        return wrapper

@CountCalls("test")
def say_whee(whee="Whee!"):
    print(whee)
```

## Modules

### defaultdict
```py
from collections import defaultdict

# returns 0 if key is not present
var = defaultdict(int)
```

### deepcopy
```py
from copy import deepcopy

# copies deeply as opposed to shallow
var2 = deepcopy(var1)
```

### breakpoint
```py
breakpoint()
```

### traceback
```py
import traceback

print(traceback.format_exc())
```

### patch

#### Environment Variable
```py
import os
from unittest.mock import patch

@patch.dict(os.environ, {"RISK_SERVICE_URL": test_url})
```

### `aiohttp` post
See [parametrize](#parametrize)

### `pytest`

#### <a name="parametrize"></a>Parametrize
```py
import pytest

@pytest.mark.parametrize(
    "status", [status.HTTP_200_OK, status.HTTP_226_IM_USED]
)
@patch("gateway.utils.aiohttp.ClientSession.post")
@pytest.mark.asyncio
async def test_async_post(mock_post, status):
    request_data = {"test": "test"}
    response_json = {"status": "ok"}
    response = create_async_response(
        url=test_url, status=status, content=json.dumps(response_json).encode()
    )
    mock_post.return_value.__aenter__.return_value = response
    result = await async_post(url=test_url, data=request_data)

    mock_post.assert_called_with(
        test_url, headers=None, params=None, json=request_data, timeout=100
    )
    assert result == response_json
```

### `async`

#### Get response body
```py
import asyncio

def async_return(result: Any) -> asyncio.Future:
    """Converts result to an awaitable result."""
    async_result = asyncio.Future()
    async_result.set_result(result)
    return async_result
```

### `aiohttp`

#### Get response body
```py
from aiohttp import ClientResponse

async def get_async_response_body(
    response: ClientResponse
) -> Union[Dict[str, Any], str]:
    """Returns response body from aiohttp request."""
    if "application/json" in response.headers.get("Content-Type"):
        body = await response.json()
    else:
        body = await response.text()
    return body
```

#### Mock response
```py
from unittest.mock import Mock

from aiohttp import ClientResponse
from aiohttp.helpers import TimerNoop
from fastapi import status
from yarl import URL

def create_async_response(
    url: str = "https://test.savii.io",
    method: str = "GET",
    status: int = status.HTTP_200_OK,
    content: bytes = b'{"status": "ok"}',
    headers: Optional[Dict[str, Any]] = None,
) -> ClientResponse:
    response = ClientResponse(
        method,
        URL(url),
        request_info=Mock(),
        writer=Mock(),
        continue100=None,
        timer=TimerNoop(),
        traces=[],
        loop=Mock(),
        session=Mock(),
    )
    response.status = status
    response._body = content
    response._headers = headers or {"Content-Type": "application/json"}
    return response
```



## References

[PEP 8](https://peps.python.org/pep-0008/)  
[Google Python Style Guide](https://google.github.io/styleguide/pyguide.html)  
[Could not import lzma module](https://stackoverflow.com/questions/57743230/userwarning-could-not-import-the-lzma-module-your-installed-python-is-incomple)  
