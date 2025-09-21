# Verdaccio 私有 NPM 仓库部署指南

## 📋 目录

- [项目简介](#项目简介)
- [服务器信息](#服务器信息)
- [部署步骤](#部署步骤)
- [使用指南](#使用指南)
- [常见问题](#常见问题)
- [维护管理](#维护管理)

## 项目简介

本项目使用 Verdaccio 搭建私有 NPM 仓库，用于管理和发布内部 npm 包。

- **仓库地址**：`http://43.252.229.237:4873`
- **版本**：Verdaccio 6.1.6
- **Node.js**：v22.18.0

## 服务器信息

- **IP 地址**：43.252.229.237
- **操作系统**：Ubuntu
- **配置文件路径**：`/root/verdaccio/config.yaml`
- **存储路径**：`/verdaccio/storage`
- **用户认证文件**：`/root/verdaccio/htpasswd`

## 部署步骤

### 1. 连接服务器

```bash
ssh root@43.252.229.237
```

### 2. 安装 Node.js 和 npm

```bash
apt update
apt install -y nodejs npm
```

### 3. 安装 Verdaccio

```bash
npm install -g verdaccio
```

### 4. 配置 Verdaccio

编辑配置文件：

```bash
nano /root/verdaccio/config.yaml
```

关键配置项：

```yaml
# 存储路径
storage: /verdaccio/storage

# 监听地址（允许外网访问）
listen: 0.0.0.0:4873
```

### 5. 安装 PM2

```bash
npm install -g pm2
```

### 6. 启动 Verdaccio

```bash
pm2 start /usr/local/bin/verdaccio --name verdaccio
```

### 7. 配置开机自启

```bash
pm2 save
pm2 startup
```

执行 `pm2 startup` 返回的命令。

## 使用指南

### 创建用户

在本地电脑执行：

```bash
npm adduser --registry http://43.252.229.237:4873
```

按提示输入用户名、密码和邮箱。

### 配置 .npmrc

在项目根目录创建 `.npmrc` 文件：

```bash
# 私有包使用私仓
@likaikai:registry=http://43.252.229.237:4873

# 公共包使用官方源
registry=https://registry.npmjs.org
```

**说明**：

- 所有 `@likaikai/xxx` 开头的包会从私仓安装
- 其他包从 npm 官方源安装

### 发布包

1. 确保 `package.json` 中包名带有 scope：

```json
{
  "name": "@likaikai/your-package-name",
  "version": "1.0.0"
}
```

2. 发布到私仓：

```bash
npm publish
```

### 安装私有包

```bash
# 方式 1：使用 .npmrc 配置
npm install @likaikai/package-name

# 方式 2：临时指定仓库
npm install @likaikai/package-name --registry http://43.252.229.237:4873
```

### 查看已发布的包

浏览器访问：`http://43.252.229.237:4873`

## 常见问题

### 1. 无法访问私仓网页

**问题**：访问 `http://43.252.229.237:4873` 显示无法连接

**解决方案**：

- 检查 Verdaccio 是否运行：`pm2 list`
- 检查配置文件中 `listen` 是否设置为 `0.0.0.0:4873`
- 检查云服务器安全组是否开放 4873 端口

### 2. 发布失败：未登录

**问题**：`npm publish` 提示需要登录

**解决方案**：

```bash
npm login --registry http://43.252.229.237:4873
```

### 3. 找不到私有包

**问题**：安装时提示找不到包

**解决方案**：

- 确认包名正确（包含 `@likaikai/` 前缀）
- 检查 `.npmrc` 配置是否正确
- 确认包已成功发布到私仓

### 4. 配置文件格式错误

**问题**：Verdaccio 启动失败，显示 `cannot open config file`

**解决方案**：

- 检查 YAML 格式是否正确（注意冒号后面要有空格）
- 使用 `cat /root/verdaccio/config.yaml` 检查内容

## 维护管理

### PM2 常用命令

```bash
# 查看进程状态
pm2 list

# 查看日志
pm2 logs verdaccio

# 重启服务
pm2 restart verdaccio

# 停止服务
pm2 stop verdaccio

# 查看详细信息
pm2 show verdaccio
```

### 备份与恢复

#### 备份包数据

```bash
# 备份存储目录
tar -czf verdaccio-backup-$(date +%Y%m%d).tar.gz /verdaccio/storage

# 备份用户数据
cp /root/verdaccio/htpasswd /root/verdaccio/htpasswd.backup
```

#### 恢复数据

```bash
# 恢复存储目录
tar -xzf verdaccio-backup-YYYYMMDD.tar.gz -C /

# 恢复用户数据
cp /root/verdaccio/htpasswd.backup /root/verdaccio/htpasswd

# 重启服务
pm2 restart verdaccio
```

### 更新 Verdaccio

```bash
# 停止服务
pm2 stop verdaccio

# 更新 Verdaccio
npm update -g verdaccio

# 启动服务
pm2 start verdaccio
```

### 查看包存储

```bash
# 查看所有已发布的包
ls -la /verdaccio/storage/

# 查看特定 scope 下的包
ls -la /verdaccio/storage/@likaikai/
```

## 安全建议

1. **修改服务器密码**：定期更换 root 密码
2. **配置 HTTPS**：生产环境建议配置 SSL 证书
3. **访问控制**：配置白名单限制访问 IP
4. **定期备份**：建立自动备份机制
5. **监控告警**：配置服务监控和告警

## 相关链接

- [Verdaccio 官方文档](https://verdaccio.org/zh-cn/)
- [PM2 官方文档](https://pm2.keymetrics.io/)
- [npm 官方文档](https://docs.npmjs.com/)

## 已发布的包

- `@likaikai/my-ai-monorepo@1.1.2` - 第一个测试包

## 更新日志

- **2025-09-21**：完成 Verdaccio 部署和 PM2 配置
- **2025-09-20**：创建首个私有包并发布

---

**维护人员**：@likaikai  
**最后更新**：2025-09-21
