# 📚 API 参考

本页面提供 Pan123 库的完整API参考。

## Pan123 类

这是 Pan123 库的主要类，提供对所有功能模块的访问。

### 初始化

```python
from pan123 import Pan123

pan = Pan123(
    access_token: str,
    base_url: str = "https://open-api.123pan.com",
    header: dict = {
        "Content-Type": "application/json",
        "Platform": "open_platform",
    }
)
```

#### 参数

| 参数 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| `access_token` | str | 是 | - | 从123云盘开放平台获取的访问令牌 |
| `base_url` | str | 否 | "https://open-api.123pan.com" | API基础URL |
| `header` | dict | 否 | 见上方 | 请求头配置 |

## 功能模块

### 文件管理 (pan.file)

主要功能：
- 上传、下载、移动、删除文件
- 创建目录和重命名
- 查询文件详情和列表

**属性方法:**
- `upload(parent_file_id, file_path)` - 一键上传文件
- `create(parent_file_id, filename, etag, size, duplicate)` - 创建文件记录
- `mkdir(name, parent_id)` - 创建目录
- `rename(rename_dict)` - 重命名文件
- `move(file_id_list, to_parent_file_id)` - 移动文件
- `trash(file_ids)` - 删除文件至回收站
- `recover(file_ids)` - 从回收站恢复文件
- `delete(file_ids)` - 彻底删除文件
- `detail(file_id)` - 获取文件详情
- `list(parent_file_id, limit, search_data, search_mode, last_file_id)` - 获取文件列表
- `download(file_id)` - 获取文件下载链接

### 分享管理 (pan.share)

主要功能：
- 创建和管理分享链接
- 查询分享列表

**属性方法:**
- `create(share_name, share_expire, file_id_list, share_pwd, traffic_switch, traffic_limit_switch, traffic_limit)` - 创建分享
- `list_info(share_id_list, traffic_switch, traffic_limit_switch, traffic_limit)` - 修改分享设置
- `list(limit, last_share_id)` - 获取分享列表

### 用户管理 (pan.user)

主要功能：
- 获取用户信息

**属性方法:**
- `info()` - 获取用户信息

### 离线下载 (pan.offline_download)

主要功能：
- 创建和查询离线下载任务

**属性方法:**
- `download(download_url, file_name, save_path, call_back_url)` - 创建离线下载任务
- `download_process(task_id)` - 获取下载进度

### 直链获取 (pan.direct_link)

主要功能：
- 获取文件直链
- 视频转码

**属性方法:**
- `enable(file_id)` - 启用直链空间
- `disable(file_id)` - 禁用直链空间
- `list_url(file_id)` - 获取直链链接
- `do_transcode(ids)` - 发起直链转码
- `query_transcode(ids)` - 查询直链转码进度
- `get_m3u8(file_id)` - 获取直链转码链接

### 视频转码 (pan.transcode)

主要功能：
- 视频文件转码
- 分辨率管理

**属性方法:**
- `folder_info(file_id)` - 查询转码文件夹信息
- `file_list(parent_file_id, limit, search_data, search_mode, last_file_id)` - 获取转码空间文件列表
- `from_cloud_disk(file_id)` - 从云盘空间上传
- `delete(file_id, trashed)` - 删除转码视频
- `video_resolution(file_id)` - 获取视频文件可转码的分辨率
- `video(file_id, codec_name, video_time, resolutions)` - 转码视频
- `video_record(file_id)` - 查询某个视频的转码记录
- `video_result(file_id)` - 查询某个视频的转码结果
- `file_download(file_id)` - 原文件下载
- `m3u8_ts_download(file_id, resolution, type, ts_name)` - 单个转码文件下载
- `file_download_all(file_id, zip_name)` - 某个视频全部转码文件下载

### 图床服务 (pan.oss)

主要功能：
- 图片上传和管理
- 类似文件管理功能

**属性方法:**
- `upload(parent_file_id, file_path)` - 上传文件
- `mkdir(name, parent_id)` - 创建目录
- `rename(rename_dict)` - 重命名文件
- `move(file_id_list, to_parent_file_id)` - 移动文件
- `delete(file_ids)` - 删除文件
- `detail(file_id)` - 获取文件详情

## 认证模块

### 获取访问令牌

```python
from pan123.auth import get_access_token

access_token = get_access_token(client_id: str, client_secret: str) -> str
```

#### 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `client_id` | str | 是 | 123云盘开放平台分配的客户端ID |
| `client_secret` | str | 是 | 123云盘开放平台分配的客户端密钥 |

#### 返回值

返回字符串形式的访问令牌。

## 错误处理

Pan123 可能抛出以下异常：

- `ClientKeyError` - 客户端认证失败
- `AccessTokenError` - 访问令牌无效或过期
- `PacketLossError` - 文件上传过程中出现数据包丢失

## 常量

### 搜索模式 (SearchMode)

用于指定搜索行为：
- `SearchMode.NORMAL` - 正常搜索
- `SearchMode.FUZZY` - 模糊搜索
- `SearchMode.PRECISE` - 精确搜索

### 重复文件处理模式 (DuplicateMode)

用于指定重复文件处理方式：
- `DuplicateMode.RENAME` - 重命名
- `DuplicateMode.OVERWRITE` - 覆盖
- `DuplicateMode.SKIP` - 跳过

## 高级用法

### 自定义请求头

```python
pan = Pan123(
    access_token="your_token",
    header={
        "Content-Type": "application/json",
        "Platform": "open_platform",
        "User-Agent": "MyApp/1.0"  # 自定义User-Agent
    }
)
```

### 设置自定义API地址

```python
pan = Pan123(
    access_token="your_token",
    base_url="https://custom-api.example.com"  # 使用自定义API地址
)
```