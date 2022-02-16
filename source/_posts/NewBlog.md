---
title: "NewBlog"
date: 2022/2/11 21:17
update: 2022/2/16 23:24
tags:  
  - hexo
  - learning
categories: Enrich 
top_img: https://gitee.com/l1yb/picgo/raw/master/cover/1003736.jpg
cover: https://gitee.com/l1yb/picgo/raw/master/cover/OneRunFlower.jpg
---

# 用github和Hexo搭建博客

步骤：
1. 创建github仓库
2. 安装nodejs和Hexo，并建站
3. 配置

## 1. 创建github仓库  
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

## 2. 安装nodejs
全部默认安装
### 换源
下载仓库为淘宝镜像   
npm config set registry http://registry.npm.taobao.org/

## 3. 安装hexo
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
>├── _config.yml
>├── package.json
>├── scaffolds
>├── source
>|   ├── _drafts
>|   └── _posts
>└── themes

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

## 4. 其他配置
参考[官方配置文档](https://hexo.io/zh-cn/docs/configuration)
网站

| 参数        | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| title       | 网站标题                                                     |
| subtitle    | 网站副标题                                                   |
| description | 网站描述                                                     |
| keywords    | 网站的关键词。支持多个关键词。                               |
| author      | 您的名字                                                     |
| language    | 网站使用的语言。对于简体中文用户来说，使用不同的主题可能需要设置成不同的值，请参考你的主题的文档自行设置，常见的有 zh-Hans和 zh-CN。 |
| timezone    | 网站时区。Hexo 默认使用您电脑的时区。请参考 时区列表 进行设置，如 America/New_York, Japan, 和 UTC 。一般的，对于中国大陆地区可以使用 Asia/Shanghai。 |

其中，description主要用于SEO，告诉搜索引擎一个关于您站点的简单描述，通常建议在其中包含您网站的关键词。author参数用于主题显示文章的作者。

--------------------------------------------------------------------

到此位置已经可以搭建完毕了，其余设置都是使博客更符合自己的偏好。



## 5. 图床
**采用gitee方案**
用github发现有点卡，测试的时候太慢了，网络原因导致心态崩溃，所以，以学习为目的的话，不要追求一步到位用什么最好了，怎么方便学习怎么来。

> 参考教程
> LeoNG：`https://zhuanlan.zhihu.com/p/102594554`

## 6. 主题界面
### 1. Front-matter
[Front-matter官方教程](https://butterfly.js.org/posts/dc584b87/#Post-Front-matter)
Front-matter 是 markdown 文件最上方以`--- `分隔的区域，用于指定个别档案的变数。
Page Front-matter 用于页面配置
Post Front-matter 用于文章页配置
```yaml
# post front-matter

```
| 写法                  | 解释                                                         |
| --------------------- | ------------------------------------------------------------ |
| title                 | 【必需】文章标题                                             |
| date                  | 【必需】文章创建日期                                         |
| updated               | 【可选】文章更新日期                                         |
| tags                  | 【可选】文章标籤                                             |
| categories            | 【可选】文章分类                                             |
| top_img               | 【可选】文章顶部图片                                         |
| cover                 | 【可选】文章缩略图(如果没有设置top_img,文章页顶部将显示缩略图，可设为false/图片地址/留空) |
| copyright             | 【可选】显示文章版权模块(默认为设置中post_copyright的enable配置) |
| copyright_author      | 【可选】文章版权模块的文章作者                               |
| copyright_author_href | 【可选】文章版权模块的文章作者链接                           |
| copyright_url         | 【可选】文章版权模块的文章连结链接                           |
| copyright_info        | 【可选】文章版权模块的版权声明文字                           |
| highlight_shrink      | 【可选】配置代码框是否展开(true/false)(默认为设置中highlight_shrink的配置) |
| aside                 | 【可选】显示侧边栏 (默认 true)                               |

###  2. 添加新页面
`hexo new page tags`
1. 前往你的 Hexo 博客的根目录
2. 输入`hexo new page tags`
3. 你会找到`source/tags/index.md`这个文件
4. 修改这个文件：
**记得添加 `type: "tags"`**
```yaml
---
title: 标籤
date: 2018-01-05 00:00:00
type: "tags"
---
```
### 3. 图库
`hexo n page xxx`
失败！预览怎么显示啊？！


### 4. 友情链接
创建链接
`hexo new page link`
添加友情链接
在根目录`source/_data`(没有自创) 创建`link.yaml`
```yaml
- class_name: 友情链接
  class_desc: 那些人，那些事
  link_list:
    - name: Hexo
      link: https://hexo.io/zh-tw/
      avatar: https://d33wubrfki0l68.cloudfront.net/6657ba50e702d84afb32fe846bed54fba1a77add/827ae/logo.svg
      descr: 快速、简单且强大的网誌框架

- class_name: 网站
  class_desc: 值得推荐的网站
  link_list:
    - name: Youtube
      link: https://www.youtube.com/
      avatar: https://i.loli.net/2020/05/14/9ZkGg8v3azHJfM1.png
      descr: 视频网站
    - name: Weibo
      link: https://www.weibo.com/
      avatar: https://i.loli.net/2020/05/14/TLJBum386vcnI1P.png
      descr: 中国最大社交分享平臺
    - name: Twitter
      link: https://twitter.com/
      avatar: https://i.loli.net/2020/05/14/5VyHPQqR6LWF39a.png
      descr: 社交分享平臺
```


## 7、主题配置

### 1. 头像
```yaml
# Avatar (頭像)
avatar:
  img: 
  https://gitee.com/l1yb/picgo/raw/master/cover/moonAnd.jpeg
  effect: false # 头像是否一直转
```

**注意！！！**
图片如果直接换行会报错，要在前面加个 `- url` 不报错了，但是照片不生效，所以不要随意换行，加个空格就行了



### 2.  顶部图：

```yaml
#默认主页图片
default_top_img: https://gitee.com/l1yb/picgo/raw/master/cover/1003736.jpg
```
文章封面：
```yaml
#配置文件中
cover:
	...
#both left right 
position: both 
default cover:
	- url
	- url
```
```yaml
title: n
```

### 3. 文章meta显示
*主题配置文件*
```yaml 
post_meta:
  page:
    date_type: both # created or updated or both 主页文章日期是创建日或者更新日或都显示
    date_format: relative # date/relative 显示日期还是相对日期
    categories: true # true or false 主页是否显示分类
    tags: true # true or false 主页是否显示标籤
    label: true # true or false 显示描述性文字
  post:
    date_type: both # created or updated or both 文章页日期是创建日或者更新日或都显示
    date_format: relative # date/relative 显示日期还是相对日期
    categories: true # true or false 文章页是否显示分类
    tags: true # true or false 文章页是否显示标籤
    label: true # true or false 显示描述性文字

```

### 4. 文章版权
*主题配置文件*: `post_copyright`
单独文章显示版权信息 设置front-matter: 
```yaml
copyright_author: xxxx
copyright_author_href: https://xxxxxx.com
copyright_url: https://xxxxxx.com
copyright_info: 此文章版权归xxxxx所有，如有转载，请註明来自原作者
```

### 5. 侧边栏设置（部分）
```yaml
aside: 
	position:left
```

