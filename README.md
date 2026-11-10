# 美力觉醒 · 品牌品宣站（H5）

单页品宣网站，宣传「美力觉醒」是做什么的、为什么选择我们。
黑底白线的高冷画廊风：极简、艺术、大留白、衬线大标题、滚动渐显。

## 文件

```
meili/
├── index.html              # 全部内容（结构 + 样式 + 脚本，零依赖单文件）
└── assets/
    ├── logo@2x.png         # Logo（黑底已抠透明，1024px）
    ├── logo.png            # Logo 小尺寸透明版
    ├── logo-src.png        # 原始备选图（黑底，留存备用）
    ├── favicon.png         # 站点图标
    └── og-cover.jpg        # 微信/社交分享封面（1200×630）
```

## 本地预览

```bash
cd meili && python3 -m http.server 8899
# 浏览器打开 http://localhost:8899
```

## 上线前必须修改

`index.html` 底部 CTA 区域有两个占位按钮（已标 `TODO`）：
- `#btn-wechat` → 真实微信号（或改为企业微信二维码图片）
- `#btn-mail` → 真实邮箱（`mailto:` 链接）

## 重新生成 Logo 资产

原图为黑底 PNG，已用亮度抠图转透明。换新 Logo 后重跑：

```bash
python3 - <<'PY'
from PIL import Image, ImageFilter
logo = Image.open('assets/logo-src.png').convert('RGBA')
# （抠图逻辑见 git 历史 / 询问 AI）
PY
```

## 部署

### 当前状态（2026-11-10）
已通过 WorkBuddy 发布应用部署到云端，访问链接：
`https://meili-awakening.app.workbuddy.host/?sharecode=MPTFRF4U7C`

### 自定义域名 mei.ssstool.cn（待完成）
发布平台的 TLS 证书只覆盖 `*.app.workbuddy.host`，**不能直接 CNAME 自定义域名**（浏览器证书校验不通过）。
需要以下两种方案之一：

**方案 A：CloudBase 静态托管（推荐，与 nukorhome.com 同款）**
```bash
# 1. 用托管 nukorhome.com 的腾讯云账号登录（本机当前 tcb 登录的是另一个账号）
tcb login
# 2. 进入项目目录，上传静态文件（envId 换成实际环境）
cd /Applications/zhangjunjie/fang-cal/meili
tcb hosting deploy ./ -e <envId>
# 3. 云开发控制台 → 静态托管 → 域名绑定 → 添加 mei.ssstool.cn（自动签发证书）
# 4. DNSPod（ssstool.cn 的 NS 在 DNSPod）添加 CNAME：
#    主机记录 mei → 目标 <envId 对应的 tcbaccess 域名，控制台绑定页会给出>
```

**方案 B：Vercel（与 fang.ssstool.cn 同款）**
```bash
# meili/ 为纯静态零构建，直接作为项目根目录部署
vercel deploy --prod
# Vercel 项目设置 → Domains → 添加 mei.ssstool.cn
# DNSPod 添加 CNAME：mei → cname.vercel-dns.com
```

### 本机现有基础设施（2026-11-10 实测）
| 域名 | 托管方式 |
| --- | --- |
| ssstool.cn / www | 腾讯云 CDN（dnsv1.com） |
| nukorhome.com | CloudBase 静态托管（tcbaccess.tencentcloudbase.com） |
| fang.ssstool.cn | Vercel（cname.vercel-dns.com） |
| ssstool.cn DNS | DNSPod（clark/optional.dnspod.net） |
