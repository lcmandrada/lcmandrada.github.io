# Requests

## Request
```py
response = requests.get(
    'https://api.github.com',
    params={'q': 'requests+language:python'}, # query string; dict or list of tuples or bytes
    headers={'Accept': 'application/vnd.github.v3.text-match+json'}, # request headers
    timeout=1 # in seconds; or in tuple, e.g. timeout=(connect_timeout, read_timeout)
)

# data can be a dictionary, a list of tuples, bytes, or a file-like object
# `data` parameter is for application/x-www-form-urlencoded
# use `json` parameter for json payload
requests.post('https://httpbin.org/post', data={'key':'value'})
requests.put('https://httpbin.org/put', data={'key':'value'})
requests.delete('https://httpbin.org/delete')
requests.patch('https://httpbin.org/patch', data={'key':'value'})
requests.head('https://httpbin.org/get')
requests.options('https://httpbin.org/get')
```

## Status Code 
```py
response.status_code

# raise error without checking status code
from requests.exceptions import HTTPError
try:
    response = requests.get('https://api.github.com')
    response.raise_for_status()
except HTTPError as http_error:
    ...
except Exception as error:
    ...
else:
    ...
```

## Response
```py
response.content                    # payload in btyes
response.encoding = 'utf-8'         # setting an encoding when calling text property
response.text                       # payload in string
response.json()                     # parsed json
response.headers['Content-Type']    # dict; keys are case-insensitive

# accessing original request details
response.request.headers['Content-Type']
response.request.url
response.request.body
```

## Basic Auth
```py
# Authorization: Basic <username:password in base64>
requests.get('https://api.github.com/user', auth=('username', 'password'))

# or

from requests.auth import HTTPBasicAuth
requests.get(
    'https://api.github.com/user',
    auth=HTTPBasicAuth('username', 'password')
)
```

## Timeout
```py
from requests.exceptions import Timeout
try:
    response = requests.get('https://api.github.com', timeout=1)
except Timeout:
    ...
else:
    ...
```

## Session
Better performance due to persistence  
Will reuse a connection when possible

```py
# By using a context manager, you can ensure the resources used by
# the session will be released after use
with requests.Session() as session:
    session.auth = ('username', getpass())
    # Instead of requests.get(), you'll use session.get()
    response = session.get('https://api.github.com/user')

from requests.adapters import HTTPAdapter
from requests.exceptions import ConnectionError

# max retries
github_adapter = HTTPAdapter(max_retries=3)
session = requests.Session()

# Use `github_adapter` for all requests to endpoints that start with this URL
session.mount('https://api.github.com', github_adapter)

try:
    session.get('https://api.github.com')
except ConnectionError as ce:
    print(ce)
```
