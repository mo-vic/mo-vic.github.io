---
title: hexo维护和使用笔记
date: 2021-02-16 12:05:31
tags:
	- Hexo
categories:
	- [工具, 个人网站]
mathjax: true
---

[toc]

<!-- toc -->

## Hexo个人博客小站维护和使用笔记

才想起自己很久前搭的这个小破站，当时没有写下安装和使用的笔记，导致现在连博客都不知道怎么发了。今天重新回忆一下当时安装和使用的流程，并整理成笔记，方便日后查阅。



## 一、 Hexo的安装

`Hexo`的安装需要先安装`Node.js`和`Git`，具体安装步骤参考：https://hexo.io/zh-cn/docs/。安装过程算是比较简单。



## 二、建站

建站教程参考链接：https://hexo.io/zh-cn/docs/setup



安装 `Hexo` 完成后，执行下列命令，`Hexo` 将会在指定文件夹中新建所需要的文件。

```shell
$ hexo init <folder>
$ cd <folder>
$ npm install
```

文件目录结构：

**scaffolds**：模板文件夹，新建文章时，`Hexo`会根据模板来建立`.md`文件。

**source**：存放用户资源的地方，除了`_posts`文件夹之外，开头命名为`_`（下划线）的文件/文件夹和隐藏文件将会被忽略。

**themes**：主题文件夹。


<!--more-->


## 三、配置

根目录下的`_config.yml`为配置文件，配置项较多，可参考：https://hexo.io/zh-cn/docs/configuration



## 四、写作

### 4.1 新建文章

执行下列命令创建一篇新文章或者新的页面：

```shell
$ hexo new [layout] <title>
```

`Hexo`默认以标题作为文件名称，但是可以通过设置目录下的`_config.yml`配置文件中的`new_post_name`参数来改变默认的文件名称格式。



### 4.2 Front-matter

Front-matter是文件最上方以 `---` 分隔的区域，用于指定个别文件的变量。其中`updated`参数用于指定更新日期。

多个并列标签的设置方法：

```yaml
tags:
	- 游戏
	- 电影
```

多个并列分类以及多级分类的设置方法：

```yaml
categories:
	- [日常, 杂项]
	- [学习]
```

其中，`杂项`是`日常`的子分类，`学习`没有子分类。



### 4.3 标签插件

用于插入图片、代码块以及引用等，大部分与`MarkDown`格式不同（代码块格式可以使用MarkDown格式）。



### 4.4 文章资源文件夹

文章资源文件夹用于存放除了文章以外的所有资源文件，例如图片等。通过将`_config.yml`配置文件中的`post_asset_folder`选项设置为`true`来打开资源文件管理功能：

```yaml
post_asset_folder: true
```

当资源文件管理功能打开后，`Hexo`会在每一次通过`hexo new [layout] title`命令创建新文章时自动创建一个文章同名文件夹，将图片等资源文件放在该资源文件夹中，即可通过相对路径在文章中进行引用，即`{% asset_img filename.png %}`（若直接使用`![title](filename.png)`插入图片，由于`.md`文件跟图片资源文件还隔着一层目录，`Typora`编辑器上没法显示出图片；同时，以这种方式插入图片，会导致图片在主页无法显示，只有点击进入文章页面才能显示图片）。



### 4.5 写作流程

```shell
# 1. 新建文章
$ hexo new [layout] <title>

# 2. 设置分类和标签

# 3. 本地预览

# 4. 部署

# 5. 版本管理
```



## 五、插件

`Hexo`插件的安装方法：

```shell
$ npm install `plugin` --save
```



