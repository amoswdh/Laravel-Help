<p align="center"><img src="https://laravel.com/assets/img/components/logo-laravel.svg"></p>

## Laravel 的数据库迁移 Migrations

<p>数据库迁移就像是数据库的版本控制，可以让你的团队轻松修改并共享应用程序的数据库结构。迁移通常与 Laravel 的数据库结构生成器配合使用，让你轻松地构建数据库结构。如果你曾经试过让同事手动在数据库结构中添加字段，那么数据库迁移可以让你不再需要做这样的事情。</p>

## 生成迁移

``` php
php artisan make:migration create_users_table --create=users

php artisan make:migration add_votes_to_users_table --table=users
```

## 运行迁移

``` php
php artisan migrate
```

## 回滚迁移

``` php
php artisan migrate:rollback
```

## 官网
- [官方迁移文档](https://learnku.com/docs/laravel/5.7/migrations/2291)
