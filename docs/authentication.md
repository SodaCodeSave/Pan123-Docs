# 🔐 认证指南

在使用 Pan123 之前，您需要获取123云盘开放平台的认证信息。

## 获取认证凭证

### 申请开发者权限

1. 访问 [123云盘开放平台](https://www.123pan.com/developer)（或相应网址）
2. 登录您的123云盘账户
3. 申请开发者权限
4. 等待审核通过

### 获取 client_id 和 client_secret

审核通过后，您会收到 `client_id` 和 `client_secret`，这些信息用于获取访问令牌。

## 获取访问令牌

### 通过代码获取

```python
from pan123.auth import get_access_token

# 使用您的client_id和client_secret获取access_token
access_token = get_access_token("your_client_id", "your_client_secret")
print(f"Access Token: {access_token}")
```

### 重要提示

> ⚠️ **频率限制**：
> `get_access_token` 接口存在调用频次限制，建议获取到认证信息后将其持久化存储，避免频繁调用接口。

## 持久化存储认证信息

### 存储到文件

```python
# 获取并存储认证信息
access_token = get_access_token("your_client_id", "your_client_secret")

# 保存到文件
with open("access_token.txt", "w", encoding="utf-8") as f:
    f.write(access_token)
```

### 从文件读取认证信息

```python
# 从文件读取认证信息
with open("access_token.txt", "r", encoding="utf-8") as f:
    access_token = f.read().strip()

# 初始化客户端
from pan123 import Pan123
pan = Pan123(access_token)
```

## 使用环境变量（推荐）

为了安全起见，建议使用环境变量存储认证信息：

```python
import os
from pan123 import Pan123
from pan123.auth import get_access_token

# 从环境变量获取认证信息
client_id = os.getenv("PAN123_CLIENT_ID")
client_secret = os.getenv("PAN123_CLIENT_SECRET")

# 如果没有access_token文件，则获取新的access_token
if not os.path.exists("access_token.txt"):
    access_token = get_access_token(client_id, client_secret)
    with open("access_token.txt", "w", encoding="utf-8") as f:
        f.write(access_token)
else:
    with open("access_token.txt", "r", encoding="utf-8") as f:
        access_token = f.read().strip()

pan = Pan123(access_token)
```

## 初始化客户端

获取访问令牌后，即可初始化 Pan123 客户端：

```python
from pan123 import Pan123

# 使用access_token初始化客户端
pan = Pan123(access_token)

# 现在您可以使用各种功能
user_info = pan.user.info()
print(user_info)
```

## 注意事项

1. **安全存储**：请妥善保管您的 `client_id`、`client_secret` 和 `access_token`
2. **令牌过期**：access_token 可能会过期，需要重新获取
3. **权限范围**：不同权限的令牌支持不同功能，请根据需要申请相应权限
4. **速率限制**：注意API调用频率限制，避免被限制访问

## 下一步

认证配置完成后，您可以开始使用 Pan123，请阅读 [基本用法](./basic_usage.md) 了解如何开始使用。