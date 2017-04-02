---
title: Hexo图片处理插件
date: 2017-01-20 14:56:08
categories: Hexo
tags: [hexo,image,图片,插件]
---
<Excert in the index | 首页摘要>
Hexo博客中图片处理插件，本地存放图片方案。
** hexo-asset-image **
<!-- more -->
<the rest of contents | 余下全文>

### 插件名称及地址
> GitHub地址：[hexo-asset-image](https://github.com/CodeFalling/hexo-asset-image "hexo-asset-image")

### 安装插件
```
npm install hexo-asset-image --save
```
### 配置文件
_config.yml文件中设置配置节
```
post_asset_folder:true
```
### 测试
新建博客，会生成一个md文件，一个博客同名的文件夹。结构如下：
```
test
├── apppicker.jpg
├── logo.jpg
└── rules.jpg
test.md
```
### 博客中使用图片
```
![logo](test/logo.jpg)
```
其中** [] **里面不写文字则没有图片标题
生成的结构为：
```
public/2016/3/9/test
├── apppicker.jpg
├── index.html
├── logo.jpg
└── rules.jpg
```
同时生成的html是
```
<img src="/2017/1/20/test/logo.jpg" alt="logo">
```
而不是
```
<img src="test/logo.jpg" alt="logo">
```
### 注意，图片引用方式
通过常规的 markdown 语法和相对路径来引用图片和其它资源可能会导致它们在存档页或者主页上显示不正确。在Hexo2时代，社区创建了很多插件来解决这个问题。但是，随着Hexo3的发布，许多新的标签插件被加入到了核心代码中。这使得你可以更简单地在文章中引用你的资源。
```
{% asset_path slug %}
{% asset_img slug [title] %}
{% asset_link slug [title] %}
```
比如：当开启了上述的文章资源文件夹功能后，把一个example.jpg图片放到资源文件夹中，如果通过使用相对路径的常规 markdown 语法 \[](/example.jpg)，它将 不会 出现在首页上。（** 但是它会在文章中按你期待的方式工作 **）。
正确的引用图片方式是使用下列的标签插件而不是markdown。
```
{% asset_img example.jpg This is an example image %}
```
![Markdown语法添加图片](example.jpg)
![MarkdownPad 2添加图片](http://i.imgur.com/cyoZpdV.jpg)
{% asset_img example.jpg 资源插件添加图片 %}