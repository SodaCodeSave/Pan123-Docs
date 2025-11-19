# 📁 文件管理

使用文件管理功能，可以让您脱离123云盘客户端来操作文件，包括上传、下载、移动、重命名等操作。

## 📋 功能概览

- **上传文件** - 支持一键上传和分片上传
- **文件操作** - 创建目录、重命名、移动、删除等
- **文件查询** - 获取文件列表、详情
- **文件恢复** - 从回收站恢复文件

## 🚀 快速示例

```python
from pan123 import Pan123

# 初始化客户端
pan = Pan123("your_access_token")

# 上传文件到根目录
result = pan.file.upload(0, "/path/to/your/file.txt")

# 获取根目录文件列表
files = pan.file.list(0, 10)  # 获取前10个项目
```

## 🛠️ 详细功能

### 上传文件

#### 一键上传（推荐）

一键上传函数会自动处理秒传和分片上传逻辑，是最推荐的上传方法。

```python
result = pan.file.upload(parent_file_id, file_path)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `parent_file_id` | int | 是 | 文件上传到哪个目录下，根目录为 0 |
| `file_path` | str | 是 | 要上传的文件的路径 |

#### 手动上传（高级用法）

对于需要更精确控制上传过程的场景，可以使用手动上传方式：

1. **创建文件记录**
   ```python
   result = pan.file.create(parent_file_id, filename, etag, size, duplicate)
   ```

   如果返回 `reuse: True`，表示秒传成功，上传结束。否则将返回预上传ID和分片大小。

2. **获取分片上传地址**
   ```python
   upload_url = pan.file.get_upload_url(preupload_id, slice_no)
   ```

3. **上传分片**
   使用PUT请求上传文件分片到获取的URL地址

4. **校验分片上传状态**
   ```python
   parts_info = pan.file.list_upload_parts(preupload_id)
   ```

5. **完成上传**
   ```python
   pan.file.upload_complete(preupload_id)
   ```

6. **获取最终结果**（可选）
   ```python
   result = pan.file.upload_async_result(preupload_id)
   ```

### 创建目录

创建新目录。

```python
result = pan.file.mkdir(name, parent_id)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `name` | str | 是 | 文件夹名 |
| `parent_id` | int | 是 | 父目录ID，根目录为 0 |

### 重命名文件/目录

批量重命名文件或目录。

```python
result = pan.file.rename(rename_dict)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `rename_dict` | dict | 是 | 重命名映射，格式为 `{文件ID: 新名称}` |

**示例：**
```python
# 重命名文件ID为123的文件为"新文件名.txt"
rename_dict = {123: "新文件名.txt"}
result = pan.file.rename(rename_dict)
```

### 移动文件/目录

将文件或目录移动到指定位置。

```python
result = pan.file.move(file_id_list, to_parent_file_id)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_id_list` | list[int] | 是 | 要移动的文件ID列表 |
| `to_parent_file_id` | int | 是 | 移动到的目标目录ID |

### 删除文件至回收站

将文件移至回收站（软删除）。

```python
result = pan.file.trash(file_ids)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_ids` | list | 是 | 要删除的文件ID列表 |

### 从回收站恢复文件

从回收站恢复文件到原位置。

```python
result = pan.file.recover(file_ids)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_ids` | list | 是 | 要恢复的文件ID列表 |

### 彻底删除文件

从回收站彻底删除文件（永久删除）。

> ⚠️ **警告**：此操作不可恢复，请谨慎使用。

```python
result = pan.file.delete(file_ids)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_ids` | list | 是 | 要彻底删除的文件ID列表，文件必须在回收站中 |

### 获取文件详情

获取指定文件的详细信息。

```python
result = pan.file.detail(file_id)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_id` | int | 是 | 要获取详情的文件ID |

### 获取文件列表

列出指定目录下的文件和子目录。

```python
result = pan.file.list(parent_file_id, limit, search_data, search_mode, last_file_id)
```

| 参数 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| `parent_file_id` | int | 是 | - | 父目录ID，根目录为 0 |
| `limit` | int | 是 | - | 每页文件数量，最大不超过100 |
| `search_data` | str | 否 | "" | 搜索关键字，设置后将进行全局搜索 |
| `search_mode` | int | 否 | 0 | 搜索模式：0-全文模糊搜索，1-精准搜索 |
| `last_file_id` | int | 否 | 0 | 翻页参数，用于获取下一页数据 |

> **提示**：当搜索模式为0时，系统会根据搜索项分词，查找出相似的匹配项；当搜索模式为1时，需要提供完整的文件名。

### 获取文件下载链接

获取文件的下载链接。

```python
download_url = pan.file.download(file_id)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_id` | int | 是 | 要获取下载链接的文件ID |

## 📝 注意事项

1. **秒传功能**：系统会自动检测文件是否已存在，如果存在相同文件（通过ETag验证），将直接复用，无需上传。
2. **分片上传**：大文件会被切分为多个分片上传，以提高上传效率和稳定性。
3. **权限验证**：所有操作都需要有效的access_token。
4. **回收站机制**：删除文件会先移至回收站，可在回收站中恢复，彻底删除才会永久移除。

## 🔗 相关链接

- [123云盘开放文档](https://123yunpan.yuque.com/org-wiki-123yunpan-muaork/cr6ced/uihb5w71iwqvi9dw)
- [错误码参考](./errors.md)
