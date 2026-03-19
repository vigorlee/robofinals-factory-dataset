# 🚀 Factory LW-BenchHub 外部部署指南

## 方案 1: Vercel 部署（推荐，5 分钟搞定）

### 步骤：
1. **打开 Vercel**: https://vercel.com/new

2. **导入 Git 仓库**:
   - 点击 "Import Git Repository"
   - 选择 "Import Third Party Git Repository"
   - 输入仓库地址或上传项目文件

3. **一键部署**:
   - Project Name: `factory-lw-benchhub`
   - Framework Preset: `Other`
   - 点击 "Deploy"

4. **获得网址**: 
   - 部署完成后获得：`https://factory-lw-benchhub.vercel.app`

---

## 方案 2: Netlify Drop（最简单，2 分钟）

### 步骤：
1. **打开 Netlify Drop**: https://app.netlify.com/drop

2. **上传文件夹**:
   - 将整个 `factory-web-portal` 文件夹拖到上传区域
   
3. **获得网址**:
   - 部署完成后获得：`https://xxx-xxx-xxx.netlify.app`

---

## 方案 3: GitHub Pages（永久免费）

### 步骤：
1. **创建 GitHub 仓库**:
   ```bash
   cd /home/gem/workspace/agent/workspace/factory-web-portal
   git remote add origin https://github.com/YOUR_USERNAME/factory-lw-benchhub.git
   git push -u origin main
   ```

2. **启用 GitHub Pages**:
   - 进入仓库 Settings → Pages
   - Source 选择 `main` branch
   - 保存

3. **获得网址**:
   - `https://YOUR_USERNAME.github.io/factory-lw-benchhub/`

---

## 方案 4: Cloudflare Pages（推荐，免费且快速）

### 步骤：
1. **登录 Cloudflare**: https://dash.cloudflare.com/sign-up

2. **创建 Pages 项目**:
   - 进入 Workers & Pages → Create application → Pages
   - 选择 "Upload assets"
   - 上传 `factory-web-portal` 文件夹所有内容

3. **部署**:
   - Project name: `factory-lw-benchhub`
   - 点击 "Deploy"

4. **获得网址**:
   - `https://factory-lw-benchhub.pages.dev`

---

## 📦 项目文件位置

```
/home/gem/workspace/agent/workspace/factory-web-portal/
├── index.html          # 主页面
├── vercel.json         # Vercel 配置
├── package.json        # 项目配置
├── data/
│   ├── robots.json     # 机器人数据
│   ├── scenes.json     # 场景数据
│   ├── tasks.json      # 任务数据
│   └── DATASET.md      # 数据集文档
└── README.md           # 说明文档
```

---

## 🎯 快速部署命令（如果有 Git 远程仓库）

```bash
cd /home/gem/workspace/agent/workspace/factory-web-portal

# 添加远程仓库（替换为你的仓库地址）
git remote add origin https://github.com/YOUR_USERNAME/factory-lw-benchhub.git

# 推送到 GitHub
git push -u origin main

# 部署到 Vercel（需要登录）
npx vercel --prod

# 或者部署到 Netlify（需要登录）
npx netlify-cli deploy --prod --dir=.
```

---

## ✅ 部署后检查清单

- [ ] 网站可以正常访问
- [ ] 所有页面加载正常
- [ ] 数据正确显示
- [ ] 响应式设计正常
- [ ] 图片/资源加载正常

---

## 🌐 预期网址格式

| 平台 | 网址格式 |
|------|---------|
| Vercel | `https://factory-lw-benchhub.vercel.app` |
| Netlify | `https://factory-lw-benchhub.netlify.app` |
| GitHub Pages | `https://<username>.github.io/factory-lw-benchhub` |
| Cloudflare Pages | `https://factory-lw-benchhub.pages.dev` |

---

**创建日期**: 2026-03-19  
**状态**: ✅ 准备部署
