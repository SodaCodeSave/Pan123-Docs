# 📎 直链

> 直链可以将文件存储在CDN中，可以放置在个人网站等位置

## 🛠️ 函数

### 查询直链转码进度

```python
pan.direct_link.query_transcode(ids)
```

- `ids`
    - 视频文件ID列表。示例:`[1, 2, 3, 4]`

### 发起直链转码

```python
pan.direct_link.do_transcode(ids)
```

- `ids`
    - 需要转码的文件ID列表,请注意该文件必须要在直链空间下,且源文件是视频文件才能进行转码操作。示例:`[1, 2, 3, 4]`

### 获取直链转码链接

```python
pan.direct_link.get_m3u8(file_id)
```

- `file_id`
    - 启用直链空间的文件夹的fileID

### 启用直链空间

```python
pan.direct_link.enable(file_id)
```

- `file_id`
    - 启用直链空间的文件夹的fileID

### 禁用直链空间

```python
pan.direct_link.disable(file_id)
```

- `file_id`
    - 禁用直链空间的文件夹的fileID

### 获取直链链接
```python
pan.direct_link.list_url(file_id)
```

- `file_id`
    - 需要获取直链链接的文件的fileID
