# 开发工具

#### 在vscode中配置eslint autoFixOnSave

打开settings.json,添加下列代码：

```
    "eslint.autoFixOnSave": true,
    "eslint.validate": [
        "javascript",
        "html",
        "vue",
        {
          "language": "vue",
          "autoFix": true
        }
      ],
```

#### 修复项目中不规范代码

进入项目目录，执行：

```
npm run lint
```