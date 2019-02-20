<p align="center"><img src="https://laravel.com/assets/img/components/logo-laravel.svg"></p>

## Laravel Passport – 创建REST API认证

## 说明

<p>
在本教程中，我们将了解如何在您的Laravel应用程序中使用Laravel Passport身份验证。我们还将使用Laravel Passport身份验证构建一个简单的产品CRUD（创建、读取、更新和删除）。
<p/>

<p>
Laravel已经提供了传统的登录表单认证，但是如果您想使用API呢？API使用令牌对用户进行身份验证，因为它们不使用会话。当用户通过API登录时，会生成一个令牌并将其发送给用于身份验证的用户。Laravel提供了使用API认证的通行证，没有任何困难。
<p/>
<p>
让我们看看如何在Laravel应用程序中设置和配置用于API身份验证的Laravel Passport和Restful API。
<p/>

## 创建一个项目

``` shell
composer create-project --prefer-dist laravel/laravel passport
```

## 安装所需的包

``` shell
composer require laravel/passport
```

## 服务提供者

<p>
我使用的是Laravel 5.6，它是Laravel的最新版本，它使用包自动发现自动注册包。如果您使用的是Laravel 5.4或更低版本，则需要在config/app.php文件中添加服务提供者。因此，打开文件并将服务提供者添加到提供者数组中。
</p>

``` php
'providers' => [
    ....
    Laravel\Passport\PassportServiceProvider::class,
]
```

## 数据库安装

<p>在.env文件中设置数据库凭据。LaravelPassport为需要在我们的数据库中的Passport表提供了迁移。Passport迁移用于存储令牌和客户端信息。运行migration命令将架构迁移到数据库。</p>

``` shell
php artisan migrate
```

<p>接下来，需要使用下面的命令安装Passport。它将生成生成生成秘密访问令牌所需的加密密钥。</p>

``` shell
php artisan passport:install
```

## Passport配置

<p>在这一步中，我们需要在Laravel应用程序中进行更改，以完成Passport配置。</p>

### app/User.php
### 添加Laravel\Passport\HasApiTokens到User模型.

``` php
<?php

namespace App;

use Illuminate\Notifications\Notifiable;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Laravel\Passport\HasApiTokens;

class User extends Authenticatable
{
    use HasApiTokens, Notifiable;

    /**
     * The attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = [
        'name', 'email', 'password',
    ];

    /**
     * The attributes that should be hidden for arrays.
     *
     * @var array
     */
    protected $hidden = [
        'password', 'remember_token',
    ];
}
```

### AuthServiceProvider
### 添加 Passport::routes 方法到 AuthServiceProvider. 他会生成所需路由. 路径 app/Providers/AuthServiceProvider.php .

``` php
<?php

namespace App\Providers;

use Illuminate\Foundation\Support\Providers\AuthServiceProvider as ServiceProvider;
use Laravel\Passport\Passport;

class AuthServiceProvider extends ServiceProvider
{
    /**
     * The policy mappings for the application.
     *
     * @var array
     */
    protected $policies = [
        'App\Model' => 'App\Policies\ModelPolicy',
    ];

    /**
     * Register any authentication / authorization services.
     *
     * @return void
     */
    public function boot()
    {
        $this->registerPolicies();

        Passport::routes();
    }
}
```