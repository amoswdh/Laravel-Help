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

### config/auth.php
### 在 config/auth.php 文件, 设置Passport为API默认认证

``` php
return [

    'guards' => [
        'web' => [
            'driver' => 'session',
            'provider' => 'users',
        ],

        'api' => [
            'driver' => 'passport',
            'provider' => 'users',
        ],
    ],

]
```

### 创建路由

<p>添加路由在 routes/api.php 文件中.</p>

``` php
Route::post('login', 'PassportController@login');
Route::post('register', 'PassportController@register');

Route::middleware('auth:api')->group(function () {
    Route::get('user', 'PassportController@details');

    Route::resource('products', 'ProductController');
});
```

### 创建认证控制器

``` shell
php artisan make:controller PassportController
```

<p>复制以下内容到 app/Http/Controllers/PassportController.php</p>

``` php
<?php

namespace App\Http\Controllers;

use App\User;
use Illuminate\Http\Request;

class PassportController extends Controller
{
    /**
     * Handles Registration Request
     *
     * @param Request $request
     * @return \Illuminate\Http\JsonResponse
     */
    public function register(Request $request)
    {
        $this->validate($request, [
            'name' => 'required|min:3',
            'email' => 'required|email|unique:users',
            'password' => 'required|min:6',
        ]);

        $user = User::create([
            'name' => $request->name,
            'email' => $request->email,
            'password' => bcrypt($request->password)
        ]);

        $token = $user->createToken('TutsForWeb')->accessToken;

        return response()->json(['token' => $token], 200);
    }

    /**
     * Handles Login Request
     *
     * @param Request $request
     * @return \Illuminate\Http\JsonResponse
     */
    public function login(Request $request)
    {
        $credentials = [
            'email' => $request->email,
            'password' => $request->password
        ];

        if (auth()->attempt($credentials)) {
            $token = auth()->user()->createToken('TutsForWeb')->accessToken;
            return response()->json(['token' => $token], 200);
        } else {
            return response()->json(['error' => 'UnAuthorised'], 401);
        }
    }

    /**
     * Returns Authenticated User Details
     *
     * @return \Illuminate\Http\JsonResponse
     */
    public function details()
    {
        return response()->json(['user' => auth()->user()], 200);
    }
}
```

### 创建 Product CRUD

<p>创建 product CRUD. 使用以下命令去创建Product模型, 迁移文件和控制器.</p>

``` shell
php artisan make:model Product -mc
```

<p>它会生成一个数据库迁移文件 create_products_table.php 在 database/migrations 目录. 使用下面代码更新 up 方法中的代码.</p>

``` php
public function up()
{
    Schema::create('products', function (Blueprint $table) {
        $table->increments('id');
        $table->integer('user_id');
        $table->string('name');
        $table->integer('price');
        $table->timestamps();

        $table->foreign('user_id')
            ->references('id')
            ->on('users');
    });
}
```

<p>Product model添加fillable信息.</p>

``` php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Product extends Model
{
    protected $fillable = [
        'name', 'price'
    ];
}
```

### 执行数据库迁移

``` shell
php artisan migrate
```

<p>将以下关联信息放到 app/User.php 文件.</p>

``` php
public function products()
{
    return $this->hasMany(Product::class);
}
```

<p>打开 ProductController.php 文件在 app/Http/Controllers 目录. 复制以下代码进 Product Controller.</p>

``` php
<?php

namespace App\Http\Controllers;

use App\Product;
use Illuminate\Http\Request;

class ProductController extends Controller
{
    public function index()
    {
        $products = auth()->user()->products;

        return response()->json([
            'success' => true,
            'data' => $products
        ]);
    }

    public function show($id)
    {
        $product = auth()->user()->products()->find($id);

        if (!$product) {
            return response()->json([
                'success' => false,
                'message' => 'Product with id ' . $id . ' not found'
            ], 400);
        }

        return response()->json([
            'success' => true,
            'data' => $product->toArray()
        ], 400);
    }

    public function store(Request $request)
    {
        $this->validate($request, [
            'name' => 'required',
            'price' => 'required|integer'
        ]);

        $product = new Product();
        $product->name = $request->name;
        $product->price = $request->price;

        if (auth()->user()->products()->save($product))
            return response()->json([
                'success' => true,
                'data' => $product->toArray()
            ]);
        else
            return response()->json([
                'success' => false,
                'message' => 'Product could not be added'
            ], 500);
    }

    public function update(Request $request, $id)
    {
        $product = auth()->user()->products()->find($id);

        if (!$product) {
            return response()->json([
                'success' => false,
                'message' => 'Product with id ' . $id . ' not found'
            ], 400);
        }

        $updated = $product->fill($request->all())->save();

        if ($updated)
            return response()->json([
                'success' => true
            ]);
        else
            return response()->json([
                'success' => false,
                'message' => 'Product could not be updated'
            ], 500);
    }

    public function destroy($id)
    {
        $product = auth()->user()->products()->find($id);

        if (!$product) {
            return response()->json([
                'success' => false,
                'message' => 'Product with id ' . $id . ' not found'
            ], 400);
        }

        if ($product->delete()) {
            return response()->json([
                'success' => true
            ]);
        } else {
            return response()->json([
                'success' => false,
                'message' => 'Product could not be deleted'
            ], 500);
        }
    }
}
```

### 测试

<p>完成以上步骤，我们可以使用API测试工具Postman做接口的测试.</p>

### 注册API

<p><img src="https://github.com/amoswdh/Laravel-Help/blob/laravel-passport/blog/example/images/register-1.png"></p>
