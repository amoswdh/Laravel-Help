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

<p>执行该命令会显示以下选项，按照个人需要设置即可.</p>

``` php
$ composer init


  Welcome to the Composer config generator



This command will guide you through creating your composer.json config.

// 1. 输入项目命名空间
// 注意<vendor>/<name> 必须要符合 [a-z0-9_.-]+/[a-z0-9_.-]+
Package name (<vendor>/<name>) [dell/htdocs]: yourname/projectname

// 2. 项目描述
Description []: 这是一个测试

// 3. 输入作者信息，可以直接回车
Author [guanguans <53222411@qq.com>, n to skip]:

// 4. 输入最低稳定版本，stable, RC, beta, alpha, dev
Minimum Stability []: dev

// 5. 输入项目类型，
Package Type (e.g. library, project, metapackage, composer-plugin) []: library

// 6. 输入授权类型
License []:
> Define your dependencies.

// 7. 输入依赖信息
Would you like to define your dependencies (require) interactively [yes]?

// 如果需要依赖，则输入要安装的依赖
Search for a package: php

// 输入版本号
Enter the version constraint to require (or leave blank to use the latest version): >=5.4.0

// 如需多个，则重复以上两个步骤

// 8. 是否需要require-dev，
Would you like to define your dev dependencies (require-dev) interactively [yes]?

// 操作同上
{
    "name": "guanguans/uploadfile",
    "description": "一个通用文件上传包",
    "type": "library",
    "require": {
        "php": ">=5.4"
    },
    "require-dev": {
        "php": ">=5.4"
    },
    "license": "MIT",
    "authors": [
        {
            "name": "guanguans",
            "email": "yzmguanguan@gmail.com"
        }
    ],
    "minimum-stability": "dev"
}

// 9. 是否生成composer.json
Do you confirm generation [yes]? yes
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
