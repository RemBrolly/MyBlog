---
# 文章标题
title: Hexo + Github Action + Gitee Pages 搭建个人博客站点
# 文章创建日期
date: 2023-07-01 17:57:58
# 文章标签
tags: 
  - Hexo 
  - Github Action
  - Gitee Pages
  - 博客搭建
# 文章分类
categories:
  - 教程

# 文章封面(这里的优先级笔主题配置中默认的封面高)
cover:

---

# Hexo + Github Action + Gitee Pages 搭建个人博客站点

- `Hexo` 为博客生成框架
- `Github` 为博客文件托管平台，使用 `Git Action` 来进行博客的自动部署 
- `Gitee Pages` 为博客静态站点  

（[点 这](#build) 跳过下面的介绍部分，直达`搭建过程`。） 

## 为什么使用这种方式搭建呢？ 

笔者最近想搭建一个自己的博客站点，然后去网上搜罗了很多教程，各位大佬博主们给的搭建方法都很细致，使用他们的方法也能够快速搭建一个自己的博客站点。我之所以使用这个组合来搭建站点主要是因为如下的原因：  
- **Hexo框架**  
首先，博客框架的选择全凭个人喜好。目前有许多的博客搭建框架，比如WordPress、VuePress、Typecho、Hugo和Hexo等，我个人比较喜欢Hexo的主题以及其使用者也挺多，当遇到问题的时候可以快速的找到相应的解决办法。而且其是基于NodeJs的，对于我这个前端佬来说，简直不要太亲密。

- **GitHub 文件托管平台**  
众所周知 `Github` 是全球最大的男性交友网站😊，作为程序猿以及互联网的打工人是离不开Github的，同样我也喜欢将自己的项目和代码放到GitHub进行存储，所以博客站点自然也不例外。笔者将Github作为博客站点的文件托管平台。

- **Gitee Pages 静态站点**  
使用 `Gitee Pages` 的一个重要原因有且只有一个：**国内访问速度快！**我之前也试过Vercel、Netlify和 Github Pages这两个静态站点，它们能能够直接将 Github 仓库的博客项目自动进行部署（*这里的自动部署包括你的项目更改时其能检测到并自己开始重新部署更新*）并且操作及其简单，但是当你访问自己的博客时会发现：进不去啊！这就很烦，自己写的博客居然得用科学上网来浏览，这我是不能接受的。  
（当然也可以使用这两个站点之后再整一个域名，然后使用 `CDN` 来进行加速访问，这里又有一个问题就是 CDN 基本都是收费的而且都需要备案，备案就意味着你要整个服务器，这里新问题就又来了，服务器贵啊！有免费的和无需备案的 CDN ，我用过一个就是 `Cloudflare` ，但这是一个国外的服务供应商，他的免费套餐用的都是国外站点，这意味着这个加速作用不大且有可能逆向加速。）   

- **Github Action 实现自动部署**  
Gitee Pages 不好的地方就在于你的项目不会自动进行部署，相对于 Vercel 和 Netlify 除了访问速度快就没有别的优势了。所以我们需要最近火热的 `Github Action` 来实现自动部署的功能，在自己的博客仓库开启 Github Action 功能之后需要进行一些配置设定（后边会介绍），当自己的博客内容修改完毕并提交仓库之后 Github 就会检测到项目变化然后开始将博客项目重新部署到 Gitee Pages ，我们博客的修改内容就会重新加载至 Gitee Pages 页面，这样就解决了 Gitee Pages 无法自动部署的问题了。（当然如果使用 Gitee 的流水线功能也能实现该效果，但是我比较喜欢将代码保存在 Github 嘛😊）

## <span id="build">搭建过程</span>

### **Hexo安装与配置**
1. 安装 `Git`，由于篇幅原因，具体的安装过程和同步 Github 的过程自己百度一下

2. 安装 NodeJs ，具体安装过程和使用自行百度 

3. win+r 然后输入 cmd 打开命令终端、

![终端](image-1.png)

4. 全局安装 Hexo 脚手架

```bash
    npm install hexo-cli -g # 全局安装hexo命令行工具
```
![Hexo脚手架](image-2.png)

5. 新建一个文件夹存储本地博客项目（在哪都行）

![文件夹](image.png)

6. 初始化Hexo框架

```bash
    hexo init "本地博客文件存放路径" # 目录名称不含空格的时候双引号可以省略
```
![打开终端](image-3.png)
![项目初始化](image-4.png)
7. 然后进入本地博客目录,打开终端安装所需要的依赖

```bash
    npm install # 安装的依赖项在package.json文件dependencies字段中可以看到
```
![安装依赖](image-5.png)

**有关博客项目目录的介绍可以[点这里](#catalog)进行查看。**


### **上传仓库**

1. 使用 `Vscode` 打开博客项目，并将项目上传至 Github

![VScode](image-6.png)
![初始化仓库](image-7.png)
初始化仓库后可能会有多余的文件（node_modules）添加至仓库，我们需要新建一个`.gitignore`文件来将他们过滤
![多余文件](image-8.png)
![过滤文件](image-9.png)
![提交本地仓库](image-10.png)
![远程仓库提交](image-11.png)
推送之后可以在Github中看到相应的仓库
![我的仓库](image-12.png)

2. 在 Gitee 中新建一个空的仓库

![Gite建库](image-13.png)
![配置空仓库](image-14.png)
创建之后会自动打开这个仓库
![管理](image-15.png)
![仓库开源](image-16.png)

### **配置项目密钥**

1. 使用 `Git bash` 生成部署密钥，打开 Git bash 然后输入如下命令来生成你自己的部署密钥，输入之后回车三次，会在 C 盘下的 `用户/XXX` （XXXX为当前系统用户名）目录的`.ssh`文件夹中生成 `id_rsa` 文件（私钥）和 `id_rsa.pub` 文件（公钥）。
```bash
    ssh-keygen -t rsa -C "你的邮箱"
```
![密钥生成](image-20.png)
![密钥目录](image-21.png)

2. 部署公钥，打开 `id_rsa.pub` 文件，将里面的内容复制，再打开 Gitee 部署公钥。

![进入个人设置](image-22.png)
![部署公钥](image-23.png)

Github 的公钥部署同理。

3. 部署私钥，打开 `id_rsa` 文件，将里面的内容复制，再打开上传的 Github 项目仓库，在设置里部署私钥。

![部署私钥](image-24.png)
**注意：这里一定要在左栏 Secrets 目录下的 Action 进行添加，不然后边会出现问题且非常难察觉到，我之前就踩了这个坑，弄了好久才弄好😥**

![添加私钥](image-25.png)

4. 加密 Gitee 密码，用于后边 Github Action 登录 Gitee ，和上边第三步一样，添加一个新的 Secrets。

![Gitee密码](image-26.png)


### **修改项目部署配置**  
因为我们这个博客项目最终是要部署到 Gitee Pages 的，所以我们要修改一下配置文件。

1. 复制 Gitee 项目仓库主页地址

![主页地址](image-36.png)

2. 打开本地博客项目的 `_config.yml` 文件进行配置（如果不进行配置的话到时 Gitee Pages 展示的页面会没有样式!），在这个配置文件里我们可以看到有许多的配置项，这些配置内容我们后续做主题会用到，具体配置信息[**点这里进行查看**](#config)。

![配置url](image-18.png)

3. 复制 Gitee 仓库的项目地址

![项目仓库地址](image-17.png)

4. 本地项目 `_config.yml` 文件拉到最底下添加如下内容用于将项目部署到 Gitee 仓库
```yml
deploy:
  type: git
  ignore_hidden: false # 添加这个属性值为false
  repo: 
    gitee: git@gitee.com:tao-renjian/hexo-blog.git,master
    github: git@github.com:RemBrolly/MyBlog.git,gh-page
```
`type` 属性固定为git，新增一个 `ignore_hidden` 属性配置,其值为 `false` ，`repo` 属性填 Gitee `仓库地址,仓库分支`（使用SSH方式会比较好一些，我这里还填了一个Github 的地址，用来备份一份静态内容到其另一个分支上，不需要的可以不加），注意看好自己仓库默认的分支名哈，笔者这里是 `master`，有的朋友的可能是 `main` ，自己看一下哈。
![配置部署](image-19.png)

5. 配置一些其他项来解决问题，可以参考这位大佬的博客 `->` [**传送门**](https://blog.csdn.net/qq_35977139/article/details/113764322)  
以下两个配置项在最新版本的 Hexo 中是预留了位置的，找不到的可以使用 VScode 的搜索功能找到。

```yml
    include: 
      - ".github/**/*"
    skip_render: 
      - ".github/**/*"
```
**注意：书写 YML 内容的时候一定要严格注意缩进，不然 YML 无法运行，后边配置 Action 的时候也是如此。**

![配置问题](image-37.png)
![配置问题](image-38.png)

将这两项配置好，后边就不会出现问题了。

**注意：这里的配置更改完之后一定要先上传 Github ，不然使用 Github Action 的时候无法成功部署到 Gitee Pages！**

### **使用 Github Action 进行自动化部署**  

1. 现在本地将项目部署至 Gitee Pages 看看是否有问题，有问题就先解决问题，没问题了就再进行 Action 的配置，而且使用 Action 进行自动化部署的前提是自己先进行第一次部署。
打开终端输入如下的命令进行部署。
```bash
    # 安装部署插件
    npm install hexo-deployer-git --save
    # 之后就可以使用 hexo deploy（或简写 hexo d）将项目部署到gitee远程仓库
```
    进行项目部署:
```bash
    # 生成静态网站文件
    hexo g  
    # 上传到远程仓库
    hexo d  
# 或者使用下面这种一键部署的方式,二选一
    # 1、清除 hexo 的缓存
    hexo clean
    # 2、采用一键部署
    hexo g --d
```

![本地项目部署](image-39.png)
![部署成功](image-40.png)

2. 打开 Gitee 仓库查看静态文件是否已经部署上来，部署成功的话 Gitee 项目仓库就会包含静态文件。

![Gitee 仓库部署成功](image-41.png)

3. 开启 Gitee Pages 功能

![开启Pages](image-42.png)
![开启Pages](image-43.png)
![点击链接](image-44.png)
![部署成功](image-45.png)
如果你的页面打开没有样式的话，尝试刷新一下，还不行的话，就是你的 `_config.yml` 文件没有配置好，去那里看看有没有问题。

4. 开启 Github 仓库的 Action 功能并进行配置.

![开启Action](image-27.png)
![配置Action](image-28.png)

配置内容如下：

```YML
# Action的名字
name: Sync

on:
# 触发条件1:master分支发生push动作时
  push:
    branches: [master]
# 触发条件2:手动按钮
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Sync to Gitee
        uses: wearerequired/git-mirror-action@master
        env:
          # 注意在 Settings->Secrets 配置 GITEE_RSA_PRIVATE_KEY
          SSH_PRIVATE_KEY: ${{ secrets.GITEE_RSA_PRIVATE_KEY }}
        with:
          # 注意替换为你的 GitHub 源仓库地址
          source-repo: git@github.com:RemBrolly/MyBlog.git
          # 注意替换为你的 Gitee 目标仓库地址
          destination-repo: git@gitee.com:tao-renjian/my-blog.git

      - name: Build Gitee Pages
        uses: yanglbme/gitee-pages-action@main
        with:
          # 注意替换为你的 Gitee 用户名
          gitee-username: tao-renjian
          # 注意在 Settings->Secrets 配置 GITEE_PASSWORD
          gitee-password: ${{ secrets.GITEE_PASSWORD }}
          # 注意替换为你的 Gitee 仓库，仓库名严格区分大小写，请准确填写，否则会出错
          gitee-repo: tao-renjian/hexo-blog
          # 要部署的分支，默认是 master，若是其他分支，则需要指定（指定的分支必须存在）
          branch: master

```
以上代码需要更改的内容如下图所标：（仓库地址请使用SSH链接）
![更改的内容](image-46.png)
**关于 Gitee 仓库用户名和仓库名的问题：（不知道是不是大家都有这个问题，反正我这里是有的）**
![Gitee用户名](image-47.png)
![Gitee仓库名](image-48.png)

以上两个问题如果没解决好的话，在运行 Action 文件的时候是会找不到你的 Gitee 仓库的。

5. 用手机微信关注 Gitee 公众号，并绑定自己的 Gitee 账户， 用于接收帐号登录通知，便可以绕过短信验证码校验，不如此做的话运行 Action 文件就会出现如下的问题。

![Alt text](image-31.png)

6. 运行 Action ，编辑完 Action 文件后，使用 `Ctrl+S` 保存后，便会自己运行。

![保存更改](image-49.png)
![运行情况](image-50.png)
![运行情况](image-51.png)
![运行成功](image-52.png)

如果配置没配好的话，到这里会运行失败会有相应的错误警告，这时候需要自己进行查错和修复了，如果有什么问题可以在评论区留言，我看到后会进行答复的。

### 测试自动化是否成功

我们在 Github 或者 VScode 进行内容修改并提交，查看 Gitee Pages 是否会自动进行相应的改变。

![修改文件](image-53.png)
![更改标题](image-54.png)
![提交更改](image-55.png)
![查看运行](image-56.png)
![运行成功](image-57.png)
![查看效果](image-58.png)
![更改成功](image-59.png)

至此，自动部署已经实现成功！虽然过程有点复杂了点，但是今后写博客就可只专注于内容的编辑了，无需每次编辑完内容后都需要手动执行命令进行部署。


> 参考的博客：  
> - [Hexo Gitee Pages 自动部署站点](https://blog.csdn.net/qq_35977139/article/details/113764322)  
> - [🤖 Auto Deploy Gitee Pages by GitHub Action | 无须人为干预，由 GitHub Action 自动部署 Gitee Pages](https://github.com/yanglbme/gitee-pages-action)


**补充：自己使用了一段时间之后，发现有时 Github Action 自动部署的过程可能会稍微久一些也就是可能没有本地运行部署命令来得快，所以如果要追求实时性的话建议还是直接使用本地命令部署，这种自动部署最大的好处就是我们今后可以在一台没有配置 NodeJS 和 Hexo 的电脑上直接更改并部署博客。**





## <span id="catalog"> 项目目录结构说明</span>
- `_config.yml`  
为全局配置文件，网站的很多信息都在这里配置，比如说网站名称，副标题，描述，作者，语言，主题等等。具体可以参考官方文档：https://hexo.io/zh-cn/docs/configuration.html。
- `scaffolds`
骨架文件，是生成新页面或者新博客的模版。可以根据需求编辑，当hexo生成新博客的时候，会用这里面的模版进行初始化。
- `source`
这个文件夹下面存放的是网站的markdown源文件，里面有一个_post文件夹，所有的.md博客文件都会存放在这个文件夹下。现在，你应该能看到里面有一个hello-world.md文件。
- `themes`
网站主题目录，hexo有非常丰富的主题支持，主题目录会存放在这个目录下面。
我们后续会以默认主题来演示，更多的主题参见：https://hexo.io/themes/


## <span id="config">博客配置说明</span>
由于我们这个教程的重点不是如何编写自己的博客，而是怎么把博客搭建起来，所以hexo的细节用法以及各种主题的设置我们就不展开细说了，官网有非常详细的文档和教程可供参考。有机会的话我也会把我的博客配置过程记录下来。这里简单提一下_config.yml的各个字段的含义：
```YML
    # Site
    title: Hexo  # 网站标题
    subtitle:    # 网站副标题
    description: # 网站描述
    author: John Doe  # 作者

    language:    # 语言

    timezone:    # 网站时区, Hexo默认使用您电脑的时区

    # URL
    ## If your site is put in a subdirectory, set url as 'http://yoursite.com/child'
    ## 如果您的网站放在子目录中，请将 url 设置为“http://yoursite.com/child”

    ## and root as '/child/'
    ## 并将根作为“/child/”

    url: http://yoursite.com   # 你的站点Url
    root: /                    # 站点的根目录
    permalink: :year/:month/:day/:title/   # 文章的 永久链接 格式   
    permalink_defaults:        # 永久链接中各部分的默认值

    # Directory   
    # 目录

    source_dir: source     # 资源文件夹，这个文件夹用来存放内容
    public_dir: public     # 公共文件夹，这个文件夹用于存放生成的站点文件。
    tag_dir: tags          # 标签文件夹     
    archive_dir: archives  # 归档文件夹
    category_dir: categories     # 分类文件夹
    category_dir: categories     # 分类文件夹

    code_dir: downloads/code     # Include code 文件夹
    code_dir: downloads/code     # Include code 文件夹

    i18n_dir: :lang              # 国际化（i18n）文件夹
    skip_render:                 # 跳过指定文件的渲染，您可使用 glob 表达式来匹配路径。    

    # Writing
    new_post_name: :title.md  # 新文章的文件名称
    default_layout: post      # 预设布局
    default_layout: post      # 预设布局

    titlecase: false          # 把标题转换为 title case
    external_link: true       # 在新标签中打开链接
    filename_case: 0          # 把文件名称转换为 (1) 小写或 (2) 大写
    render_drafts: false      # 是否显示草稿
    post_asset_folder: false  # 是否启动 Asset 文件夹
    relative_link: false      # 把链接改为与根目录的相对位址    
    future: true              # 显示未来的文章
    highlight:                # 内容中代码块的设置    
    enable: true            # 开启代码块高亮
    line_number: true       # 显示行数
    auto_detect: false      # 如果未指定语言，则启用自动检测
    tab_replace:            # 用 n 个空格替换 tabs；如果值为空，则不会替换 tabs

    # Category & Tag # 类别和标签
    default_category: uncategorized # 未分类

    category_map:       # 分类别名
    tag_map:            # 标签别名

    # Date / Time format
    # 日期/时间格式

    ## Hexo uses Moment.js to parse and display date
    ## Hexo 使用 Moment.js 解析并显示日期

    ## You can customize the date format as defined in
    ## 您可以自定义日期格式，如定义的

    ## http://momentjs.com/docs/#/displaying/format/

    date_format: YYYY-MM-DD     # 日期格式
    date_format: YYYY-MM-DD     # 日期格式

    time_format: HH:mm:ss       # 时间格式    
    time_format: HH:mm:ss       # 时间格式


    # Pagination # 分页
    ## Set per_page to 0 to disable pagination
    ## 将 per_page 设置为 0 以禁用分页

    per_page: 10           # 分页数量
    pagination_dir: page   # 分页目录
    pagination_dir: page   # 分页目录


    # Extensions # 扩展
    ## Plugins: https://hexo.io/plugins/
    ## 插件：https://hexo.io/plugins/

    ## Themes: https://hexo.io/themes/
    theme: landscape   # 主题名称

    # Deployment # 部署
    ## Docs: https://hexo.io/docs/deployment.html

    #  部署部分的设置
    deploy:     

    type: '' # 类型，常用的git 
```

