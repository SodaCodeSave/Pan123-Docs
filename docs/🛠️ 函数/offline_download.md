# 🔦 离线下载

> 离线下载可以在云端下载文件，而不需要用户在本地下载

## 🛠️ 函数
### 创建离线下载任务
```python
pan.offline_download.download(download_url, file_name, save_path, call_back_url)
```
- `dowload_url`
    - 下载资源地址(http/https)
- `file_name`(可选)
    - 自定义文件名称 （需携带图片格式，支持格式：png, gif, jpeg, tiff, webp, jpg, tif, svg, bmp）  
- `save_path`(可选)
    - 选择下载到指定目录ID。 示例:10023
    - 注:不支持下载到根目录,默认会下载到名为"来自:离线下载"的目录中
- `call_back_url`(可选)
    - 回调地址,当文件下载成功或者失败,均会通过回调地址通知。回调内容如下
        - url: 下载资源地址
        - status: 0 成功，1 失败
        - fileReason：失败原因
        - fileID:成功后,该文件在云盘上的ID
    - 请求类型：POST
> {
> 	"url": "http://dc.com/resource.jpg",
> 	"status": 0, 
> 	"failReason": "",
>   "fileID":100
> }
### 获取离线下载进度
```python
pan.offline_download.download_process(task_id)
```
- `task_id`
    - 离线下载任务ID