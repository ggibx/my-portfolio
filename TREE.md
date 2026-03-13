# 项目目录结构

```
resume/
├── node_modules/          # 项目依赖包（npm install 后生成）
├── src/                   # 源代码目录
│   ├── components/        # Vue 组件目录
│   │   └── ResumePage.vue # 简历上传页面组件
│   ├── App.vue            # 根组件
│   ├── main.js            # 应用入口文件
│   └── style.css          # 全局样式文件
├── index.html             # HTML 入口文件
├── package.json           # 项目配置文件
├── package-lock.json      # 依赖锁定文件
├── vite.config.js         # Vite 构建配置
├── README.md              # 项目说明文档
└── .gitignore            # Git 忽略文件配置
```

## 文件说明

### 配置文件
- **package.json** - 项目依赖和脚本配置
- **vite.config.js** - Vite 开发服务器和构建配置
- **.gitignore** - Git 版本控制忽略文件

### 入口文件
- **index.html** - HTML 入口，挂载 Vue 应用
- **src/main.js** - Vue 应用入口，创建并挂载根组件

### 组件文件
- **src/App.vue** - 根组件，包含应用容器
- **src/components/ResumePage.vue** - 简历上传和预览组件（核心功能）

### 样式文件
- **src/style.css** - 全局样式定义

### 文档
- **README.md** - 项目使用说明

