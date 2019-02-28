<p align="center"><img src="https://laravel.com/assets/img/components/logo-laravel.svg"></p>

## 从零开始Laravel包开发指南

<p>包开发适用与大部分PHP框架及项目.通用、易于复用.</p>

### 为什么使用Laravel包开发?

- 更好的编码技巧
- 更多（和更好）的工作
- 节省时间

### 创建新程序包目录

``` php
mkdir laravel-log-enhancer && cd laravel-log-enhancer
```

### Git初始化 / Git clone

<p>初始化git存储库。我们需要GitHub repo才能将软件包放到网上。</p>

<p>通常，我会预先在Github新建一个项目，使用Git clone 将项目拉取到预先建立好的包目录.</p>

``` php
git init
```

### composer.json

<p>每个包都以一个composer.json文件开头。让我们创建它。创建它的最简单方法是触发init命令。</p>
<p>或者您可以从此存根中复制它并相应地进行更改。</p>

``` php
composer init
```

### 命名空间和自动加载

<p>我们将把我们包的主要代码放在src目录中。</p>

``` php
mkdir src && cd src
```

<p>顺便说一下，就像Laravel Web应用程序开发一样，这里没有关于目录结构的硬规则，但是遵循一个公共的约定。</p>

### 如何发布包

1. push代码到Github
2. 前往Packagist，创建一个帐户或登录，单击提交，指定GitHub repo URL;

### 如何在本地使用该包

``` php
composer require amoswdh/packagename
```
