# 🛑 错误码参考

本页面列出了 Pan123 库可能遇到的错误码及其含义。

## 通用错误码

以下是在123云盘开放平台API中常见的错误码：

| 错误码 | 错误名称 | 含义 | 解决方案 |
|--------|----------|------|----------|
| 0 | Success | 操作成功 | - |
| 400 | BadRequest | 请求参数错误 | 检查传入的参数格式和值 |
| 401 | Unauthorized | 认证失败 | 检查access_token是否有效 |
| 403 | Forbidden | 权限不足 | 检查当前账户权限 |
| 404 | NotFound | 资源不存在 | 检查文件ID、分享ID等是否正确 |
| 429 | TooManyRequests | 请求过于频繁 | 降低请求频率 |
| 500 | InternalServerError | 服务器内部错误 | 稍后重试或联系技术支持 |
| 502 | BadGateway | 网关错误 | 稍后重试 |
| 503 | ServiceUnavailable | 服务暂时不可用 | 稍后重试 |

## 业务错误码

### 文件相关错误

| 错误码 | 错误名称 | 含义 | 解决方案 |
|--------|----------|------|----------|
| 1001 | FileNotFound | 文件不存在 | 检查文件ID是否正确 |
| 1002 | FileAlreadyExists | 文件已存在 | 修改文件名或选择覆盖模式 |
| 1003 | InsufficientStorage | 存储空间不足 | 清理空间或升级账户 |
| 1004 | FileTooLarge | 文件过大 | 检查文件大小限制 |
| 1005 | FileTypeNotAllowed | 文件类型不允许 | 检查文件类型是否被支持 |
| 1006 | FileAccessDenied | 文件访问被拒绝 | 检查文件权限 |

### 分享相关错误

| 错误码 | 错误名称 | 含义 | 解决方案 |
|--------|----------|------|----------|
| 2001 | ShareNotFound | 分享不存在 | 检查分享ID是否正确 |
| 2002 | ShareExpired | 分享已过期 | 重新创建分享链接 |
| 2003 | ShareQuotaExceeded | 分享配额超出 | 检查分享限制 |
| 2004 | SharePasswordIncorrect | 分享密码错误 | 提供正确的密码 |

### 用户相关错误

| 错误码 | 错误名称 | 含义 | 解决方案 |
|--------|----------|------|----------|
| 3001 | UserNotLoggedIn | 用户未登录 | 重新认证获取access_token |
| 3002 | UserAccountDisabled | 账户已被禁用 | 联系客服解决问题 |
| 3003 | UserQuotaExceeded | 用户配额超出 | 升级账户或清理空间 |

### 转码相关错误

| 错误码 | 错误名称 | 含义 | 解决方案 |
|--------|----------|------|----------|
| 4001 | TranscodeNotSupported | 不支持的转码格式 | 检查视频格式是否支持 |
| 4002 | TranscodeInProgress | 转码正在进行中 | 等待转码完成 |
| 4003 | TranscodeFailed | 转码失败 | 检查视频文件完整性 |

## Pan123 库异常

### ClientKeyError

**含义：** 客户端认证密钥错误

**可能原因：**
- client_id 或 client_secret 错误
- 客户端没有权限访问API

**解决方案：**
```python
# 检查client_id和client_secret是否正确
try:
    access_token = get_access_token("your_client_id", "your_client_secret")
except ClientKeyError as e:
    print(f"客户端认证失败: {e}")
    # 检查并更正client_id和client_secret
```

### AccessTokenError

**含义：** 访问令牌错误或过期

**可能原因：**
- access_token 过期
- access_token 无效
- 令牌权限不足

**解决方案：**
```python
from pan123 import Pan123
from pan123.auth import get_access_token

# 尝试重新获取access_token
try:
    pan = Pan123("your_token")
    result = pan.user.info()
except AccessTokenError:
    # 重新获取token
    new_token = get_access_token("client_id", "client_secret")
    pan = Pan123(new_token)
    result = pan.user.info()
```

### PacketLossError

**含义：** 文件上传过程中出现数据包丢失

**可能原因：**
- 网络连接不稳定
- 上传过程中断

