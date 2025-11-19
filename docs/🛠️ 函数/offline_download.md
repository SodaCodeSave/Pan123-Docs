# 📥 离线下载

离线下载功能允许您在云端下载网络资源到123云盘，无需在本地下载后再上传。

## 📋 功能概览

- **创建下载任务** - 创建网络资源的离线下载任务
- **查询下载进度** - 获取下载任务的当前状态

## 🚀 快速示例

```python
from pan123 import Pan123

# 初始化客户端
pan = Pan123("your_access_token")

# 创建离线下载任务
result = pan.offline_download.download(
    download_url="https://example.com/file.zip",
    file_name="example_file.zip",  # 可选：自定义文件名
    save_path=123  # 可选：保存到指定目录ID
)

task_id = result["data"]["taskID"]  # 获取任务ID
print(f"下载任务已创建，任务ID: {task_id}")

# 查询下载进度
process_result = pan.offline_download.download_process(task_id)
print(f"下载进度: {process_result}")
```

## 🛠️ 详细功能

### 创建离线下载任务

创建一个新的离线下载任务。

```python
result = pan.offline_download.download(download_url, file_name, save_path, call_back_url)
```

| 参数 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| `download_url` | str | 是 | - | 要下载的网络资源地址（必须是http/https链接） |
| `file_name` | str | 否 | 原文件名 | 自定义文件名（需包含文件扩展名） |
| `save_path` | int | 否 | - | 保存到指定目录ID（不支持根目录） |
| `call_back_url` | str | 否 | - | 下载状态回调URL |

**返回值说明：**
- `taskID` - 创建的下载任务唯一标识

> **注意**：
> - 下载任务默认会保存到名为"来自:离线下载"的目录中
> - 不支持直接下载到根目录
> - 如果提供了回调URL，下载成功或失败时会向该URL发送POST请求

**回调请求格式：**
```json
{
    "url": "下载资源地址",
    "status": 0,  // 0表示成功，1表示失败
    "failReason": "",  // 失败原因，成功时为空
    "fileID": 100  // 下载成功后的文件ID
}
```

**示例：**
```python
# 创建下载任务，使用自定义文件名并指定保存目录
result = pan.offline_download.download(
    download_url="https://example.com/video.mp4",
    file_name="my_video.mp4",
    save_path=456,  # 保存到ID为456的目录
    call_back_url="https://your-domain.com/callback"  # 可选：回调地址
)

task_id = result["data"]["taskID"]
print(f"任务已创建，ID: {task_id}")
```

### 获取离线下载进度

查询指定离线下载任务的当前状态和进度。

```python
result = pan.offline_download.download_process(task_id)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `task_id` | int | 是 | 要查询的离线下载任务ID |

**示例：**
```python
# 查询任务进度
process_result = pan.offline_download.download_process(task_id=12345)
print(process_result)

# 可能的返回数据包含任务状态、进度等信息
```

## 📝 注意事项

1. **资源限制**：仅支持HTTP/HTTPS协议的网络资源。
2. **文件类型**：部分特殊文件类型可能不支持下载。
3. **网络限制**：受云端网络环境限制，某些资源可能无法下载。
4. **任务管理**：下载任务在后台执行，需要主动查询进度。
5. **回调功能**：使用回调功能可以及时获知下载结果，无需频繁查询。
6. **目录限制**：无法直接下载到根目录，系统会默认创建"来自:离线下载"目录存放文件。

## 🔗 相关链接

- [123云盘开放文档](https://123yunpan.yuque.com/org-wiki-123yunpan-muaork/cr6ced/uihb5w71iwqvi9dw)
- [错误码参考](./errors.md)