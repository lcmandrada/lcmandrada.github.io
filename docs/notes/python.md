# Python

## Built-in

### Breakpoint
```py
breakpoint()
```

### Traceback
```py
import traceback

print(traceback.format_exc())
```



## Patch

### Environment Variable
```py
import os
from unittest.mock import patch

@patch.dict(os.environ, {"RISK_SERVICE_URL": test_url})
```

### `aiohttp` post
See [parametrize](#parametrize)



## `pytest`

### <a name="parametrize"></a>Parametrize
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



## `async`

### Get response body
```py
import asyncio

def async_return(result: Any) -> asyncio.Future:
    """Converts result to an awaitable result."""
    async_result = asyncio.Future()
    async_result.set_result(result)
    return async_result
```



## `aiohttp`

### Get response body
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

### Mock response
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

[Could not import lzma module](https://stackoverflow.com/questions/57743230/userwarning-could-not-import-the-lzma-module-your-installed-python-is-incomple)