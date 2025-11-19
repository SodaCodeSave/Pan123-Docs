# Pan123-Docs

Pan123-Docs 是 [Pan123](https://github.com/SodaCodeSave/Pan123) 库的官方文档项目。

Pan123 是123云盘开放平台的非官方Python封装库，用于在Python中与123云盘开放平台进行交互。

## 📚 文档地址

在线文档: [https://sodacodesave.github.io/Pan123-Docs/site/](https://sodacodesave.github.io/Pan123-Docs/site/)

## 📋 功能特性

Pan123 提供以下功能：

- 🔗 **分享链接** - 创建和管理分享链接
- 📁 **文件管理** - 上传、下载、移动、删除文件等
- 👤 **用户管理** - 获取用户信息
- 📥 **离线下载** - 下载网络资源到云盘
- 🔗 **直链获取** - 获取文件直链
- 🎞️ **视频转码** - 视频转码服务
- 🖼️ **图床功能** - 图片存储和管理

## 🚀 快速开始

1. 安装 Pan123:
   ```bash
   pip install pan123
   ```

2. 获取访问令牌：
   ```python
   from pan123.auth import get_access_token

   access_token = get_access_token("your_client_id", "your_client_secret")
   ```

3. 使用 Pan123:
   ```python
   from pan123 import Pan123

   pan = Pan123(access_token)

   # 获取用户信息
   user_info = pan.user.info()
   print(user_info)
   ```

## 📖 文档结构

- [首页](./docs/index.md) - 项目概述和快速入门
- [安装指南](./docs/installation.md) - 如何安装和配置
- [认证指南](./docs/authentication.md) - 如何获取和管理认证信息
- [基本用法](./docs/basic_usage.md) - 基本使用方法
- [功能文档](./docs/🛠️ 函数/) - 各功能模块详细说明
- [API参考](./docs/api_reference.md) - 完整API参考
- [错误码](./docs/error_codes.md) - 错误码及解决方案
- [更新日志](./docs/changelog.md) - 版本更新历史

## 🤝 贡献

欢迎提交 issue 和 pull request 来改进 Pan123 和其文档。

## 📄 许可证

本项目使用 MIT 许可证。

## 🐙 GitHub 仓库

- [Pan123 库](https://github.com/SodaCodeSave/Pan123)
- [Pan123-Docs 文档](https://github.com/SodaCodeSave/Pan123-Docs)