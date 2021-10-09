- [工具](#工具)
  - [git](#git)
    - [原理](#原理)
      - [git 分区](#git-分区)
      - [文件的三种状态](#文件的三种状态)
      - [`.git` 目录](#git-目录)
      - [git 对象](#git-对象)
      - [一些问题](#一些问题)
    - [git fetch 和 git pull 的区别](#git-fetch-和-git-pull-的区别)
    - [git rebase 和 git merge 的区别](#git-rebase-和-git-merge-的区别)
    - [git 与传统的集中式版本控制系统 (CVCS) 的区别](#git-与传统的集中式版本控制系统-cvcs-的区别)
  - [Webpack](#webpack)
    - [构建流程](#构建流程)
    - [entry 入口](#entry-入口)
    - [module 模块](#module-模块)
    - [chunk 代码块](#chunk-代码块)
    - [bundle](#bundle)
    - [loader 模块转换器](#loader-模块转换器)
    - [plugins 插件](#plugins-插件)
      - [loader 和 plugin 的区别](#loader-和-plugin-的区别)
    - [模块热替换 HMR](#模块热替换-hmr)
    - [如何自定义一个 webpack 插件](#如何自定义一个-webpack-插件)


# 工具

## git

### 原理

从根本上来讲，Git 是一个内容寻址 (content-addressable) 文件系统，并在此之上提供了一个版本控制系统的用户界面

#### git 分区

- 工作区 (workspace)：电脑中能看到的目录就是工作区
- 暂存区 (index/stage)：
- 本地仓库（版本库） (repository)：工作区中的隐藏目录 `.git` 就是版本库
- 远程仓库 (remote)

#### 文件的三种状态

- 已提交 (committed)：表示该文件已经被安全地保存在本地数据库中了
- 已修改 (modified)：表示修改了某个文件，还没有提交保存
- 已暂存 (staged)：表示把已修改的文件放在下次提交时要保存的清单中

#### `.git` 目录

如果想备份或复制一个版本库，把这个目录拷贝至另一处即可

- description 文件：仅供 GitWeb 程序使用
- config 文件：包含项目特有的配置选项
- HEAD 文件：指示目前被检出的分支
- index 文件：保存暂存区信息
- objects 目录：存储所有数据内容
- refs 目录：存储指向数据（分支）的提交对象的指针
- info 目录：包含一个全局性排除 (global exclude) 文件，用于放置那些不希望记录在 `.gitignore` 文件中的忽略模式 (ignored patterns)
- hooks 目录：包含客户端或服务端的钩子脚本

#### git 对象

git 是一个内容寻址文件系统，意味着 git 的核心部分是一个简单的键值对数据库，向 git 仓库中插入任意类型的内容，会返回一个唯一的键，通过该键可以在任意时刻取回该内容

git 对象类型是树对象 (tree object)，git 以一种类似于 UNIX 文件系统的方式存储内容，树对象对应 UNIX 中的目录项，数据对象 (blob object) 则大致对应 inodes 或文件内容。一个树对象包含一条或多条树对象记录 (tree entry)，每条记录含有一个指向数据对象或者子树对象的 SHA-1 指针，以及相应的模式、类型、文件名信息

#### 一些问题

- 每次 commit，git 储存的是全新的文件快照。因为如果要储存文件的变更部分，git 需要全都遍历一遍计算出变更内容，会花费很长时间。当然，如果 git 仓库体积真的很大的时候， git 会有垃圾回收机制，不仅会清除无用的 object，还会把已有的相似 object 打包压缩

- git 通过 SHA1 哈希算法和哈希树来保证历史记录不可篡改，如果纪录被篡改，数据对象的 SHA1 哈希值就会改变，与之相关的树对象的哈希值也会改变，commit 的哈希值也会跟着改变。而且 git 是分布式系统，每个人都有一份完整历史的 git 仓库，很容易就能发现问题

[ Git 内部原理（git pro）](https://git-scm.com/book/zh/v2/Git-%E5%86%85%E9%83%A8%E5%8E%9F%E7%90%86-%E5%BA%95%E5%B1%82%E5%91%BD%E4%BB%A4%E4%B8%8E%E4%B8%8A%E5%B1%82%E5%91%BD%E4%BB%A4) 我大受震撼，不太能懂，以后再说

[浅析 Git 思想和工作原理](https://www.jianshu.com/p/619122f8747b)

[深入理解Git实现原理](https://zhuanlan.zhihu.com/p/45510461)

[这才是真正的Git——Git内部原理揭秘！](https://www.jiqizhixin.com/articles/2019-12-20)

### git fetch 和 git pull 的区别

- git fetch 只是将远程仓库的变化下载下来，并没有和本地分支合并

- git pull 会将远程仓库的变化下载下来，并和当前分支合并

### git rebase 和 git merge 的区别

- git rebase 会先找到两个分支的第一个共同的 commit 祖先记录，然后提取当前分支这之后的所有 commit 记录，然后将这个 commit 记录添加到目标分支的最新提交的后面。经过这个合并后，两个分支合并后的 commit 记录就变成了线性的记录

- git merge 会新建一个新的 commit 对象，然后两个分支以前的 commit 记录都指向这个新 commit 记录，这种方法会保留之前每个分支的 commit 历史

### git 与传统的集中式版本控制系统 (CVCS) 的区别

## Webpack 

静态模块打包工具，会在内部构建一个依赖关系图

### 构建流程

1. 初始化：开始构建，读取配置参数，加载 plugin，实例化 compiler
2. 编译：从 entry 出发，调用每个模块所对应的 loader，根据依赖关系递归编译处理
3. 输出：将编译后的模块组合成 chunk，转换成 bundle 文件输出

### entry 入口

指示 webpack 应该使用哪个模块作为起点

默认是 `./src/index.js`

### module 模块

在 webpack 中，所有的资源都被当作是模块，可以被引用

### chunk 代码块

由多个模块组成

### bundle 

最后输出的文件，可以直接在浏览器中运行

### loader 模块转换器

webpack 只能理解 JavaScript 和 JSON 文件，loader 可以用来转换不同格式的文件

`npm install --save-dev css-loader`

```
   module.exports = {
     module: {
      rule: [
        { test: /\.css$/, use: 'css-loader' },
      ],
     },
   };
```
### plugins 插件

plugins 用来解决 loader 没能解决的事，比如打包优化，资源管理，注入环境变量

#### loader 和 plugin 的区别

- loader
  - 在打包前或期间调用
  - 根据不同匹配条件处理资源
  - 调用顺序与书写顺序相反
  - 写在前面的接收写在后面的返回值作为输入值

- plugin
  - 基于 Tapable 实现
  - 事件触发调用，监听 webpack 广播的事件
  - 不同生命周期改变输出结果


### 模块热替换 HMR


###

webpack --config：打包，在配置文件中指定 entry 和 output，指定配置文件，默认的配置文件是 webpack.config.js 或 webpackfile.js，

webpack --watch：监听文件变化，发现修改后重新编译

webpack-dev-server：改变开发服务器的行为，可以提供本地服务


### 如何自定义一个 webpack 插件

- 声明一个自定义命名的类或函数
- 在原型中新增 apply 方法
- 声明由 Compiler 模块暴露的生命周期函数
- 使用 webpack 提供的 API 或 自行处理内部实例的数据
- 处理完成后，调用 webpack 提供的回调函数


