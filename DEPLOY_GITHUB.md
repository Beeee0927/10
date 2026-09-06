# 翼灵工作室网站 · GitHub Pages 免费部署教程

> 目标：用 GitHub Pages 托管这套纯静态站，先跑起来看看效果。
> 方案：GitHub Pages（免费、免备案、无需碰阿里云账号）。
> ⚠️ 国内访问说明：github.io 在国内经常**打不开或很慢**，此方案适合「先体验验证」。
> 若要国内流畅，见同目录《DEPLOY.md》（OSS 香港方案），代码完全不用改即可切换。

---

## 整体流程（5 分钟跑通）

1. GitHub 上建仓库
2. 本地代码 push 上去
3. 开启 GitHub Pages
4. 访问 `https://<你的用户名>.github.io/...`
5. （可选）绑定自定义域名 yiling10.xyz

---

## 第 0 步：确认本地已初始化 git 并提交

本地我已经帮你处理好 git（含排除 `10.zip`）。如果你想自己重来：

```bash
cd "F:\work\trae-work\6a9c12254f8e2a832abf58f5"
git init
git add .
git commit -m "chore: initial site"
```

.gitignore 已默认排除 `10.zip`（那个 9MB 压缩备份包，不用进 git）。

---

## 第 1 步：在 GitHub 创建仓库（两种仓库名，选一种）

> 仓库名决定访问路径，强烈建议用第一种。

### 方式 A：【推荐】用户主页仓库 → 裸域可直接访问
- 仓库名必须严格等于：**`<你的GitHub用户名>.github.io`**
  - 例：用户名是 `along` → 仓库名 `along.github.io`
- 好处：访问地址是 `https://<用户名>.github.io`（不带子路径），将来绑 `yiling10.xyz` 裸域也最顺。

### 方式 B：普通仓库（如 `yiling-website`）
- 访问地址会带仓库名：`https://<用户名>.github.io/yiling-website`
- 图片/脚本用相对路径所以渲染正常，但绑裸域时裸域会指到 `/` 根，可能 404。仅用于试跑可以用。

**建仓库操作**：github.com → 右上角「+」→ New repository → 填仓库名 → 选 **Public** → 不要勾选 "Add a README file" → Create repository。

---

## 第 2 步：推送本地代码到 GitHub

```bash
cd "F:\work\trae-work\6a9c12254f8e2a832abf58f5"

# 换成你自己的用户名和仓库名
git remote add origin https://github.com/<你的用户名>/<仓库名>.git
git branch -M main
git push -u origin main
```

推送时 GitHub 要登录：
- 网页方式：浏览器会自动弹授权页，直接点 Authorize。
- 若要求输入：用户名 + **Personal Access Token**（不是密码）。
  - 生成令牌：GitHub → Settings → Developer settings → Personal access tokens → Generate new token
  - 勾选 `repo` 权限 → 生成 → 复制保存（只在生成时显示一次）

---

## 第 3 步：开启 GitHub Pages

1. 进入你的仓库 → 顶部「**Settings**」→ 左侧「**Pages**」
2. 「Build and deployment」→ 选择 **Deploy from a branch**
3. Branch 选 **main**，目录选 **/(root)** → 点 **Save**
4. 等 1~2 分钟刷新页面上方会出现访问地址：
   - 方式 A：`https://<你的用户名>.github.io`
   - 方式 B：`https://<你的用户名>.github.io/<仓库名>`

打开看看，翼灵页面出现即成功。✅

---

## 第 4 步（可选）：绑定自定义域名 yiling10.xyz

> Git 仓库需要放一个 **CNAME 文件**，内容写你的域名，且**必须提交**（别被 .gitignore 排除，我没在 .gitignore 里排除它）。

1. 在「Pages」→ «Custom domain» 处填入 `yiling10.xyz` → Save。
2. 仓库根目录新增文件 `CNAME`（无扩展名），内容一行：`yiling10.xyz`，提交并 push。
3. 到**阿里云云解析**（域名还留在阿里云管，不用迁移 NS）加解析记录：
   - **裸域** `yiling10.xyz` → 记录类型 **A**，记录值填以下任一 GitHub IP：
     ```
     185.199.108.153
     185.199.109.153
     185.199.110.153
     185.199.111.153
     ```
   - **www 子域** `www.yiling10.xyz` → 记录类型 **CNAME**，记录值填 `<你的用户名>.github.io`
4. 等 DNS 生效（几分钟），GitHub 会自动为自定义域名签发 **Let's Encrypt HTTPS 证书**。
5. 访问 `https://yiling10.xyz` 验证。

> 注意：绑了自定义域名后，用 GitHub 提供的 `https://<用户名>.github.io` 访问可能被重定向到你的域名，属正常。

---

## 常见坑速查

| 症状 | 原因 | 解决 |
|------|------|------|
| 页面图片不显示 | 图片路径用了绝对路径 `/images/...` | 我们代码用的是**相对路径**，正常；若你自己改了，确保是相对路径 |
| 404 | 仓库名是普通名，访问地址漏了 `/仓库名` | 加仓库名路径，或用方式 A 的 `.github.io` 仓库 |
| 绑定域名后打不开 | CNAME 文件没提交 / DNS 记录值填错 | 确保 CNAME 文件在仓库根并已 push；记录值别带 `http://` |
| push 时报错 403 / auth | 用了密码而非令牌 | 用 Personal Access Token |
| 国内打开极慢或超时 | github.io 被网络限制 | 这是平台硬伤，正式上线换 OSS（见 DEPLOY.md） |

---

## 之后想切换 OSS（国内流畅）

代码和文件完全不用改，只需要把 `index.html / styles.css / script.js / images/ / studio-bold-showcase/` 传到阿里云香港 OSS 桶，再绑域名即可——流程见同目录《DEPLOY.md》。

---

*教程基于 2026 年 GitHub Pages 常规流程整理；界面文案可能微调，以「要达成的目标」为准。*
