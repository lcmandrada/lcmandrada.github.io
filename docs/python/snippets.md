# Snippets

## Decorators

### Retry
Retry execution

```py
def retry(ExceptionToCheck, tries=4, delay=3, backoff=2, logger=None):
    """Retry calling the decorated function using an exponential backoff.

    Args:
        ExceptionToCheck (Exception or tuple): the exception to check
            may be a tuple of exceptions to check
        tries (int): number of times to try (not retry) before giving up
        delay (int): initial delay between retries in seconds
        backoff (int): backoff multiplier
            e.g. value of 2 will double the delay each retry
        logger (logging.Logger): logger to use. If None, print
    
    Returns:
        function
    """

    def deco_retry(f):
        @wraps(f)
        def f_retry(*args, **kwargs):
            mtries, mdelay = tries, delay

            while mtries > 1:
                try:
                    return f(*args, **kwargs)
                except ExceptionToCheck:
                    msg = f'str(e), Retrying in {delay} seconds...'

                    if logger:
                        logger.warning(msg)
                    else:
                        print(msg)

                    time.sleep(mdelay)
                    mtries -= 1
                    mdelay *= backoff

            return f(*args, **kwargs)
        return f_retry
    return deco_retry
```

### Runtime
Print runtime duration

```py
import functools
import time

def timer(func):
    """Print the runtime of the decorated function"""
    @functools.wraps(func)
    def wrapper_timer(*args, **kwargs):
        start_time = time.perf_counter()

        value = func(*args, **kwargs)

        end_time = time.perf_counter()
        run_time = end_time - start_time

        print(f"Finished {func.__name__!r} in {run_time:.4f} secs")
        return value
    return wrapper_timer

@timer
def waste_some_time(num_times):
    for _ in range(num_times):
        sum([i**2 for i in range(10000)])
```

### Debug Log
Print debug logs

```py
import functools

def debug(func):
    """Print the function signature and return value"""
    @functools.wraps(func)
    def wrapper_debug(*args, **kwargs):
        args_repr = [repr(a) for a in args]                      
        kwargs_repr = [f"{k}={v!r}" for k, v in kwargs.items()]  

        signature = ", ".join(args_repr + kwargs_repr)           
        print(f"Calling {func.__name__}({signature})")

        value = func(*args, **kwargs)
        print(f"{func.__name__!r} returned {value!r}")           

        return value
    return wrapper_debug
```

### Registry
Register to a context

```py
import random
PLUGINS = dict()

def register(func):
    """Register a function as a plug-in"""
    PLUGINS[func.__name__] = func
    return func

@register
def say_hello(name):
    return f"Hello {name}"

@register
def be_awesome(name):
    return f"Yo {name}, together we are the awesomest!"

def randomly_greet(name):
    greeter, greeter_func = random.choice(list(PLUGINS.items()))
    print(f"Using {greeter!r}")
    return greeter_func(name)
```

### Login Required
Check if user is logged in

```py
from flask import Flask, g, request, redirect, url_for
import functools

app = Flask(__name__)

def login_required(func):
    """Make sure user is logged in before proceeding"""
    @functools.wraps(func)
    def wrapper_login_required(*args, **kwargs):
        if g.user is None:
            return redirect(url_for("login", next=request.url))
        return func(*args, **kwargs)
    return wrapper_login_required

@app.route("/secret")
@login_required
def secret():
    ...
```

### Singleton Class
Implement Singleton

```py
import functools

def singleton(cls):
    """Make a class a Singleton class (only one instance)"""
    @functools.wraps(cls)
    def wrapper_singleton(*args, **kwargs):
        if not wrapper_singleton.instance:
            wrapper_singleton.instance = cls(*args, **kwargs)
        return wrapper_singleton.instance

    wrapper_singleton.instance = None
    return wrapper_singleton

@singleton
class TheOne:
    pass
```

### Route JSON Validator
Validate if given arguments are available in JSON payload

```py
from flask import Flask, request, abort
import functools
app = Flask(__name__)

def validate_json(*expected_args):
    def decorator_validate_json(func):
        @functools.wraps(func)
        def wrapper_validate_json(*args, **kwargs):
            json_object = request.get_json()

            for expected_arg in expected_args:
                if expected_arg not in json_object:
                    abort(400)

            return func(*args, **kwargs)
        return wrapper_validate_json
    return decorator_validate_json

@app.route("/grade", methods=["POST"])
@validate_json("student_id")
def update_grade():
    json_data = request.get_json()
    # Update database.
    return "success!"
```

## Dispatch
Dispatch a function upon certain criteria

```py
from functools import partial

def dirty_strip(value: str, to_remove: str, split_by: str) -> list:
    return value.strip(to_remove).split(split_by)

split_square = partial(dirty_strip, to_remove='[]', split_by=',')
split_circle = partial(dirty_strip, to_remove='()', split_by=',')

dispatch = {
    'app_id': lambda x: x,
    'category': split_square,
    'content_rating': split_circle,
    ...
}

data = [{ ... }]
processed_data = []

for item in data:
    converted_data = {}

    for key, val in item.items():
        converted_data[key] = dispatch[key](val)

    processed_data.append(converted_data)
```
