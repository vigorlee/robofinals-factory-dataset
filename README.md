# Factory LW-BenchHub Web Portal - 部署说明

## 🎉 项目已完成

**项目位置**: `/home/gem/workspace/agent/workspace/factory-web-portal/`

**本地访问**: `http://localhost:3000`

---

## 📁 项目结构

```
factory-web-portal/
├── index.html              # 主页面（包含所有功能）
├── vercel.json             # Vercel 部署配置
├── package.json            # 项目配置
├── data/
│   ├── robots.json         # 机器人数据
│   ├── scenes.json         # 场景数据
│   ├── tasks.json          # 任务数据
│   └── DATASET.md          # 数据集文档（Lightwheel 格式）
├── public/
│   ├── images/             # 图片资源
│   └── models/             # 3D 模型资源
└── README.md               # 本文件
```

---

## 🚀 部署方式

### 方式 1: Vercel 部署（推荐）

1. **安装 Vercel CLI**（如果未安装）:
   ```bash
   npm install -g vercel
   ```

2. **登录 Vercel**:
   ```bash
   vercel login
   ```

3. **部署到预览环境**:
   ```bash
   cd /home/gem/workspace/agent/workspace/factory-web-portal
   vercel
   ```

4. **部署到生产环境**:
   ```bash
   vercel --prod
   ```

**部署后会获得网址**: `https://factory-lw-benchhub.vercel.app`

### 方式 2: GitHub Pages

1. **创建 GitHub 仓库**:
   ```bash
   cd /home/gem/workspace/agent/workspace/factory-web-portal
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/factory-lw-benchhub.git
   git push -u origin main
   ```

2. **启用 GitHub Pages**:
   - 进入仓库 Settings → Pages
   - Source 选择 `main` branch
   - 保存后获得网址：`https://YOUR_USERNAME.github.io/factory-lw-benchhub/`

### 方式 3: 本地预览

```bash
cd /home/gem/workspace/agent/workspace/factory-web-portal
npx serve . -l 3000
```

访问：`http://localhost:3000`

---

## 📊 网站功能

### 1. Home (首页)
- 项目概览
- 关键统计数据
- 快速入口

### 2. Robots (机器人)
- 支持的机器人列表（UR5, UR10, Franka 等）
- 机器人规格参数
- USD 模型下载

### 3. Scenes (场景)
- 工厂场景库（装配线、仓储区、质检站）
- Scene Graph 预览
- 场景下载

### 4. Tasks (任务)
- 任务库表格视图
- 任务模板（YAML 格式）
- 成功率和统计数据

### 5. Dataset (数据集)
- 数据集统计（2,500+ episodes）
- π0.5 训练结果
- 数据下载链接

### 6. Leaderboard (排行榜)
- 模型性能排名
- 评估指标对比
- 提交评测入口

---

## 📄 数据集文档

数据集文档已按照 Lightwheel 格式创建：
- **位置**: `data/DATASET.md`
- **内容**:
  - 数据集概览
  - 数据格式说明
  - 任务规格
  - 场景规格
  - 训练协议（π0.5）
  - 评测指标
  - 下载链接

---

## 🎨 设计特点

1. **响应式设计**: 支持桌面和移动端
2. **单页应用**: 所有功能在一个页面，加载快速
3. **数据驱动**: 从 JSON 文件动态加载内容
4. **现代化 UI**: 使用 Tailwind CSS
5. **图标支持**: Lucide Icons

---

## 📈 数据集统计

| 指标 | 数值 |
|------|------|
| 总 Episodes | 2,500+ |
| 任务模板 | 4 |
| 场景变体 | 3 |
| 支持机器人 | 3 (UR5, UR10, Franka) |
| 成功率（基线） | 85-92% |
| Sim2Real Gap | 8.5% |

---

## 🔧 自定义内容

### 添加新机器人
编辑 `data/robots.json`，添加新条目：
```json
{
  "id": "new-robot-id",
  "name": "Robot Name",
  "manufacturer": "Manufacturer",
  "type": "industrial",
  "dof": 6,
  "payload": "5kg",
  ...
}
```

### 添加新场景
编辑 `data/scenes.json`，添加新条目。

### 添加新任务
编辑 `data/tasks.json`，添加新条目。

---

## 📝 下一步

1. **立即访问**: 打开 `http://localhost:3000` 预览网站
2. **部署到 Vercel**: 获得公网可访问网址
3. **补充资源**: 添加真实的机器人/场景图片
4. **集成 3D 查看器**: 添加 Three.js 实现模型预览
5. **连接后端**: 实现动态数据加载

---

## 🌐 访问网址

**本地预览**: `http://localhost:3000`

**Vercel 部署后**: `https://factory-lw-benchhub.vercel.app` (部署后更新)

**GitHub Pages**: `https://<username>.github.io/factory-lw-benchhub/` (部署后更新)

---

## 📞 联系

- **文档**: `data/DATASET.md`
- **技术支持**: contact@lightwheel.net
- **项目主页**: https://lightwheel.net

---

**创建日期**: 2026-03-19  
**版本**: V1.0  
**状态**: ✅ 已完成
