---
title: vscode 插件与配置
date: 2019-02-06 17:29:38
tags:
  - vscode
  - 工具
---

所谓磨刀不误砍柴工，作为一个前端菜鸟更应该把刀磨快一点。
总结了一下我自己常用的 vscode 的插件以及 setting.json 如何配置，可以更好的提高开发效率。

<!--more-->

## 插件集合

| 插件名                                        | 插件功能                                                               |
| --------------------------------------------- | ---------------------------------------------------------------------- |
| Auto Close Tag                                | 自动关闭 html/xml 标签                                                 |
| Auto Rename Tag                               | 自动匹配关闭标签                                                       |
| Can I Use                                     | html5,css3,svg 的浏览器兼容查询                                        |
| HTML CSS Class Completion CSS                 | class 提示                                                             |
| HTML CSS Support                              | css 提示（支持 vue）                                                   |
| background                                    | 主题背景图（windows）                                                  |
| Bracket Pair Colorizer                        | 彩虹括号，轻松找到对应括号的另外一半                                   |
| colorize                                      | 颜色显示，主要用于 css 写入颜色类样式时直接显示该颜色                  |
| Document This                                 | 智能添加注释                                                           |
| ES7 React/Redux/GraphQL/React-Native snippets | react/redux/react native 代码片段                                      |
| ESlint                                        | 代码风格检查，智能提醒报错，培养一个良好的代码风格才是重要的事         |
| Git Blame                                     | 检测每一行代码的提交记录                                               |
| indent-rainbow                                | 彩虹缩进                                                               |
| Indenticator                                  | 选中一行智能匹配代码块并连线                                           |
| JavaScript (ES6) code snippets                | es6 代码片段                                                           |
| Lodash Snippets                               | lodash 代码片段                                                        |
| miniapp                                       | 微信小程序标签/属性的智能补全，支持原生小程序/mpvue/wepy，包含代码片段 |
| Output Colorizer                              | 输出面板彩虹提示                                                       |
| Prettier - Code formatter                     | 代码格式化，需要配合 setting.json 使用                                 |
| Quokka.js                                     | 自动计算结果并显示，省去 console.log()的调试                           |
| TODO Highlight                                | TODO FIXME 高亮                                                        |
| Vetur                                         | vue 官方插件                                                           |
| VueHelper                                     | vue2 代码段，包含 vue2api，vur-router2，vuex2                          |
| vscode-fileheader                             | 一键生成文件头，并自动保存修改时间,配合 setting.json 使用              |
| vscode icons                                  | 文件图标，方便定位文件                                                 |
| Setting Sync                                  | vscode 配置同步到 gist，意味着不用再换电脑就要重写一次配置了           |

## IDE 的配置 setting.json

```json
{
  "fileheader.Author": "chenghao", //vscode-fileheader 使用
  "fileheader.LastModifiedBy": "chenghao", //vscode-fileheader 使用
  "editor.fontSize": 14, // 编辑器字号
  "editor.tabSize": 2, // 1个制表符的缩进大小
  "editor.lineHeight": 17, // 通过使用鼠标滚轮同时按住 Ctrl 可缩放编辑器的字体
  "editor.mouseWheelZoom": true, // 行太长自动换行
  "editor.wordWrap": "on",
  "explorer.confirmDelete": false, // 控制文件资源管理器删除文件到废纸篓是否进行确认
  "terminal.integrated.shell.windows": "C:\\Windows\\System32\\cmd.exe", // 终端，mac可省略
  "breadcrumbs.enabled": true, //启用面包屑导航
  "workbench.editor.enablePreview": false, //打开文件不覆盖
  "editor.formatOnSave": true, //保存时是否进行格式化
  "search.exclude": {
    // 搜索黑名单
    "**/node_modules": true,
    "**/bower_components": true,
    "**/dist": true,
    "**/build": true,
    "**/.git": true,
    "**/.gitignore": true,
    "**/.svn": true,
    "**/.DS_Store": true,
    "**/.idea": true,
    "**/.vscode": false,
    "**/yarn.lock": true
  },
  "files.associations": {
    // 配置文件关联，以便启用对应的智能提示，比如wxss使用css
    "*.vue": "vue",
    "*.wxss": "css"
  },
  "background.enabled": true, // background插件是否可用，以下是对background插件的配置
  "background.useDefault": false,
  "background.customImages": ["file:///F:/background/3.jpg"],
  "background.useFront": false,
  "background.style": {
    "content": "''",
    "pointer-events": "none",
    "position": "absolute",
    "top": "0",
    "right": "0",
    "width": "100%",
    "height": "100%",
    "z-index": "99999",
    "background.repeat": "no-repeat",
    "background-size": "contain",
    "opacity": 0.1
  },
  "prettier.semi": false, //是否使用分号
  "prettier.tabWidth": 2,
  "prettier.singleQuote": true, // 是否使用单引号，true则在js中不能使用双引号
  "prettier.printWidth": 120, //每行代码在该数字范围内，分辨率大可以把它写大些
  "eslint.validate": [
    // eslint 验证文件类型
    "javascript",
    "javascriptreact",
    { "language": "vue", "autoFix": true },
    { "language": "html", "autoFix": true }
  ],
  "eslint.autoFixOnSave": true, // ctrl+s 保存时自动修正格式错误的js代码
  "vetur.format.defaultFormatter.js": "vscode-typescript", //格式化的风格覆盖vscode的默认配置
  "javascript.format.insertSpaceBeforeFunctionParenthesis": true, // 函数定义与后面括号之间增加一个空格
  "vetur.format.defaultFormatter.html": "js-beautify-html", //格式化.vue中html
  "vetur.format.defaultFormatterOptions": {
    "js-beautify-html": {
      "wrap_attributes": "force" //属性强制折行对齐
    }
  }
}
```

真正做到拿来既用，希望对你有帮助 💕

晚安 🌙
