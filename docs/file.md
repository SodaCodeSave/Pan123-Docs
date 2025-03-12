# 📁 文件管理

提供云端文件的基础管理能力，包含文件列表获取、目录创建等功能

## 🛠️ 函数

### 获取文件列表

```python
pan.file.list(parent_file_id, limit, search_data, search_mode, last_file_id)
```

- parent_file_id
    - 父文件夹ID，根目录传 0
- limit
    - 每页文件数量，最大不超过100
- search_data (可选)
    - 搜索关键字，将无视文件夹ID参数。将会进行全局查找
- search_mode (可选)
    - 0:全文模糊搜索(注:将会根据搜索项分词,查找出相似的匹配项)  
    - 1:精准搜索(注:精准搜索需要提供完整的文件名)
- last_file_id
    - 翻页查询时需要填写，最后一个文件的ID

### 创建目录

```python
pan.file.mkdir(name, parent_id)
```

- name
    - 文件夹名
- parent_id
    - 父目录id，上传到根目录时填写 0
