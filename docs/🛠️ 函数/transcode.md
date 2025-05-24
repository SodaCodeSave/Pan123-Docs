# 📎 转码

> 将视频转为m3u8格式的链接

## 🛠️ 函数

### 查询转码文件夹信息
```python
pan.transcode.folder_info(file_id)
```
- `file_id`
    - 转码空间文件夹ID

### 获取转码空间文件列表
```python
pan.transcode.file_list(parent_file_id, limit, search_data, search_mode, last_file_id)
```
- `parent_file_id`
    - 文件夹ID，根目录传 0
- `limit`
    - 每页文件数量，最大不超过100
- `search_data`
    - 搜索关键字将无视文件夹ID参数。将会进行全局查找
- `search_mode`
    - （可选）
    - 0:全文模糊搜索(注:将会根据搜索项分词,查找出相似的匹配项)
    - 1:精准搜索(注:精准搜索需要提供完整的文件名)
- `last_file_id`
    - 翻页查询时需要填写（可选）

### 从云盘空间上传
```python
pan.transcode.from_cloud_disk(file_id)
```
- `file_id`
    - 云盘空间文件ID
    - 注意：一次性最多支持100个文件上传

### 删除转码视频
```python
pan.transcode.delete(file_id, trashed)
```
- `file_id`
    - 要删除的文件ID。示例:`1`
- `trashed`
    - 1：删除原文件
    - 2：删除原文件+转码后的文件

### 获取视频文件可转码的分辨率

> 该接口需要轮询去查询结果，建议10s一次

```python
pan.transcode.video_resolution(file_id)
```
- `file_id`
    - 文件Id

### 转码视频
```python
pan.transcode.video(file_id, codec_name, video_time, resolutions)
```
- `file_id`
    - 文件Id
- `codec_name`
    - 编码方式
- `video_time`
    - 视频时长，单位：秒
- `resolutions`
    - 必填	要转码的分辨率，多个之间以逗号分割，如："2160P,1080P,720P",注意：p是大写，如果之前已经转码过别的分辨率，那就无需再传

### 查询某个视频的转码记录
```python
pan.transcode.video_record(file_id)
```
- `file_id`
    - 文件Id

### 查询某个视频的转码结果
```python
pan.transcode.video_result(file_id)
```
- `file_id`
    - 文件ID

### 原文件下载
```python
pan.transcode.file_download(file_id)
```
- `file_id`
    - 转码文件ID

### 单个转码文件下载（m3u8或ts）
```python
pan.transcode.m3u8_ts_download(file_id, resolution, type, ts_name)
```
- `fileid`
    - 转码文件ID。示例:`1`
- `resolution`
    - 分辨率
- `type`
    - 1代表下载m3u8文件
    - 2代表下载ts文件
- `ts_name`
    - 下载ts文件时必须要指定ts文件的名称，ts的名称参考查询某个视频的转码结果

### 某个视频全部转码文件下载
> 1. 该接口需要轮询去查询结果，建议10s一次
> 2. 这里只是返回下载地址，获取到下载地址之后，把地址放在浏览器中即可进行下载
```python
pan.transcode.file_download_all(file_id, zip_name)
```
- `file_id`
    - 文件Id
- `zip_name`
    - 下载zip文件的名字