# ACEM Capital 官网

静态营销网站，支持中英文切换。

## 文件结构

```
acemcapital-website/
├── index.html        # 英文首页（默认）
├── zh/
│   └── index.html    # 中文首页
└── README.md
```

URL 对应关系：
- `acemcapital.hk/` → 英文
- `acemcapital.hk/zh/` → 中文

## 本地预览

直接双击 `index.html` 即可在浏览器打开，或者在终端：

```bash
cd /Users/kehuang/Desktop/acemcapital-website
open index.html
```

修改文案后刷新浏览器即可看到效果。

## 部署到线上（推荐：Cloudflare Pages，永久免费）

### 第一步：注册 Cloudflare 账号
访问 https://pages.cloudflare.com，用邮箱注册。

### 第二步：上传站点
1. 进入 Cloudflare Pages → **Create a project** → **Direct Upload**
2. 项目名填 `acemcapital`（决定临时 URL：`acemcapital.pages.dev`）
3. **把整个 `acemcapital-website` 文件夹拖进上传区**
4. 点击 **Deploy site**
5. 等 30 秒，会得到一个临时地址 `acemcapital.pages.dev`，先在这里检查页面是否正常

### 第三步：绑定自有域名
1. 项目详情页 → **Custom domains** → **Set up a custom domain**
2. 填入 `acemcapital.hk` → 继续
3. 再加一条 `www.acemcapital.hk`
4. Cloudflare 会显示 DNS 配置说明（具体的 CNAME 值）

### 第四步：到阿里云配置 DNS
登录阿里云 → 云解析 DNS → 找到 `acemcapital.hk` → 添加记录：

| 类型 | 主机记录 | 记录值 | 说明 |
|---|---|---|---|
| CNAME | `@` | `acemcapital.pages.dev` | 根域名指向 Cloudflare |
| CNAME | `www` | `acemcapital.pages.dev` | www 子域也指向 |

> 注：阿里云的根域名（`@`）CNAME 用"默认线路"即可。如果阿里云不允许 `@` 用 CNAME，改用 Cloudflare 提供的 A 记录。

### 第五步：等待生效
- DNS 生效一般 10 分钟内
- Cloudflare 会自动签发 SSL 证书（10-30 分钟）
- 完成后 `https://acemcapital.hk` 即可访问

## 修改内容

### 修改文字
- 英文：编辑 `index.html`
- 中文：编辑 `zh/index.html`
- 用 VS Code / Sublime 打开，搜索要改的文字，直接替换

### 修改颜色
两个 HTML 文件顶部都有这段配置：

```javascript
colors: {
  terracotta: { DEFAULT: '#B85C38', ... },  // 主色（陶土红）
  gold: { DEFAULT: '#C9A961', ... },        // 强调色（古铜金）
  parchment: '#F5E6D3',                      // 暖底色
  espresso: { DEFAULT: '#2C1810', ... },     // 文字色（深棕墨）
  sand: '#FAF6F0',                           // 背景色（沙漠白）
}
```

改 HEX 值即可全站换色。

### 添加新业务/优势
找到对应的 `<article>` 或 `<div>` 复制一份改文字即可。

### 重新部署
修改后回到 Cloudflare Pages → 项目 → **Create new deployment** → 拖文件夹重新上传。

## 邮件 mailto 链接

页面上的邮箱点击会直接打开邮件客户端。
邮箱地址：`kenzhang@acemcapital.hk`

修改邮箱：在 `index.html` 和 `zh/index.html` 里搜索 `kenzhang@acemcapital.hk`，全部替换。

## 重要注意事项

1. **网站和邮箱共存**：阿里云 DNS 里的 MX 记录是邮箱用的，不要删。网站只需要加 CNAME 记录（主机记录 `@` 和 `www`），不冲突。
2. **SSL 证书**：Cloudflare 自动配置，不用买。
3. **备案**：`.hk` 域名 + 海外托管，不需要中国大陆备案。

## 后续优化建议

- 加 favicon（小图标）：放一张 `favicon.ico` 在根目录
- 加 OG 图（社交分享缩略图）：用 Figma/Canva 做一张 1200×630 的图，在 HTML 里加 `<meta property="og:image" content="...">`
- 接入 Google Analytics / Plausible 统计访问量
- 添加 Cookie 通知（如果以后做欧洲市场要符合 GDPR）
