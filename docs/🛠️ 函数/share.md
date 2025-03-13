# 📨 分享链接

提供分享文件的基础功能，包括创建分享、修改分享等功能

## 🛠️ 函数

### 创建分享

```python
pan.share.create(share_name, share_expire, file_id_list, share_pwd, traffic_switch, traffic_limit_switch, traffic_limit)
```

- `share_name`
    - 分享名称
- `share_expire`
    - 分享链接有效期天数,该值为枚举
    - 固定只能填写:1、7、30、0
    - 填写0时代表永久分享
- `file_id_list`
    - 分享文件ID列表, 最大只支持100个文件IDs
- `share_pwd`(可选)
    - 分享密码, 默认为空
- `traffic_switch`(可选)
    - 免登录流量包开关，True表示开启，False表示关闭
- `traffic_limit_switch`(可选)
    - 流量限制开关，True表示开启，False表示关闭
    - 流量限制开关为True时，traffic_limit必填
- `traffic_limit`(可选)
    - 免登陆限制流量 单位：字节

### 修改分享

```python
pan.share.list_info(share_id_list, traffic_switch, traffic_limit_switch, traffic_limit)
```

- `share_id_list`
    - 要修改的分享ID列表
- `traffic_switch`(可选)
    - 免登录流量包开关，True表示开启，False表示关闭
- `traffic_limit_switch`(可选)
    - 流量限制开关，True表示开启，False表示关闭
    - 流量限制开关为True时，traffic_limit必填
- `traffic_limit`(可选)
    - 免登陆限制流量 单位：字节

### 获取分享链接列表

```python
pan.share.list(limit, last_share_id)
```

- `limit`
    - 每页文件数量，最大不超过100
- `last_share_id`(可选)
    - 分享ID，用于分页，默认为0