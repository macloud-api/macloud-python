# MacCloud API SDK for Python

A Python SDK for interacting with the MacCloud API. This SDK provides a simple and secure way to make authenticated requests to MacCloud RESTful APIs with automatic request signing.

## Features

- 🔐 **Secure Authentication**: HMAC-SHA256 signature algorithm for secure API requests
- 🚀 **Full HTTP Method Support**: GET, POST, PUT, PATCH, DELETE
- 📦 **Easy to Use**: Simple and intuitive API
- 🛡️ **Request Signing**: Automatic request signing to prevent data tampering
- ⚙️ **Configurable**: Customizable timeout, logging, and proxy settings
- 🐍 **Python 3.5+**: Compatible with Python 3.5 and above

## Installation

Install the package using pip:

```bash
pip install macloudapisdk
```

## Quick Start

### Basic Usage

```python
import os
import logging
from macloudapisdk import Sdk

# Configure logging (optional)
logger = logging.getLogger()
formatter = logging.Formatter('%(asctime)s %(filename)s[line:%(lineno)d] %(levelname)s %(message)s')

# Log to file
fileHandle = logging.FileHandler('/tmp/sdk.log', encoding='utf-8')
fileHandle.setFormatter(formatter)
logger.addHandler(fileHandle)

# Log to stdout
streamHandle = logging.StreamHandler()
streamHandle.setFormatter(formatter)
logger.addHandler(streamHandle)

# Initialize SDK
sdk = Sdk({
    "app_id": os.environ['SDK_APP_ID'],
    "app_secert": os.environ['SDK_APP_SECERT'],
    "api_pre": os.environ['SDK_API_PRE'],  # e.g., "https://api.local.com/V4/"
    "timeout": 30,
    "logger": logger,  # Optional
})

# Make a GET request
raw, jsonData, err = sdk.get('test.sdk.get', query={'page': 1, 'pagesize': 10})
print("Response:", jsonData)
print("Error:", err)
```

## Configuration

### SDK Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `app_id` | string | Yes | Your application ID (contact technical support to obtain) |
| `app_secert` | string | Yes | Your application secret for request signing |
| `api_pre` | string | Yes | API base URL prefix (e.g., `https://api.local.com/V4/`) |
| `timeout` | int | No | Request timeout in seconds (default: 30) |
| `user_id` | int | No | Current user ID |
| `logger` | object | No | Python logger object for logging requests/responses |
| `host` | string | No | Custom Host header |
| `proxy_url` | string | No | Proxy URL (e.g., `1.1.1.1:80`) |

## API Methods

### GET Request

```python
api = 'test.sdk.get'
query = {
    "page": 1,
    "pagesize": 10,
    "data": {
        "name": "example",
        "domain": "example.com",
    }
}
raw, jsonData, err = sdk.get(api, query=query)
```

### POST Request

```python
api = 'test.sdk.post'
query = {}
postData = {
    "name": "example",
    "age": 10,
    "data": {
        "name": "example",
        "domain": "example.com",
    }
}
raw, jsonData, err = sdk.post(api, postData=postData, query=query)
```

### PUT Request

```python
api = 'test.sdk.put'
query = {}
postData = {
    "name": "example",
    "age": 10,
}
raw, jsonData, err = sdk.put(api, postData=postData, query=query)
```

### PATCH Request

```python
api = 'test.sdk.patch'
query = {}
postData = {
    "name": "example",
    "age": 10,
}
raw, jsonData, err = sdk.patch(api, postData=postData, query=query)
```

### DELETE Request

```python
api = 'test.sdk.delete'
query = {}
postData = {
    "name": "example",
    "age": 10,
}
raw, jsonData, err = sdk.delete(api, postData=postData, query=query)
```

## Response Format

All SDK methods return a tuple of three values:

```python
raw, jsonData, err = sdk.get('api.endpoint')
```

- `raw` (string): Raw response body as string
- `jsonData` (dict): Parsed JSON response as dictionary (empty dict if parsing fails)
- `err` (string): Error message (empty string if no error)

## Important Notes

### Query Parameters

For all requests, URI and query parameters are separated. For example, if the full URL is `https://api.local.com/V4/version?v=1`, you should pass `v=1` through the `query` parameter:

```python
raw, body, err = sdk.get('version', query={'v': 1})
```

### API Base URL

The API base URL should be provided in the `api_pre` parameter. Contact your operations team for the specific base URL (e.g., `https://api.local.com/V4/`).

### Authentication

- Contact technical support to register an account and obtain `app_id` and `app_secert`
- The SDK automatically signs all requests using HMAC-SHA256 algorithm
- Request signing ensures data integrity during transmission

## Signature Algorithm

The SDK uses HMAC-SHA256 for request signing:

1. **Client Side**: Parameters are base64 encoded and signed with `app_secret` using SHA256
2. **Server Side**: Server verifies the signature using the same algorithm

This ensures that data cannot be tampered with during transmission.

## Requirements

- Python >= 3.5
- requests library

## Examples

### Complete Example

```python
import os
import logging
from macloudapisdk import Sdk

# Setup logging
logger = logging.getLogger()
formatter = logging.Formatter('%(asctime)s %(filename)s[line:%(lineno)d] %(levelname)s %(message)s')

fileHandle = logging.FileHandler('/tmp/sdk.log', encoding='utf-8')
fileHandle.setFormatter(formatter)
logger.addHandler(fileHandle)

streamHandle = logging.StreamHandler()
streamHandle.setFormatter(formatter)
logger.addHandler(streamHandle)

# Initialize SDK
sdk = Sdk({
    "app_id": os.environ['SDK_APP_ID'],
    "app_secert": os.environ['SDK_APP_SECERT'],
    "api_pre": os.environ['SDK_API_PRE'],
    "timeout": 30,
    "logger": logger,
})

# GET request
raw, jsonData, err = sdk.get('test.sdk.get', query={'page': 1})
print("GET Response:", jsonData)

# POST request
raw, jsonData, err = sdk.post('test.sdk.post', postData={'name': 'test'})
print("POST Response:", jsonData)
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

For issues, questions, or contributions, please contact:
- Email: macloudapisdk@outlook.com
- GitHub: [macloud-api/macloud-python](https://github.com/macloud-api/macloud-python)

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
