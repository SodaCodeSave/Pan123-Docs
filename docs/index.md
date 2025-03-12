# 👏 欢迎使用Pan123

欢迎使用Pan123~ 这里是Pan123的官方文档

## 📦 安装指南

通过pip安装最新版本库：

```bash
pip install pan123
```

## 🚀 快速入门

在代码中导入~

```python
from pan123 import Pan123
```

如果你需要获取access_token的话，还需要导入get_access_token函数

```python
from pan123.auth import get_access_token
```

## 🔑 认证凭证获取

导入完成后，如果你需要获取access_token，请使用如下代码进行获取，试试看吧~

```python
# 获取access_token并存储在access_token变量中
access_token = get_access_token("你的client_id", "你的client_secret")
```

> ⚠️ 重要提示：  
> `get_access_token` 存在接口调用频次限制，建议完成认证后持久化存储凭证

建议按如下方式存储`get_access_token`

```python
with open("access_token.txt", "r") as f:
    access_token = f.read()
```

## 📂 凭证加载与初始化

从存储介质加载凭证并初始化客户端：

```python
pan = Pan123(access_token)
```

Pan123的基础教程就说到这里，感谢您耐心看到最后

那么，接下来就以做出一个程序为目标，一起来Pan123吧~
