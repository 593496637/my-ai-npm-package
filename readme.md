# @likaikai/my-ai-npm-package

[![npm version](https://badge.fury.io/js/%40likaikai%2Fmy-ai-npm-package.svg)](https://badge.fury.io/js/%40likaikai%2Fmy-ai-npm-package)
[![License: ISC](https://img.shields.io/badge/License-ISC-blue.svg)](https://opensource.org/licenses/ISC)

一个简单而实用的自定义 npm 包，提供基础的功能模块。

## 📋 目录

- [安装](#安装)
- [快速开始](#快速开始)
- [API 文档](#api-文档)
- [使用示例](#使用示例)
- [开发指南](#开发指南)
- [更新日志](#更新日志)
- [许可证](#许可证)

## 安装

### 从私有仓库安装

```bash
npm install @likaikai/my-ai-npm-package
```

### 配置私有仓库

确保你的项目根目录有 `.npmrc` 文件：

```bash
# 私有包使用私仓
@likaikai:registry=http://43.252.229.237:4873

# 公共包使用官方源
registry=https://registry.npmjs.org
```

## 快速开始

```javascript
const myPackage = require('@likaikai/my-ai-npm-package');

// 使用基础功能
const message = myPackage.hello();
console.log(message); // 输出: Hello from my private npm!
```

## API 文档

### `hello()`

返回一个欢迎消息。

**返回值：**
- `string` - 欢迎消息字符串

**示例：**
```javascript
const myPackage = require('@likaikai/my-ai-npm-package');
const greeting = myPackage.hello();
console.log(greeting); // "Hello from my private npm!"
```

## 使用示例

### 基础使用

```javascript
const myPackage = require('@likaikai/my-ai-npm-package');

// 获取欢迎消息
const welcomeMessage = myPackage.hello();
console.log(welcomeMessage);
```

### 在 Express 应用中使用

```javascript
const express = require('express');
const myPackage = require('@likaikai/my-ai-npm-package');

const app = express();

app.get('/', (req, res) => {
  const message = myPackage.hello();
  res.json({ message });
});

app.listen(3000, () => {
  console.log('服务器运行在端口 3000');
});
```

### 在 Node.js 脚本中使用

```javascript
#!/usr/bin/env node

const myPackage = require('@likaikai/my-ai-npm-package');

function main() {
  console.log('=== 我的 AI NPM 包演示 ===');
  console.log(myPackage.hello());
  console.log('========================');
}

main();
```

## 开发指南

### 本地开发

1. 克隆项目：
```bash
git clone <repository-url>
cd my-ai-npm-package
```

2. 安装依赖：
```bash
npm install
```

3. 运行测试：
```bash
npm test
```

### 发布新版本

1. 更新版本号：
```bash
npm version patch  # 或 minor, major
```

2. 发布到私有仓库：
```bash
npm publish
```

### 项目结构

```
my-ai-npm-package/
├── index.js          # 主入口文件
├── package.json      # 包配置文件
├── readme.md         # 项目文档
├── deployment-guide.md # 部署指南
└── .npmrc           # npm 配置文件
```

## 更新日志

### [1.2.0] - 2025-01-XX

#### 新增
- 更新 README 文档
- 添加完整的 API 文档
- 添加使用示例

#### 修复
- 优化代码结构

### [1.1.0] - 2025-09-21

#### 新增
- 基础功能实现
- 私有仓库发布

### [1.0.0] - 2025-09-20

#### 新增
- 初始版本发布
- 基础 hello 功能

## 贡献

欢迎贡献代码！请遵循以下步骤：

1. Fork 本项目
2. 创建你的特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交你的更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 打开一个 Pull Request

## 许可证

本项目基于 ISC 许可证开源 - 查看 [LICENSE](LICENSE) 文件了解详情。

## 联系方式

- **作者**: likaikai
- **邮箱**: [your-email@example.com]
- **项目链接**: [https://github.com/likaikai/my-ai-npm-package]

## 相关链接

- [私有 NPM 仓库部署指南](./deployment-guide.md)
- [Verdaccio 官方文档](https://verdaccio.org/zh-cn/)
- [npm 官方文档](https://docs.npmjs.com/)

---

**维护人员**: @likaikai  
**最后更新**: 2025-01-XX