**解决方案：**
```python
from pan123 import Pan123
import time

pan = Pan123("your_token")

# 上传文件，支持重试
for attempt in range(3):
    try:
        result = pan.file.upload(parent_file_id=0, file_path="path/to/file")
        break  # 成功则跳出循环
    except PacketLossError as e:
        print(f"上传失败，正在重试... (第{attempt+1}次)")
        time.sleep(2)  # 等待2秒后重试
else:
    print("上传失败，已达最大重试次数")
```

## 错误处理最佳实践

### 通用错误处理

```python
from pan123 import Pan123
from pan123.utils.exceptions import AccessTokenError, ClientKeyError

def safe_api_call():
    pan = Pan123("your_token")
    
    try:
        # 执行API调用
        result = pan.user.info()
        return result
    except AccessTokenError:
        # 重新获取访问令牌
        print("访问令牌过期，正在重新获取...")
        # 这里可以重新获取token并重试
        return None
    except ClientKeyError:
        print("客户端认证失败，请检查认证信息")
        return None
    except Exception as e:
        print(f"未知错误: {e}")
        return None
```

### 重试机制

```python
import time
from pan123 import Pan123
from pan123.utils.exceptions import AccessTokenError

def retry_api_call(func, max_retries=3, delay=1):
    """
    带重试机制的API调用
    """
    for attempt in range(max_retries):
        try:
            return func()
        except AccessTokenError:
            if attempt == max_retries - 1:
                raise  # 最后一次尝试失败，抛出异常
            print(f"访问令牌错误，正在重试... ({attempt + 1}/{max_retries})")
            time.sleep(delay)
        except Exception as e:
            if "rate limit" in str(e).lower():
                print(f"达到速率限制，等待后重试... ({attempt + 1}/{max_retries})")
                time.sleep(delay * (attempt + 1))  # 递增延迟
            else:
                raise  # 其他错误直接抛出

# 使用示例
def get_user_info():
    pan = Pan123("your_token")
    return pan.user.info()

try:
    user_info = retry_api_call(get_user_info)
    print(user_info)
except Exception as e:
    print(f"API调用最终失败: {e}")
```

## 调试技巧

### 启用详细日志

```python
import logging

# 设置日志级别以获取更多调试信息
logging.basicConfig(level=logging.DEBUG)
```

### 检查响应内容

当遇到错误时，可以检查详细的响应信息：

```python
import requests
from pan123 import Pan123

# 直接使用requests检查详细错误
try:
    pan = Pan123("your_token")
    result = pan.user.info()
except Exception as e:
    print(f"错误类型: {type(e).__name__}")
    print(f"错误信息: {e}")
    # 根据错误类型执行相应处理
```

## 常见问题解答

### Q: "访问令牌过期"错误如何处理？

**A:** 实现令牌自动刷新机制：

```python
import os
from datetime import datetime, timedelta
from pan123 import Pan123
from pan123.auth import get_access_token

class Pan123WithRefresh:
    def __init__(self, client_id, client_secret, token_file="access_token.txt"):
        self.client_id = client_id
        self.client_secret = client_secret
        self.token_file = token_file
        self.pan = None
        self._init_client()
    
    def _init_client(self):
        token = self._get_valid_token()
        self.pan = Pan123(token)
    
    def _get_valid_token(self):
        if os.path.exists(self.token_file):
            with open(self.token_file, "r") as f:
                token = f.read().strip()
            # 这里可以添加检查token是否过期的逻辑
            # 比如检查token获取时间等
            return token
        else:
            # 如果没有存储的token，获取新的
            token = get_access_token(self.client_id, self.client_secret)
            with open(self.token_file, "w") as f:
                f.write(token)
            return token
    
    def call_api(self, api_func, *args, **kwargs):
        try:
            return api_func(*args, **kwargs)
        except AccessTokenError:
            # 令牌过期，重新获取
            new_token = get_access_token(self.client_id, self.client_secret)
            with open(self.token_file, "w") as f:
                f.write(new_token)
            self.pan = Pan123(new_token)
            return api_func(*args, **kwargs)
```

### Q: 如何处理网络连接问题？

**A:** 添加网络异常处理：

```python
import requests
from pan123 import Pan123

def network_safe_call():
    pan = Pan123("your_token")
    
    try:
        result = pan.user.info()
        return result
    except requests.ConnectionError:
        print("网络连接错误，请检查网络连接")
        return None
    except requests.Timeout:
        print("请求超时，请稍后重试")
        return None
    except requests.RequestException as e:
        print(f"请求异常: {e}")
        return None
```