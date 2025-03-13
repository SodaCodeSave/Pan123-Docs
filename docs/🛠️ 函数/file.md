# 📁 文件管理

提供云端文件的基础管理能力，包含文件列表获取、目录创建等功能

## 🛠️ 函数

### 上传文件

#### 一键上传（推荐）

```python
pan.file.upload(parent_file_id, file_path)
```
- `parent_file_id`
    - 文件上传到哪个目录下
- `file_path`
    - 要上传的文件的路径

#### 手动上传

> 更多内容请参考[123云盘开放文档](https://123yunpan.yuque.com/org-wiki-123yunpan-muaork/cr6ced/uihb5w71iwqvi9dw)

1. 使用`pan.file.create(parent_file_id, filename, etag, size, duplicate)`创建文件，接口返回的reuse为true时，表示秒传成功，上传结束。非秒传的情况将会返回预上传ID `preuploadID` 与分片大小 `sliceSize`,请将文件根据分片大小切分。
2. 非秒传时，携带步骤1返回的`preuploadID` 与分片序号`sliceNo`，使用`get_upload_url(preupload_id, slice_no)`获取上传地址。
3. 用PUT请求步骤2中返回的地址，上传文件分片。
    - (推荐操作)文件分片上传完成后，使用`list_upload_parts(preupload_id)`函数，在本地进行与云端的分片md5比对
> 注:如果您的文件小于 sliceSize ,该操作将会返回空值,可以不进行这步操作。
4. 校验完成后，使用`upload_complete(preupload_id)`函数，完成上传。
    - 根据步骤4返回的结果，判断是否需要使用`upload_async_result(preupload_id)`函数，获取上传的最终结果。该时间需要等待，123云盘服务器会校验用户预上传时的MD5与实际上传成功的MD5是否一致。

### 创建目录

```python
pan.file.mkdir(name, parent_id)
```

- `name`
    - 文件夹名
- `parent_id`
    - 父目录id，上传到根目录时填写 0

### 重命名文件

```python
pan.file.rename(rename_dict)
```

- `rename_dict`
    - 文件重命名字典，格式为 `{文件ID: 新文件名}`

### 移动文件

```python
pan.file.move(file_id_list, to_parent_file_id)
```

- `file_id_list`
    - 要移动的文件ID列表
- `to_parent_file_id`
    - 移动到的目标文件夹ID

### 删除文件至回收站

```python
pan.file.trash(file_ids)
```

- `file_ids`
    - 要删除的文件ID列表

### 从回收站恢复文件

```python
pan.file.recover(file_ids)
```

- `file_ids`
    - 要恢复的文件ID列表

### 彻底删除文件
```python
pan.file.delete(file_ids)
```
- `file_ids`
    - 要彻底删除的文件ID列表，文件必须在回收站中

### 获取文件详情
```python
pan.file.detail(file_id)
```
- `file_id`
    - 要获取详情的文件ID

### 获取文件列表

```python
pan.file.list(parent_file_id, limit, search_data, search_mode, last_file_id)
```

- `parent_file_id`
    - 父文件夹ID，根目录传 0
- `limit`
    - 每页文件数量，最大不超过100
- `search_data` (可选)
    - 搜索关键字，将无视文件夹ID参数。将会进行全局查找
- `search_mode` (可选)
    - 0:全文模糊搜索(注:将会根据搜索项分词,查找出相似的匹配项)  
    - 1:精准搜索(注:精准搜索需要提供完整的文件名)
- `last_file_id` (可选)
    - 翻页查询时需要填写，最后一个文件的ID
