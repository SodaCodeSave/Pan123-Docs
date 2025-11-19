# 🖼️ OSS资源（图床）

OSS资源功能提供图床服务，可以轻松地将图片嵌入到您的网站或其他应用中。

## 📋 功能概览

- **文件管理** - 上传、下载、移动、重命名、删除文件等
- **目录管理** - 创建目录结构
- **文件操作** - 获取文件详情等
- **高级功能** - 文件复制、离线下载（已弃用）

## 🚀 快速示例

```python
from pan123 import Pan123

# 初始化客户端
pan = Pan123("your_access_token")

# 上传图片到根目录
result = pan.oss.upload(parent_file_id=0, file_path="/path/to/image.jpg")

# 从返回结果中获取图片链接
print(f"上传成功，文件ID: {result}")
```

## 🛠️ 详细功能

### 创建目录

创建新的目录来组织您的资源文件。

```python
result = pan.oss.mkdir(name, parent_id)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `name` | str | 是 | 文件夹名称 |
| `parent_id` | int | 是 | 父目录ID，上传到根目录时填写 0 |

### 重命名文件

批量重命名文件。

```python
result = pan.oss.rename(rename_dict)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `rename_dict` | dict | 是 | 重命名映射，格式为 `{文件ID: 新文件名}` |

**示例：**
```python
# 重命名ID为123的文件
result = pan.oss.rename({123: "new_image.jpg"})
```

### 移动文件

将文件移动到指定目录。

```python
result = pan.oss.move(file_id_list, to_parent_file_id)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_id_list` | list[int] | 是 | 要移动的文件ID列表 |
| `to_parent_file_id` | int | 是 | 移动到的目标文件夹ID |

### 删除文件

彻底删除文件。

```python
result = pan.oss.delete(file_ids)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_ids` | list | 是 | 要删除的文件ID列表 |

### 获取文件详情

获取指定文件的详细信息。

```python
result = pan.oss.detail(file_id)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_id` | int | 是 | 要获取详情的文件ID |

### 上传文件

#### 一键上传（推荐）

这是最简单的上传方式，会自动处理秒传和分片上传。

```python
result = pan.oss.upload(parent_file_id, file_path)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `parent_file_id` | int | 是 | 文件上传到哪个目录下 |
| `file_path` | str | 是 | 要上传的文件的路径 |

#### 手动上传（高级用法）

如需要更精确控制上传过程，可以使用手动上传方式：

1. **创建文件记录**
   ```python
   result = pan.oss.create(parent_file_id, filename, etag, size, duplicate)
   ```

   如果返回 `reuse: True`，表示秒传成功，上传结束。否则将返回预上传ID和分片大小。

2. **获取分片上传地址**
   ```python
   upload_url = pan.oss.get_upload_url(preupload_id, slice_no)
   ```

3. **上传分片**
   使用PUT请求上传文件分片到获取的URL地址

4. **校验分片上传状态**
   ```python
   parts_info = pan.oss.list_upload_parts(preupload_id)
   ```

5. **完成上传**
   ```python
   pan.oss.upload_complete(preupload_id)
   ```

6. **获取最终结果**（可选）
   ```python
   result = pan.oss.upload_async_result(preupload_id)
   ```

### 文件复制相关功能

> ⚠️ **注意**：以下功能属于高级操作，请谨慎使用。

#### 创建复制任务

创建文件复制任务，从源位置复制文件到图床。

```python
result = pan.oss_source_copy.copy(file_ids, to_parent_file_id, sourceType, type)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_ids` | list[str] | 是 | 需要复制的文件ID列表 |
| `to_parent_file_id` | int | 否 | 要移动到的图床目标文件夹ID，移动到根目录时为空 |
| `sourceType` | int | 是 | 复制来源（1=云盘） |
| `type` | int | 是 | 业务类型，固定为 1 |

#### 获取复制任务详情

查询复制任务的执行状态和详情。

```python
result = pan.oss_source_copy.process(task_id)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `task_id` | int | 是 | 复制任务ID |

#### 获取复制失败文件列表

获取复制过程中失败的文件列表。

```python
result = pan.oss_source_copy.fail(task_id, limit, page)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `task_id` | int | 是 | 失败任务ID |
| `limit` | int | 是 | 每页文件数量，最大不超过100 |
| `page` | int | 是 | 当前页码 |

## 📝 注意事项

1. **已弃用功能**：离线下载相关功能已在OSS模块中弃用，请使用主模块的离线下载功能。
2. **秒传功能**：系统会自动检测文件是否已存在，如果存在相同文件（通过ETag验证），将直接复用，无需上传。
3. **分片上传**：大文件会被切分为多个分片上传，以提高上传效率和稳定性。
4. **文件类型**：虽然图床主要用于图片，但也可以存储其他类型的文件。
5. **权限验证**：所有操作都需要有效的access_token。
6. **存储限制**：注意您的云盘存储空间限制。

## 🔗 相关链接

- [123云盘开放文档](https://123yunpan.yuque.com/org-wiki-123yunpan-muaork/cr6ced/uihb5w71iwqvi9dw)
- [错误码参考](./errors.md)