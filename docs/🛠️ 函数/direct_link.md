# 🔗 直链获取

直链功能允许您获取文件的直接访问链接，可以用于在个人网站或其他应用中直接引用文件资源。

## 📋 功能概览

- **启用直链空间** - 为文件夹启用直链功能
- **获取直链** - 获取文件的直接访问链接
- **视频转码** - 对视频文件进行转码以支持在线播放
- **转码管理** - 查询转码进度和获取转码后的播放链接

## 🚀 快速示例

```python
from pan123 import Pan123

# 初始化客户端
pan = Pan123("your_access_token")

# 为文件夹启用直链空间
pan.direct_link.enable(folder_id=123)

# 获取文件的直链
direct_url = pan.direct_link.list_url(file_id=456)
print(f"文件直链: {direct_url}")
```

## 🛠️ 详细功能

### 启用直链空间

为指定文件夹启用直链功能，启用后该文件夹下的文件可以获取直链。

```python
result = pan.direct_link.enable(file_id)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_id` | int | 是 | 要启用直链空间的文件夹ID |

### 禁用直链空间

禁用指定文件夹的直链功能。

```python
result = pan.direct_link.disable(file_id)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_id` | int | 是 | 要禁用直链空间的文件夹ID |

### 获取直链链接

获取指定文件的直接访问链接。

```python
result = pan.direct_link.list_url(file_id)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_id` | int | 是 | 需要获取直链链接的文件ID |

### 发起视频转码

对视频文件进行转码，以便在网页中直接播放。

> ⚠️ **注意**：文件必须在已启用直链空间的文件夹中，且为视频文件才能进行转码。

```python
result = pan.direct_link.do_transcode(ids)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `ids` | list[int] | 是 | 需要转码的视频文件ID列表 |

**示例：**
```python
# 对ID为123和456的视频文件进行转码
result = pan.direct_link.do_transcode([123, 456])
```

### 查询转码进度

查询视频转码的进度状态。

```python
result = pan.direct_link.query_transcode(ids)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `ids` | list[int] | 是 | 要查询转码进度的视频文件ID列表 |

### 获取转码后播放链接

获取转码后的视频播放链接（M3U8格式）。

```python
result = pan.direct_link.get_m3u8(file_id)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_id` | int | 是 | 已转码视频文件的ID |

## 📝 注意事项

1. **空间限制**：只有在已启用直链空间的文件夹中的文件才能获取直链。
2. **文件类型**：视频转码功能仅适用于视频文件。
3. **转码时间**：视频转码可能需要一些时间，建议在转码完成后查询进度。
4. **链接时效**：直链可能存在时效性，请根据需要定期获取。
5. **访问权限**：直链访问可能需要特定权限，确保链接的安全性。

## 🔗 相关链接

- [123云盘开放文档](https://123yunpan.yuque.com/org-wiki-123yunpan-muaork/cr6ced/uihb5w71iwqvi9dw)
- [错误码参考](./errors.md)
