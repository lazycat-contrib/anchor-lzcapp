# Anchor - 懒猫应用

离线优先的自托管笔记应用

## 应用简介

Anchor 是一个注重隐私和离线优先的笔记应用，支持自托管部署。通过懒猫云平台，你可以轻松地在自己的服务器上运行 Anchor。

**主要特性：**
- 📝 离线优先 - 无需联网也能正常使用
- 🔒 隐私保护 - 数据完全掌控在自己手中
- 🚀 自托管 - 部署在自己的服务器上
- 💾 持久化存储 - 笔记数据安全保存

## 快速开始

### 本地测试安装

1. **准备图标文件**（必需）
   ```bash
   # 下载或创建 512x512 PNG 格式的图标
   # 保存为 icon.png
   ```

2. **构建应用包**
   ```bash
   chmod +x build.sh
   ./build.sh
   # 选择 1 - 构建应用
   ```

3. **本地安装**
   ```bash
   lzc-cli app install anchor-1.0.0.lpk
   ```

### 发布到应用商店

1. **登录懒猫应用商店**
   ```bash
   lzc-cli appstore login
   ```

2. **一键发布**
   ```bash
   ./build.sh
   # 选择 4 - 一键构建+镜像复制+发布
   ```

   脚本将自动完成：
   - ✅ 初始构建
   - ✅ 镜像复制到懒猫仓库
   - ✅ 自动更新 manifest
   - ✅ 重新构建
   - ✅ 提交审核

3. **等待审核**
   - 审核时间：1-3 天
   - 审核通过后即可在应用商店中找到

## 应用配置

### 基本信息

- **包名**: cloud.lazycat.app.anchor
- **版本**: 1.0.0
- **最低系统版本**: 1.3.8
- **访问端口**: 3000

### 存储配置

应用数据存储在 `/lzcapp/var/data` 路径下，确保数据持久化。

### 资源配置

- **CPU**: 512 shares
- **内存**: 512M

根据实际使用情况，你可以在 `lzc-manifest.yml` 中调整这些值。

## 文件说明

```
anchor-lzcapp/
├── lzc-manifest.yml    # 应用配置文件
├── lzc-build.yml       # 构建配置文件
├── build.sh            # 自动化构建脚本
├── icon.png            # 应用图标（需要提供）
└── README.md           # 本文件
```

### lzc-manifest.yml

应用的核心配置文件，包含：
- 应用元信息（名称、版本、描述等）
- 服务配置（镜像、环境变量、存储等）
- 路由配置（访问路径）
- 多语言支持

### lzc-build.yml

构建配置文件，指定：
- Manifest 文件路径
- 输出路径
- 图标文件路径

### build.sh

自动化脚本，提供以下功能：
1. 📦 构建应用包
2. 🔧 镜像复制到懒猫仓库
3. 📤 发布到应用商店
4. 🚀 一键完整发布流程
5. 📋 查看应用信息

## 使用指南

### 方式一：交互式菜单

```bash
./build.sh
```

然后根据菜单选择相应操作。

### 方式二：手动步骤

```bash
# 1. 构建应用
lzc-cli project build -o anchor-1.0.0.lpk

# 2. 复制镜像（发布到商店时需要）
lzc-cli appstore copy-image ghcr.io/zhfahim/anchor:latest

# 3. 更新 manifest 中的镜像地址
# 将返回的新镜像地址更新到 lzc-manifest.yml

# 4. 重新构建
lzc-cli project build -o anchor-1.0.0.lpk

# 5. 发布
lzc-cli appstore publish anchor-1.0.0.lpk
```

## 更新应用

当需要发布新版本时：

1. **更新版本号**
   ```yaml
   # lzc-manifest.yml
   version: 1.0.1  # 从 1.0.0 升级
   ```

2. **如果镜像更新**
   ```bash
   # 复制新镜像
   lzc-cli appstore copy-image ghcr.io/zhfahim/anchor:new-version

   # 更新 lzc-manifest.yml 中的镜像地址
   ```

3. **构建并发布**
   ```bash
   ./build.sh
   # 选择 4 - 一键发布
   ```

## 常见问题

### 1. 缺少 icon.png

**错误**: 构建时提示缺少 icon.png

**解决**:
- 准备一个 512x512 像素的 PNG 格式图标
- 保存为 `icon.png` 并放在应用目录中

### 2. 构建失败

**检查清单**:
- ✅ 所有必需文件都存在
- ✅ YAML 格式正确
- ✅ 图标文件格式正确（PNG, 512x512）

### 3. 发布失败

**可能原因**:
- 未登录应用商店
- 镜像未复制到懒猫仓库
- 网络连接问题

**解决步骤**:
```bash
# 1. 确认登录状态
lzc-cli appstore login

# 2. 检查镜像是否已复制
grep "registry.lazycat.cloud" lzc-manifest.yml

# 3. 如果没有，先复制镜像
./build.sh
# 选择 2 - 镜像复制
```

## 技术支持

### 官方文档
- 懒猫开发者文档: https://developer.lazycat.cloud
- Anchor 项目: https://github.com/zhfahim/anchor

### 相关命令

```bash
# 查看已安装的应用
lzc-cli app list

# 卸载应用
lzc-cli app uninstall cloud.lazycat.app.anchor

# 查看应用日志
lzc-cli app logs cloud.lazycat.app.anchor

# 重启应用
lzc-cli app restart cloud.lazycat.app.anchor
```

## 许可证

本应用配置遵循 MIT 许可证。

Anchor 应用本身的许可证请参考其官方仓库。

## 贡献

欢迎提交问题和改进建议！

---

**生成工具**: 懒猫应用发布技能 v1.4.1+
**生成时间**: 2026-01-12
# anchor-lzcapp
