# 仓库说明

## 这个仓库存在的意义

1. 学习脚手架的搭建
2. 根据学习到的东西用到工作中，提升工作效率

## 学习笔记

### 第一步：新建一个项目，然后使用 npm link 将此项目本地可用

1. npm init
2. 新建文件 /bin/mvc.js
3. package.json 中设置起始文件 bin:'bin/mvc.js'
4. npm link

### 第二步：使用 inquirer 插件进行命令行交互

1. 安装

```ssh
 npm install inquirer --dev # yarn add inquirer --dev
```

bin/mvc.js

```js
#! /usr/bin/env node
const inquirer = require("inquirer");

inquirer
  .prompt([
    {
      type: "input", //type： input, number, confirm, list, checkbox ...
      name: "name", // key 名
      message: "请输入你要建的项目名", // 提示信息
      default: "yf-cli-product", // 默认值
    },
  ])
  .then(answers => {
    // 打印互用输入结果
    console.log(answers);
  });
```

### 第三步：新建模板目录，根据目录生成对应文件

1.  新建模版文件夹

```
mkdir templates # 创建模版文件夹
```

2. 3.2 新建 index.html 和 common.css 两个简单的示例文件

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title><!-- ejs 语法 --> <%= name %></title>
  </head>
  <body>
    <h1><%= name %></h1>
  </body>
</html>
```

```css
/* common.css */
body {
  margin: 20px auto;
  background-color: azure;
}
```

3. 接着完善文件生成逻辑

```js
#! /usr/bin/env node
const inquirer = require("inquirer");
const path = require("path");
const fs = require("fs");
const ejs = require("ejs");

inquirer
  .prompt([
    {
      type: "input", //type： input, number, confirm, list, checkbox ...
      name: "name", // key 名
      message: "请输入你要建的项目名", // 提示信息
      default: "yf-cli-product", // 默认值
    },
  ])
  .then(answers => {
    // 模版文件目录
    const destUrl = path.join(__dirname, "../templates");
    // 生成文件目录
    // process.cwd() 对应控制台所在目录
    const cwdUrl = process.cwd();
    // 从模版目录中读取文件
    fs.readdir(destUrl, (err, files) => {
      if (err) throw err;
      files.forEach(file => {
        // console.log('file',file);
        // 使用 ejs 渲染对应的模版文件
        // renderFile（模版文件地址，传入渲染数据）
        ejs.renderFile(path.join(destUrl, file), answers).then(data => {
          // console.log('--------------------------------------------------------');
          // console.log('data',data);
          // 生成 ejs 处理后的模版文件
          fs.writeFileSync(path.join(cwdUrl, file), data);
        });
      });
    });
  });
```
