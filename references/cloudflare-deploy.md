# Cloudflare Pages 部署指南

## 项目结构

```
prototype/
├── index.html          # 原型页面
└── (其他静态资源)
```

## 前置准备：API Token 配置

在服务器/CI 环境中部署（无法交互式登录）时，需要提前配置 API Token。

### 创建 API Token

1. 访问 https://dash.cloudflare.com/profile/api-tokens
2. 点击 **Create Token** → **Custom Token**
3. 配置以下权限：

| 权限 | 资源范围 | 说明 |
|------|---------|------|
| `Cloudflare Pages:Edit` | Account | 创建/更新 Pages 项目（必须） |
| `Account Settings:Read` | Account | 读取账户信息（必须） |
| `Zone:Read` | All Zones | 自定义域名时需要 |
| `DNS:Edit` | 指定 Zone | 配置 DNS 绑定域名时需要 |

4. 创建后复制 Token（只显示一次）

### 获取 Account ID

在 Cloudflare Dashboard 右侧边栏可以看到 **Account ID**，复制备用。

### 配置环境变量

```bash
export CLOUDFLARE_API_TOKEN="your-api-token"
export CLOUDFLARE_ACCOUNT_ID="your-account-id"
```

或写入 `.env` 文件（注意不要提交到 Git）：

```
CLOUDFLARE_API_TOKEN=your-api-token
CLOUDFLARE_ACCOUNT_ID=your-account-id
```

配置好后，`wrangler` 会自动读取这两个环境变量，无需交互式登录。

---

## 部署方式

### 方式1：Wrangler CLI（推荐）

```bash
# 安装 Wrangler
npm install -g wrangler

# 本地开发：交互式登录（浏览器授权）
wrangler login

# 服务器/CI：使用 API Token（见上方前置准备）
# 确保已设置 CLOUDFLARE_API_TOKEN 和 CLOUDFLARE_ACCOUNT_ID

# 部署到 Cloudflare Pages
cd prototype
wrangler pages deploy . --project-name=your-project-name
```

### 方式2：Cloudflare Dashboard

1. 访问 https://dash.cloudflare.com
2. 进入 **Workers & Pages** → **Create**
3. 选择 **Pages** → **Upload assets**
4. 拖入 `prototype` 文件夹
5. 设置项目名称，点击部署

### 方式3：连接 Git 仓库

1. 访问 https://dash.cloudflare.com
2. 进入 **Workers & Pages** → **Create**
3. 选择 **Pages** → **Connect to Git**
4. 选择 GitHub 仓库
5. 构建设置：
   - **Build command**: 留空（静态站点）
   - **Build output directory**: `/` 或 `public`
6. 点击 **Save and Deploy**

## 环境变量（可选）

如需配置环境变量：

1. 进入项目设置 → **Environment variables**
2. 添加变量（如 `API_URL`）
3. 重新部署生效

## 自定义域名（可选）

1. 进入项目设置 → **Custom domains**
2. 点击 **Set up a custom domain**
3. 输入你的域名
4. 按提示配置 DNS 记录

## 常见问题

### 1. 404 错误
确保 `index.html` 在根目录

### 2. 路由问题
添加 `_redirects` 文件：
```
/*    /index.html   200
```

### 3. 缓存问题
Cloudflare 自动处理缓存，无需额外配置

## 优势

- ✅ **国内可访问** - 无需科学上网
- ✅ **免费额度充足** - 每月 500 次构建
- ✅ **自动 HTTPS** - 免费 SSL 证书
- ✅ **全球 CDN** - 访问速度快
- ✅ **无需备案** - pages.dev 域名

## 示例地址

部署后会获得类似地址：
```
https://your-project.pages.dev
https://abc12345.your-project.pages.dev
```
