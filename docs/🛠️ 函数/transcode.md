# 🎞️ 视频转码

视频转码功能将视频文件转换为适合在线播放的格式（如m3u8），支持多种分辨率转码。

## 📋 功能概览

- **转码空间管理** - 管理转码空间文件夹和文件
- **视频转码** - 将视频文件转码为多种分辨率
- **转码管理** - 查询转码进度、记录和结果
- **文件下载** - 下载转码后的视频文件

## 🚀 快速示例

```python
from pan123 import Pan123

# 初始化客户端
pan = Pan123("your_access_token")

# 获取视频文件可转码的分辨率
resolution_info = pan.transcode.video_resolution(file_id=123)

# 执行视频转码
result = pan.transcode.video(
    file_id=123,
    codec_name="h264",
    video_time=120,  # 视频时长（秒）
    resolutions="1080P,720P"  # 转码为1080P和720P
)

# 查询转码结果
transcode_result = pan.transcode.video_result(file_id=123)
print(transcode_result)
```

## 🛠️ 详细功能

### 查询转码文件夹信息

获取指定转码空间文件夹的基本信息。

```python
result = pan.transcode.folder_info(file_id)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_id` | int | 是 | 转码空间文件夹ID |

### 获取转码空间文件列表

获取转码空间中指定文件夹的文件列表。

```python
result = pan.transcode.file_list(parent_file_id, limit, search_data, search_mode, last_file_id)
```

| 参数 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| `parent_file_id` | int | 是 | - | 文件夹ID，根目录传 0 |
| `limit` | int | 是 | - | 每页文件数量，最大不超过100 |
| `search_data` | str | 否 | "" | 搜索关键字，将进行全局查找 |
| `search_mode` | int | 否 | 0 | 搜索模式：0-全文模糊搜索，1-精准搜索 |
| `last_file_id` | int | 否 | 0 | 翻页参数，用于获取下一页数据 |

### 从云盘空间上传

将云盘中的文件添加到转码空间。

```python
result = pan.transcode.from_cloud_disk(file_id)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_id` | int | 是 | 云盘空间文件ID |

> **注意**：一次性最多支持100个文件上传。

### 删除转码视频

删除转码视频及其相关文件。

```python
result = pan.transcode.delete(file_id, trashed)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_id` | int | 是 | 要删除的文件ID |
| `trashed` | int | 是 | 删除选项：1-删除原文件，2-删除原文件+转码后的文件 |

### 获取视频文件可转码的分辨率

查询视频文件支持的转码分辨率。

> ⚠️ **注意**：该接口需要轮询查询结果，建议每10秒查询一次。

```python
result = pan.transcode.video_resolution(file_id)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_id` | int | 是 | 文件ID |

### 转码视频

将视频文件转码为指定分辨率格式。

```python
result = pan.transcode.video(file_id, codec_name, video_time, resolutions)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_id` | int | 是 | 文件ID |
| `codec_name` | str | 是 | 编码方式（如"h264"） |
| `video_time` | int | 是 | 视频时长，单位：秒 |
| `resolutions` | str | 是 | 要转码的分辨率，多个之间以逗号分割，如："2160P,1080P,720P" |

> **注意**：分辨率中的"P"是大写字母。如果之前已经转码过别的分辨率，无需重复传输。

### 查询某个视频的转码记录

获取指定视频的转码历史记录。

```python
result = pan.transcode.video_record(file_id)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_id` | int | 是 | 文件ID |

### 查询某个视频的转码结果

获取指定视频的转码结果详情。

```python
result = pan.transcode.video_result(file_id)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_id` | int | 是 | 文件ID |

### 原文件下载

下载转码前的原始视频文件。

```python
result = pan.transcode.file_download(file_id)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_id` | int | 是 | 转码文件ID |

### 单个转码文件下载

下载转码后的特定格式文件（m3u8或ts）。

```python
result = pan.transcode.m3u8_ts_download(file_id, resolution, type, ts_name)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_id` | int | 是 | 转码文件ID |
| `resolution` | str | 是 | 分辨率 |
| `type` | int | 是 | 下载类型：1-下载m3u8文件，2-下载ts文件 |
| `ts_name` | str | 否 | 下载ts文件时必须指定ts文件的名称 |

> **注意**：下载ts文件时必须指定ts文件的名称，名称可从查询转码结果接口中获取。

### 某个视频全部转码文件下载

下载视频的所有转码文件打包为zip格式。

> ⚠️ **注意**：
> 1. 该接口需要轮询查询结果，建议每10秒查询一次。
> 2. 接口仅返回下载地址，获取下载地址后在浏览器中访问即可下载。

```python
result = pan.transcode.file_download_all(file_id, zip_name)
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_id` | int | 是 | 文件ID |
| `zip_name` | str | 是 | 下载zip文件的名称 |

## 📝 注意事项

1. **轮询查询**：部分接口（如查询转码结果）需要轮询查询，建议间隔10秒。
2. **编码格式**：转码时需正确指定视频编码格式，常见的有h264等。
3. **分辨率格式**：分辨率参数中的"P"必须为大写字母。
4. **转码耗时**：视频转码需要一定时间，具体时长取决于视频大小和复杂度。
5. **存储空间**：转码后的文件会占用额外的存储空间，需确保有足够的容量。
6. **批量处理**：从云盘上传转码支持批量操作，最多100个文件。

## 🔗 相关链接

- [123云盘开放文档](https://123yunpan.yuque.com/org-wiki-123yunpan-muaork/cr6ced/uihb5w71iwqvi9dw)
- [错误码参考](./errors.md)