# 长征 · 三维全景沙盘（独立网页版）

这是从 Harness 插件中抽出的独立静态站点，包含完整的三维长征地图、时间轴、省界、苏维埃区域、河流/湖泊、部队态势与兵力轴。无需后端，任何静态托管平台都可以直接部署。

## 目录

- `index.html` — 入口页
- `client.js` — 地图主体（与 `../long-march-atlas/client.js` 同步）
- `vendor/` — React 18 UMD（本地文件，不依赖 CDN）
- `icon.svg` — 站点图标
- `serve.cjs` — 本地静态服务器（仅预览用）
- `.github/workflows/pages.yml` — GitHub Pages 自动部署
- `netlify.toml` / `vercel.json` / `_headers` — 托管平台配置

## 本地预览

在本目录运行：

```powershell
node serve.cjs
```

打开 `http://127.0.0.1:4173/`。地图默认打开；关闭后可从底部按钮重新打开。

## 发布到公网

### 方案 A：GitHub Pages（推荐，推送后自动更新）

1. 新建 GitHub 仓库，把本目录的全部文件上传到仓库根目录。
2. 仓库 **Settings → Pages → Build and deployment → Source** 选择 **GitHub Actions**。
3. 之后每次 push 到 `main`，`.github/workflows/pages.yml` 会自动部署。
4. 页面地址：`https://<用户名>.github.io/<仓库名>/`。

### 方案 B：Netlify（拖拽即可上线）

1. 打开 <https://app.netlify.com/drop>，把本目录拖进去即可获得临时网址。
2. 需要自动更新时，把仓库连接到 Netlify；构建命令留空，发布目录填 `.`。

### 方案 C：Vercel

导入仓库，Framework 选择 **Other**，Build Command 留空，Output Directory 填 `.`。

### 方案 D：Cloudflare Pages

连接仓库，Build command 留空，Build output directory 填 `.`。

## 修改后如何同步

源文件是工作区里的 `long-march-atlas/client.js`。在**工作区根目录**运行：

```powershell
powershell -ExecutionPolicy Bypass -File sync-site.ps1
```

或加 `-Push`，在已安装 Git 且仓库已配置远端时自动提交并推送：

```powershell
powershell -ExecutionPolicy Bypass -File sync-site.ps1 -Push
```

`-Watch` 会持续监听源文件并复制到本站点，适合本地预览或隧道演示：

```powershell
powershell -ExecutionPolicy Bypass -File sync-site.ps1 -Watch
```

静态托管平台在收到 push 后通常 30—90 秒完成部署，这就是公网版能实现的“自动同步”。如果要毫秒级实时同步，需要把站点放在一台常开服务器上并配合文件监听/热更新；纯静态托管做不到。

## 说明

地形为程序化三维示意，省界为简化轮廓，苏区范围、部队位置和兵力为公开史料基础上的概化约数。地理底图数据来自公开的 Natural Earth / DataV 简化数据。
