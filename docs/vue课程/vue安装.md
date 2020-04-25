## 1. 前言

本小节将带领大家学习一下如何安装 `Vue`。包括独立版本、CDN、NPM、CLI 工具四种不同的安装方法。

## 2. 独立版本

我们可以在 Vue.js 的官网上直接下载 vue.min.js 并用`<script>`标签引入。

[下载Vue.js](https://vuejs.org/js/vue.min.js)

安装步骤：

1. 打开[Vue.js下载地址](https://vuejs.org/js/vue.min.js)并拷贝所有代码。
2. 创建`vue.min.js`文件，并将拷贝的源码粘贴进去。
3. 创建`index.html`并通过`<script>`标签引入。
4. 打印`Vue`验证是否成功。

```
//文件结构
└─ lession
   ├─ index.html
   └─ vue.min.js
```

案例演示

```html
<!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
  </head>
  <body>
    <div id="app">
      Hello Imooc ！
    </div>
  </body>
  <script src="./vue.min.js"></script>
  <script type="text/javascript">
    console.log(Vue)
  </script>
</html>
```

[运行案例](http://www.imooc.com/wiki/run/529.html)点击 "运行案例" 可查看在线运行效果

```
## 打印结果
ƒ wn(e){this._init(e)}
```

## 3. 使用CDN方法

使用CDN的方式引入`Vue`更加方便、快捷。以下推荐几个比较稳定的 CDN。

1. <https://cdn.staticfile.org/vue/2.2.2/vue.min.js>
2. <https://unpkg.com/vue/dist/vue.js>
3. <https://cdnjs.cloudflare.com/ajax/libs/vue/2.1.8/vue.min.js>
4. <https://cdn.jsdelivr.net/npm/vue/dist/vue.js>

案例演示

```html
<!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
  </head>
  <body>
    <div id="app">
      Hello Imooc ！ 
    </div>
  </body>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.1.8/vue.min.js"></script>
  <script type="text/javascript">
    console.log(Vue)
  </script>
</html>
```

[运行案例](http://www.imooc.com/wiki/run/528.html)点击 "运行案例" 可查看在线运行效果

## 4. 使用NPM的方法

使用`NPM`的方法进行安装需要先在本地安装`Node`环境。

#### 3.1、Windows 上安装 Node.js

[32 位安装包下载地址](http://nodejs.org/dist/v0.10.26/node.exe)
64 位安装包下载地址
下载对应的安装包后，双击安装包，傻瓜式下一步安装就好了。

#### 3.2、Mac 上安装 Node.js

1、在[官方下载网站](https://nodejs.org/en/download/)下载 pkg 安装包，直接点击安装即可。
2、使用 brew 命令来安装：`brew install node`

```
brew install node
```

```
# 查看本地node环境
$ node -v
v10.16.0
```

在用 `Vue` 构建大型应用时推荐使用 `NPM` 安装。当然，仅仅使用`npm install`是不能完整搭建`Vue`开发环境的。还需要[webpack](https://webpack.js.org/)或 [Browserify](http://browserify.org/) 等模块打包器配合使用。

```
# 创建项目目录
$ mkdir demo

# 进入项目文件夹
$ cd demo
$ npm init -y

# 最新稳定版
$ npm install vue

# 安装指定版本Vue
$ npm install vue@2.6.3
```

安装完成后可以查看到demo目录下多了 node_module/vue 文件夹。说明`Vue`成功安装。

## 5. 命令行工具（CLI）

当然，看过 `Vue` 官网的同学肯定都知道 `Vue` 提供了一个[官方的 CLI](https://github.com/vuejs/vue-cli)，为我们快速搭建大型单页应用。我们并不建议同学们在一开始就使用 `Vue-cli` 来构建项目，因为这需要同学们对 `Node.js`、`Webpack` 等技术有所了解。在熟悉了 `Vue` 的语法之后，我们将有一个完整的章节来学习和使用 `Vue-cli`。

## 6. 小结

本小节我们介绍了 `Vue` 安装的几种方式。作为初学者，我们建议同学们先使用第一、第二种方式进行学习。在对 `Vue` 有一定的了解之后再学习脚手架工具 `Vue-Cli` 的使用。在接下来 `Vue` 基础的章节中我们都将使用 `<script src="https://unpkg.com/vue/dist/vue.js"></script>` 的方式进行代码演示。关于脚手架工具 `Vue-Cli` 的学习，我们将在最后一个章节中一起探讨。