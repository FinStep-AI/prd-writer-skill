# PRD Writer Skill

AI Agent 技能：撰写产品需求文档(PRD) + 生成可交互原型。

## 功能

- 📋 从会议记录/需求描述提取结构化需求
- 📝 生成完整的 PRD 文档（Markdown 格式）
- 🎨 集成 UI-UX-Pro-Max 设计系统
- 🖥️ 生成单文件 HTML 可交互原型
- 🚀 支持 Cloudflare Pages 一键部署

## 安装

```bash
clawhub install prd-writer
```

或手动克隆到你的 skills 目录：

```bash
git clone https://github.com/FinStep-AI/prd-writer-skill.git ~/clawd/skills/prd-writer
```

## 触发条件

当用户提到以下关键词时自动触发：
- "需求文档"、"PRD"、"产品需求"、"写需求"
- "原型"、"feature list"

## 目录结构

```
prd-writer/
├── SKILL.md                 # 技能主文件
├── references/              # 参考文档
│   ├── prd-template.md      # PRD 模板
│   ├── feature-list-template.md
│   ├── prototype-guide.md   # 原型生成指南
│   ├── quality-checklist.md # 质量检查清单
│   └── ...
└── scripts/                 # 可执行脚本
    └── ui-ux-pro-max/       # 设计系统搜索引擎
        ├── search.py        # 主入口
        └── data/            # 设计数据 CSV
```

## 使用示例

```
用户: 帮我写一个电商小程序的 PRD

Agent: [自动触发 prd-writer 技能]
       1. 采集需求
       2. 生成 Feature List
       3. 生成 PRD 文档
       4. 生成设计系统
       5. 生成 HTML 原型
```

## 设计系统搜索

```bash
# 生成完整设计系统推荐
python3 scripts/ui-ux-pro-max/search.py "SaaS dashboard" --design-system -p "项目名"

# 搜索配色方案
python3 scripts/ui-ux-pro-max/search.py "fintech" --domain color

# 搜索字体配对
python3 scripts/ui-ux-pro-max/search.py "modern elegant" --domain typography
```

## 部署到 Cloudflare Pages

原型生成后可一键部署到 Cloudflare Pages（国内可访问、免费、自动 HTTPS）。

### 1. 创建 API Token

访问 https://dash.cloudflare.com/profile/api-tokens → **Create Token** → **Custom Token**，配置以下权限：

| 权限 | 资源范围 | 是否必须 |
|------|---------|---------|
| `Cloudflare Pages:Edit` | Account | ✅ 必须 |
| `Account Settings:Read` | Account | ✅ 必须 |
| `Zone:Read` | All Zones | 自定义域名时需要 |
| `DNS:Edit` | 指定 Zone | 绑定域名时需要 |

同时在 Dashboard 右侧边栏复制你的 **Account ID**。

### 2. 配置环境变量

```bash
export CLOUDFLARE_API_TOKEN="your-api-token"
export CLOUDFLARE_ACCOUNT_ID="your-account-id"
```

### 3. 部署

```bash
npm install -g wrangler
cd prototype
wrangler pages deploy . --project-name=your-project-name
```

部署成功后获得访问地址：`https://your-project.pages.dev`

> 详细说明见 [references/cloudflare-deploy.md](references/cloudflare-deploy.md)

## License

MIT
