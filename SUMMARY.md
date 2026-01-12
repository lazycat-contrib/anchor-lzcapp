# Anchor 应用转换完成

## 📦 生成的文件

```
anchor-lzcapp/
├── lzc-manifest.yml    ✅ 应用配置文件（v1.4.1+）
├── lzc-build.yml       ✅ 构建配置文件
├── build.sh            ✅ 自动化构建脚本（可执行）
└── README.md           ✅ 用户指南
```

## 🎯 应用特点

### 应用类型
- **类型**: Web 应用（HTTP 服务）
- **复杂度**: 简单应用（零配置）
- **端口**: 3000

### 配置特点
- ✅ **零用户配置**: 无需 lzc-deploy-params.yml
- ✅ **upstreams 路由**: 使用推荐的 upstreams 配置
- ✅ **public_path**: 自动添加根路径访问
- ✅ **多语言支持**: 英文 + 中文
- ✅ **持久化存储**: /lzcapp/var/data

### 资源配置
- **CPU**: 512 shares
- **内存**: 512M

## 🚀 快速开始

### 必需步骤

**1. 准备图标文件**
```bash
# 需要一个 512x512 PNG 格式的图标
# 保存为: icon.png
```

### 本地测试

```bash
cd anchor-lzcapp

# 方式一：使用自动化脚本
./build.sh
# 选择 1 - 构建应用

# 然后安装
lzc-cli app install anchor-1.0.0.lpk
```

### 发布到商店

```bash
# 方式一：一键发布（推荐）
./build.sh
# 选择 4 - 一键构建+镜像复制+发布

# 方式二：手动步骤
lzc-cli appstore login
lzc-cli appstore copy-image ghcr.io/zhfahim/anchor:latest
# 更新 lzc-manifest.yml 中的镜像
lzc-cli project build -o anchor-1.0.0.lpk
lzc-cli appstore publish anchor-1.0.0.lpk
```

## 📋 转换详情

### Docker Compose → LazyCat 映射

| Docker 配置 | LazyCat 配置 | 说明 |
|------------|-------------|------|
| `ports: 3000:3000` | `upstreams: [{location: "/", backend: "http://anchor:3000/"}]` | HTTP 路由 |
| `volumes: anchor_data:/data` | `binds: ["/lzcapp/var/data:/data"]` | 持久化存储 |
| `image: ghcr.io/zhfahim/anchor:latest` | `image: ghcr.io/zhfahim/anchor:latest` | 镜像（需复制到懒猫仓库） |
| `restart: unless-stopped` | 自动管理 | 懒猫自动处理 |

### 配置优化

**应用的简化：**
1. ❌ **未添加 healthcheck**: 原配置无 healthcheck，遵循最佳实践不添加
2. ✅ **跳过 deploy-params**: 零配置应用，无需设置向导
3. ✅ **使用 upstreams**: 采用推荐的现代路由配置
4. ✅ **自动资源限制**: 添加合理的 CPU/内存限制

## 🔍 配置说明

### lzc-manifest.yml 关键配置

```yaml
application:
  subdomain: anchor           # 访问地址: anchor.your-domain
  background_task: false      # Web 应用，非后台任务
  public_path: [/]           # 根路径公开访问
  upstreams:                 # 推荐的路由配置
    - location: /
      backend: http://anchor:3000/

services:
  anchor:
    image: ghcr.io/zhfahim/anchor:latest
    binds:
      - /lzcapp/var/data:/data  # 持久化数据
    cpu_shares: 512             # CPU 限制
    mem_limit: 512M             # 内存限制
```

### 存储路径

- **数据目录**: `/lzcapp/var/data` → 容器内 `/data`
- **类型**: 持久化存储（重启不丢失）

## ⚠️ 重要提醒

### 1. 图标文件（必需）
```bash
# 必须提供 icon.png
# 规格：512x512 像素，PNG 格式
```

### 2. 镜像复制（发布时必需）
```bash
# 发布前必须将镜像复制到懒猫仓库
lzc-cli appstore copy-image ghcr.io/zhfahim/anchor:latest

# 然后更新 lzc-manifest.yml 中的镜像地址
# 原镜像会被自动注释保留
```

### 3. 版本管理
```yaml
# 首次发布
version: 1.0.0

# 后续更新需递增版本号
version: 1.0.1
version: 1.0.2
```

## 📊 build.sh 功能

自动化脚本提供以下功能：

1. **📦 构建应用** - 生成 LPK 包
2. **🔧 镜像复制** - 复制到懒猫仓库并自动更新 manifest
3. **📤 发布应用** - 提交到应用商店审核
4. **🚀 一键发布** - 完整自动化流程（4个阶段）
5. **📋 查看信息** - 显示应用配置详情

### 一键发布流程

```
阶段 1: 初始构建（原始镜像）
  ↓
阶段 2: 镜像复制（自动更新 manifest）
  ↓
阶段 3: 重新构建（新镜像）
  ↓
阶段 4: 发布审核（1-3天）
```

## 🎓 最佳实践

### ✅ 正确做法

1. **使用 upstreams** - 现代推荐的路由配置
2. **不添加不必要的 healthcheck** - 避免启动失败
3. **使用 /lzcapp 路径** - 正确的持久化存储
4. **版本号递增** - 更新时正确管理版本

### ❌ 避免错误

1. ❌ 使用公共镜像而不复制到懒猫仓库
2. ❌ 给没有健康检查工具的容器添加 healthcheck
3. ❌ 使用相对路径或主机路径作为 binds
4. ❌ 更新时忘记递增版本号

## 📈 下一步

### 本地测试
1. 准备 icon.png
2. 运行 `./build.sh` → 选择 1（构建）
3. 安装测试：`lzc-cli app install anchor-1.0.0.lpk`
4. 访问测试

### 发布商店
1. 登录：`lzc-cli appstore login`
2. 一键发布：`./build.sh` → 选择 4
3. 等待审核：1-3 天
4. 审核通过后在商店上架

## 🔗 相关资源

- **懒猫开发者文档**: https://developer.lazycat.cloud
- **Anchor 项目**: https://github.com/ZhFahim/anchor
- **应用商店**: 懒猫云应用商店

## ✅ 转换完成清单

- [x] lzc-manifest.yml（v1.4.1+ 格式）
- [x] lzc-build.yml
- [x] build.sh（自动化脚本）
- [x] README.md（用户指南）
- [x] 权限设置（build.sh 可执行）
- [x] 配置验证（YAML 格式正确）
- [ ] icon.png（需要用户提供）

---

**转换工具**: 懒猫应用发布技能 v1.4.1+
**转换时间**: 2026-01-12
**应用版本**: 1.0.0
