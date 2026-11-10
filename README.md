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

### GitHub 仓库（已上传）
`https://github.com/amjack/meili-awakening`（PUBLIC，main 分支）

### Vercel 部署步骤
1. 登录 [vercel.com](https://vercel.com) → **Add New… → Project** → **Import** 选择 `amjack/meili-awakening`
2. Framework Preset 选 **Other**（纯静态零构建，无需任何配置，根目录 `index.html` 即入口）
3. **Deploy** → 约 10 秒完成，获得 `xxx.vercel.app` 链接
4. 绑定自定义域名：**Settings → Domains → Add** `mei.ssstool.cn`
   - Vercel 会提示添加 DNS 记录，去 DNSPod（ssstool.cn 的 NS 在 DNSPod）加：
     ` mei  CNAME  cname.vercel-dns.com`
   - 与 fang.ssstool.cn 同款路径，证书由 Vercel 自动签发

### 本机现有基础设施（2026-11-10 实测）
| 域名 | 托管方式 |
| --- | --- |
| ssstool.cn / www | 腾讯云 CDN（dnsv1.com） |
| nukorhome.com | CloudBase 静态托管（tcbaccess.tencentcloudbase.com） |
| fang.ssstool.cn | Vercel（cname.vercel-dns.com） |
| ssstool.cn DNS | DNSPod（clark/optional.dnspod.net） |

### 更新流程
改完 `meili/` 内文件后：
```bash
cd meili && git add -A && git commit -m "更新说明" && git push
# Vercel 已绑定该仓库会自动重新部署
```
