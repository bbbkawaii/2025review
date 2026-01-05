# 部署到 Vercel 并配置自定义域名指南

本指南将帮助你将这个年终总结页面部署到 Vercel，并配置自己的域名。

## 前置要求

1. 一个 GitHub 账号
2. 一个 [Vercel](https://vercel.com) 账号（可以使用 GitHub 账号登录）
3. 一个已购买的域名（如果要使用自定义域名）

## 步骤 1: 准备代码仓库

1. 确保你的代码已经推送到 GitHub 仓库
2. 项目应该包含以下文件：
   - `index.html` - 主页面
   - `FZPingXYSK.TTF` - 字体文件
   - `vercel.json` - Vercel 配置文件

## 步骤 2: 部署到 Vercel

### 方法一：通过 Vercel 网站部署（推荐）

1. **登录 Vercel**
   - 访问 [vercel.com](https://vercel.com)
   - 点击 "Sign Up" 或 "Log In"
   - 选择使用 GitHub 账号登录

2. **导入项目**
   - 登录后，点击 "Add New..." → "Project"
   - 选择 "Import Git Repository"
   - 找到并选择你的 `2025review` 仓库
   - 点击 "Import"

3. **配置项目**
   - **Project Name**: 可以自定义项目名称（例如：`my-2025-review`）
   - **Framework Preset**: 选择 "Other"（因为这是纯静态 HTML）
   - **Root Directory**: 保持默认 `./`
   - **Build and Output Settings**: 保持默认即可

4. **部署**
   - 点击 "Deploy" 按钮
   - 等待部署完成（通常需要 1-2 分钟）
   - 部署成功后，你会获得一个 `https://your-project-name.vercel.app` 的地址

### 方法二：通过 Vercel CLI 部署

1. **安装 Vercel CLI**
   ```bash
   npm install -g vercel
   ```

2. **登录**
   ```bash
   vercel login
   ```

3. **部署项目**
   ```bash
   cd /path/to/2025review
   vercel
   ```

4. **生产环境部署**
   ```bash
   vercel --prod
   ```

## 步骤 3: 配置自定义域名

### 3.1 在 Vercel 中添加域名

1. **进入项目设置**
   - 在 Vercel 控制台中，进入你的项目
   - 点击顶部的 "Settings" 标签
   - 在左侧菜单中选择 "Domains"

2. **添加域名**
   - 在 "Add" 输入框中输入你的域名，例如：
     - `example.com`（顶级域名）
     - `www.example.com`（子域名）
     - `2025.example.com`（自定义子域名）
   - 点击 "Add" 按钮

3. **查看 DNS 配置要求**
   - Vercel 会显示需要添加的 DNS 记录
   - 记录下这些信息（下一步需要用到）

### 3.2 配置 DNS 记录

根据你的域名类型，需要添加不同的 DNS 记录：

#### 方案 A：顶级域名（example.com）

在你的域名服务商（如阿里云、腾讯云、GoDaddy、Namecheap 等）的 DNS 管理页面添加以下记录：

```
类型: A
名称: @
值: 76.76.21.21
TTL: 3600
```

#### 方案 B：子域名（www.example.com 或 2025.example.com）

```
类型: CNAME
名称: www（或你的子域名前缀，如 2025）
值: cname.vercel-dns.com
TTL: 3600
```

#### 推荐配置（同时支持带 www 和不带 www）

1. 为顶级域名添加 A 记录：
   ```
   类型: A
   名称: @
   值: 76.76.21.21
   ```

2. 为 www 子域名添加 CNAME 记录：
   ```
   类型: CNAME
   名称: www
   值: cname.vercel-dns.com
   ```

### 3.3 常见域名服务商配置指南

#### 阿里云

1. 登录 [阿里云控制台](https://dns.console.aliyun.com/)
2. 进入 "域名解析"
3. 找到你的域名，点击 "解析设置"
4. 点击 "添加记录"
5. 填入相应的记录类型、主机记录和记录值
6. 点击 "确认"

#### 腾讯云 DNSPod

1. 登录 [DNSPod 控制台](https://console.dnspod.cn/)
2. 找到你的域名，点击 "解析"
3. 点击 "添加记录"
4. 填入相应的记录类型、主机记录和记录值
5. 点击 "保存"

#### Cloudflare

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. 选择你的域名
3. 进入 "DNS" 标签
4. 点击 "Add record"
5. 填入相应的记录类型、名称和值
6. 点击 "Save"

### 3.4 验证域名配置

1. **等待 DNS 传播**
   - DNS 更改可能需要几分钟到 48 小时才能生效
   - 通常在 10-30 分钟内就能生效

2. **检查 DNS 记录**
   - 使用在线工具检查 DNS 是否生效：
     - [DNSChecker](https://dnschecker.org/)
     - [What's My DNS](https://whatsmydns.net/)

3. **在 Vercel 中验证**
   - 返回 Vercel 项目的 Domains 页面
   - 域名状态应该从 "Invalid Configuration" 变为 "Valid Configuration"
   - Vercel 会自动为你的域名配置 SSL 证书（HTTPS）

## 步骤 4: 测试访问

1. 打开浏览器，访问你的自定义域名
2. 检查页面是否正常显示
3. 检查字体是否正确加载
4. 检查移动端显示是否正常

## 常见问题

### 1. 域名显示 "Invalid Configuration"

**解决方法：**
- 检查 DNS 记录是否正确添加
- 等待 DNS 传播（通常需要 10-30 分钟）
- 使用 DNS 检查工具验证记录是否生效

### 2. 页面显示 404

**解决方法：**
- 确保 `index.html` 文件在项目根目录
- 检查 Vercel 部署日志是否有错误
- 尝试重新部署项目

### 3. 字体无法加载

**解决方法：**
- 确保 `FZPingXYSK.TTF` 文件存在
- 检查 `vercel.json` 配置是否正确
- 查看浏览器控制台是否有 CORS 错误

### 4. HTTPS 证书问题

**解决方法：**
- Vercel 会自动配置 SSL 证书
- 如果证书未自动配置，等待几分钟后刷新
- 确保域名的 DNS 记录正确

## 自动部署

配置完成后，每次你向 GitHub 仓库推送代码，Vercel 都会自动部署最新版本：

1. 修改代码
2. 提交到 Git：`git commit -am "更新内容"`
3. 推送到 GitHub：`git push`
4. Vercel 会自动检测并部署新版本

## 其他资源

- [Vercel 官方文档](https://vercel.com/docs)
- [Vercel 域名配置文档](https://vercel.com/docs/concepts/projects/domains)
- [Vercel CLI 文档](https://vercel.com/docs/cli)

## 需要帮助？

如果在部署过程中遇到问题，可以：
- 查看 [Vercel 社区论坛](https://github.com/vercel/vercel/discussions)
- 查看 Vercel 项目的部署日志
- 检查浏览器控制台的错误信息
