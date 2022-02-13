---
title: "NewBlog"
date: 2022/2/11 21:17
tags:  
  - blog
  - learning
categories: "Enrich "
discription: "learn how to build my own blog"

---

# 用github和Hexo搭建博客
步骤：
1. 创建github仓库
2. 安装nodejs和Hexo，并建站
3. 配置

## 创建github仓库  
create a new repository  
Rrpository name :**name**.github.io    (as your follow)
仓库类型为：Public  
**先不要git clone** 原因有二：
1. git clone 之后文件文件夹以后文件夹以`**name**.github.io`命名，不符合自己命名习惯，最重要的是不是空文件夹，就算什么都没有也会产生.git隐藏文件
2. github的clone速度取决于网速，我的网较慢（有时候甚至不能称之为网），所以设置ssh密钥，连接本地计算机。

### 设置ssh
生成ssh，复制.pub内容，并邮件发送
cat ~/.ssh/id_rsa.pub
然后，在github（gitee、gitlab）中设置ssh即可，注意有公钥有效时间的设置。

## 安装nodejs
全部默认安装
### 换源
下载仓库为淘宝镜像   
npm config set registry http://registry.npm.taobao.org/

## 安装hexo
新建文件夹，在git bash窗口中输入命令：
```bash
npm install -g hexo-cli
```
安装完成后，初始化：
```bash
hexo init <folder>
cd <folder>
npm install
```
新建完成后，指定文件夹的目录如下：
>.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes

### _config.yml
**网站的配置信息**，您可以在此配置大部分的参数。

### package.json
**应用程序的信息**。EJS, Stylus 和 Markdown renderer 已默认安装，您可以自由移除。
### scaffolds
**模版文件夹**。当您新建文章时，Hexo 会根据 scaffold 来建立文件。

Hexo的模板是指在新建的文章文件中默认填充的内容。例如，如果您修改scaffold/post.md中的Front-matter内容，那么每次新建一篇文章时都会包含这个修改。

### source
**资源文件夹**是存放用户资源的地方。除 _posts 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件会被拷贝过去。

### themes
**主题文件夹**。Hexo 会根据主题来生成静态页面。

在_config.yml

改url

```yaml
# URL
## Set your site url here. For example, if you use GitHub Page, set url as 'https://username.github.io/project'
url: https://L1yb.github.io #自己的网址
permalink: :year/:month/:day/:title/
permalink_defaults:
pretty_urls:
  trailing_index: true # Set to false to remove trailing 'index.html' from permalinks
  trailing_html: true # Set to false to remove trailing '.html' from permalinks
```

最下面

```yaml
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: 'git'
  repo: "git@github.com:L1yb/L1yb.github.io.git"
  branch: "website"
```

然后回到 Blog 文件夹中，打开 Git Bash，安装Git部署插件，输入命令：
```bash
npm install hexo-deployer-git --save
```

在输入：
```bash
hexo g     #生成网站静态文件到默认设置的 public 文件夹(hexo generate 的缩写)
hexo d    #自动生成网站静态文件，并部署到设定的仓库(hexo deploy 的缩写)
```
常用[指令](https://hexo.io/zh-cn/docs/commands "官方指令文档")：

$ hexo clean
清除缓存文件 (db.json) 和已生成的静态文件 (public)。

*在某些情况（尤其是更换主题后），如果发现您对站点的更改无论如何也不生效，您可能需要运行该命令。*

## 其他配置
参考[官方配置文档](https://hexo.io/zh-cn/docs/configuration)
网站

| 参数        | 描述                                                                                                                                                 |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| title       | 网站标题                                                                                                                                             |
| subtitle    | 网站副标题                                                                                                                                           |
| description | 网站描述                                                                                                                                             |
| keywords    | 网站的关键词。支持多个关键词。                                                                                                                       |
| author      | 您的名字                                                                                                                                             |
| language    | 网站使用的语言。对于简体中文用户来说，使用不同的主题可能需要设置成不同的值，请参考你的主题的文档自行设置，常见的有 zh-Hans和 zh-CN。                 |
| timezone    | 网站时区。Hexo 默认使用您电脑的时区。请参考 时区列表 进行设置，如 America/New_York, Japan, 和 UTC 。一般的，对于中国大陆地区可以使用 Asia/Shanghai。 |

其中，description主要用于SEO，告诉搜索引擎一个关于您站点的简单描述，通常建议在其中包含您网站的关键词。author参数用于主题显示文章的作者。

