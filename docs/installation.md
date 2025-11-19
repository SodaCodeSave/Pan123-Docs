# 📦 安装指南

本页面介绍了如何安装和配置 Pan123 库。

## 系统要求

- Python 3.6 或更高版本
- 稳定的网络连接（用于连接123云盘API）

## 安装方法

### 使用 pip 安装（推荐）

```bash
pip install pan123
```

### 从源码安装

如果您想使用最新开发版本：

```bash
git clone https://github.com/SodaCodeSave/Pan123.git
cd Pan123
pip install -e .
```

## 验证安装

安装完成后，可以在Python环境中测试：

```python
import pan123
print(pan123.__version__)
```

如果能正常输出版本号，说明安装成功。

## 依赖项

Pan123 依赖以下Python库：
- `requests`
- `typing`（Python < 3.5 时需要）

通常这些依赖会在安装时自动安装。

## 常见问题

### 安装失败

如果在安装过程中遇到问题，可以尝试：

1. 更新pip版本：
   ```bash
   pip install --upgrade pip
   ```

2. 使用国内镜像源：
   ```bash
   pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pan123
   ```

### 权限问题

在某些系统上可能需要使用 `--user` 参数：

```bash
pip install --user pan123
```

## 下一步

安装完成后，您需要获取API认证信息，请继续阅读 [认证指南](./authentication.md)。