1. [hexo-image-link](https://github.com/cocowool/hexo-image-link)：用于将`MarkDown`图片路径转换成`asset_img`语法格式，使得在`Hexo`和`Typora`两边都能够正常显示图片。

   ```
   ![image file label](./markdown-file-name/local-image.png) -> {% asset_img label local-image.png %}
   ```

   实际使用中发现如果加上图片路径最前面的`./`会导致出现`Markdown Image Path does not exists!`错误，去掉图片路径最前面的`./`对`Typora`编辑器引用图片没有影响：

   ```
   ![image file label](markdown-file-name/local-image.png) -> {% asset_img label local-image.png %}
   ```

   ![图片测试](hexo维护和使用笔记/banner.jpg)

   

2. [hexo-filter-mathjax](https://github.com/next-theme/hexo-filter-mathjax)：该插件不需要安装任何其他的数学插件，渲染引擎可以使用[hexo-renderer-marked](https://github.com/hexojs/hexo-renderer-marked)。包含公式的文章需要在`.md`文件前面的Front-matter中打开`mathjax`：

   ```YAML
   ---
   mathjax: true
   ---
   ```

   手机端可以比较完美显示公式，`$$`公式块过长会自动加入滚动条，`hexo-math`插件使用`{% %}`tag插入公式，而我比较习惯于使用`$`或`$$`插入公式，所以选择了[hexo-filter-mathjax](https://github.com/next-theme/hexo-filter-mathjax)插件，公式的换行符`\\`需要在`\begin{array} \end{array}`之类的环境中才能够生效。
   $$
   \mathbf{E} = mc^2
   $$

3. [hexo-toc](https://github.com/bubkoo/hexo-toc)：用于插入目录。与`Typora`插入目录不同，[hexo-toc](https://github.com/bubkoo/hexo-toc)不使用`[toc]`。使用`[toc]`标签会导致页面出现一个莫名其妙的纯文本`[toc]`，看着很不舒服。



4. [hexo-tag-mmedia](https://github.com/u2sb/hexo-tag-mmedia)：媒体插入插件。



## 六、部署

使用`Travis CI`将`Hexo`博客部署到GitHub Pages上：

1. 新建一个仓库，以`<GitHub用户名>.github.io`命名；

2. 将`Hexo`站点文件夹推送到新建的`GitHub`仓库中；

3. 将`Travis CI`添加到`GitHub`账户中；

4. 前往`GitHub`的`Application settings`，配置`Travis CI`的权限使其能够访问`GitHub`仓库；

5. 到`GitHub`账户设置中，选择`Developer settings -> Personal access tokens -> Generate new token`，只勾选`repo`的权限并生成一个新的`Token`；

6. 前往`Travis CI`，在仓库的设置页面中，新建一个环境变量，变量名为`GH_TOKEN`，值为生成的`Token`，确保`DISPLAY VALUE IN BUILD LOG`不被勾选避免`Token`泄漏；

7. 在`Hexo`站点文件夹中新建一个`.travis.yml`文件：

   ```yaml
   sudo: false
   language: node_js
   node_js:
     - 10 # use nodejs v10 LTS
   cache: npm
   branches:
     only:
       - source # 只构建你的站点源文件分支
   script:
     - hexo generate # generate static files
   deploy:
     provider: pages
     skip-cleanup: true
     github-token: $GH_TOKEN
     keep-history: true
     on:
       branch: master # 部署在你的master分支，确保GitHub Pages的部署分支为master
     local-dir: public
   ```


9. 将`.travis.yml`推送到`GitHub`仓库中；
10. 前往`GitHub`仓库，修改`GitHub Pages`的部署分支为`master`；
11. 创建`source`站点源文件分支，使用`Git`做好版本控制；
12. 前往`Travis CI`，在仓库的设置页面中，打开`Build pushed branches`选项。



## 七、npm

`package.json`类似于`requirements.txt`。



## 八、存在的问题

1. [hexo-renderer-marked](https://github.com/hexojs/hexo-renderer-marked)不能渲染双等号==高亮==语法；
2. `Typora`的内部链接可能存在bug，如：跳转至[五、插件](#五、插件)，在`Typora`中跳转到了[六、部署](#六、部署)。实际上不管跳转目标怎么写，似乎都会跳转到[六、部署](#六、部署)。其次，`Hexo`在生成目录时，标题中的中文顿号`、`会被转换成`-`，大写字母会被转换成小写字母，因此当标题中不存在中文顿号和大写字母时，内部链接才能同时在`Hexo`和`Typora`中生效。