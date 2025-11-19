# 🚀 基本用法

本页面介绍 Pan123 的基本使用方法。

## 初始化客户端

首先，您需要使用获取的访问令牌初始化 Pan123 客户端：

```python
from pan123 import Pan123

# 使用访问令牌初始化客户端
pan = Pan123("your_access_token")
```

## 主要功能模块

Pan123 提供以下功能模块：

- `pan.file` - 文件管理
- `pan.share` - 分享链接
- `pan.user` - 用户管理
- `pan.offline_download` - 离线下载
- `pan.direct_link` - 直链获取
- `pan.transcode` - 视频转码
- `pan.oss` - 图床服务

## 常见操作示例

### 文件操作

#### 上传文件

```python
# 上传文件到根目录（ID为0）
result = pan.file.upload(parent_file_id=0, file_path="/path/to/your/file.txt")
print("文件上传成功")
```

#### 获取文件列表

```python
# 获取根目录下前10个文件
files = pan.file.list(parent_file_id=0, limit=10)
for file_info in files["data"]["InfoList"]:
    print(f"文件名: {file_info['FileName']}, ID: {file_info['FileId']}")
```

#### 创建目录

```python
# 在根目录创建新文件夹
result = pan.file.mkdir(name="新文件夹", parent_id=0)
print(f"目录创建成功，ID: {result['data']['FileId']}")
```

### 分享操作

#### 创建分享链接

```python
# 创建一个7天后过期的分享链接
result = pan.share.create(
    share_name="我的分享",
    share_expire=7,  # 7天后过期
    file_id_list=[123],  # 分享文件ID为123的文件
    share_pwd="1234"  # 设置访问密码
)

print(f"分享链接: {result['shareLink']}")
```

### 用户信息

#### 获取用户信息

```python
# 获取当前账户信息
user_info = pan.user.info()
print(f"用户名: {user_info['data']['userName']}")
print(f"总容量: {user_info['data']['TotalCapacity']} bytes")
print(f"已使用: {user_info['data']['UsedCapacity']} bytes")
```

## 错误处理

在使用 Pan123 时，建议添加错误处理：

```python
try:
    # 执行操作
    result = pan.file.list(parent_file_id=0, limit=10)
    print("操作成功")
except Exception as e:
    print(f"操作失败: {e}")
```

## 完整示例

以下是一个完整的使用示例：

```python
from pan123 import Pan123
from pan123.auth import get_access_token
import os

def main():
    # 从文件读取access_token
    if os.path.exists("access_token.txt"):
        with open("access_token.txt", "r", encoding="utf-8") as f:
            access_token = f.read().strip()
    else:
        print("请先获取access_token并保存到access_token.txt文件中")
        return

    # 初始化客户端
    pan = Pan123(access_token)

    try:
        # 获取用户信息
        user_info = pan.user.info()
        print(f"用户: {user_info['data']['userName']}")
        
        # 获取根目录文件列表
        files = pan.file.list(parent_file_id=0, limit=5)
        print(f"根目录共有 {files['data']['TotalCount']} 个项目")
        print("前5个文件/文件夹:")
        
        for item in files["data"]["InfoList"]:
            item_type = "文件" if item["Type"] == 0 else "文件夹"
            print(f"  - {item_type}: {item['FileName']} (ID: {item['FileId']})")
        
        # 创建一个测试文件夹
        result = pan.file.mkdir(name="Pan123测试文件夹", parent_id=0)
        print(f"创建测试文件夹成功，ID: {result['data']['FileId']}")
        
    except Exception as e:
        print(f"执行操作时出错: {e}")

if __name__ == "__main__":
    main()
```

## 最佳实践

1. **持久化存储令牌**：获取access_token后应持久化存储，避免频繁获取
2. **错误处理**：对所有API调用进行适当的错误处理
3. **资源清理**：及时处理不需要的资源，如临时文件
4. **权限管理**：根据需要使用适当的权限令牌

## 下一步

了解基本用法后，您可以深入学习各个功能模块:

- [📁 文件管理](./🛠️ 函数/file.md)
- [🔗 分享链接](./🛠️ 函数/share.md)
- [📥 离线下载](./🛠️ 函数/offline_download.md)
- [🔗 直链获取](./🛠️ 函数/direct_link.md)
- [🎞️ 视频转码](./🛠️ 函数/transcode.md)
- [🖼️ 图床服务](./🛠️ 函数/oss.md)
- [👤 用户管理](./🛠️ 函数/user.md)