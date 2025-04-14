# 🧠 Cognitive Style Quiz – 认知风格测试问卷

本项目是一个基于 Vue 3 + Vite 构建的认知风格问卷应用，用于评估用户在四种认知取向中的倾向性：

- **直觉主义 Intuitionism**
- **逻辑主义 Logicalism**
- **理性主义 Rationalism**
- **意志主义 Voluntarism**

通过 20 道情境题目，帮助用户了解自己在面对问题、做决策时更偏向哪种认知风格。

---

## ✨ 功能特色

- 随机打乱选项顺序，确保无选项顺序偏差
- 实时进度条显示当前完成情况
- 最终展示各认知维度的得分条形图
- 支持 GitHub Pages 部署访问

---

## 🚀 在线体验

👉 [点击此处访问问卷页面](https://droggeljugLM.github.io/cognitive-style-quiz/){:target="_blank"}

---

## 🛠 技术栈

- [Vue 3](https://vuejs.org/)
- [Vite](https://vitejs.dev/)
- [gh-pages](https://www.npmjs.com/package/gh-pages) 用于部署

---

## 🧩 本地开发

```bash
# 安装依赖
npm install

# 本地运行开发服务器
npm run dev
```

---

## 📦 构建与部署

```bash
# 构建静态文件（输出至 dist/）
npm run build

# 将 dist 部署至 GitHub Pages（gh-pages 分支）
npm run deploy
```

---

## 📁 项目结构简要

```
├── public/             # 公共资源目录
├── src/
│   ├── components/
│   │   └── CognitiveQuiz.vue   # 问卷主组件
│   └── App.vue
├── index.html
├── vite.config.js
└── package.json
```

---

## 📖 License

MIT License © 2025 droggeljugLM
