<p align="center)<img src="https://laravel.com/assets/img/components/logo-laravel.svg)</p>

## HTTP访问控制（CORS）

跨域资源共享(CORS) 是一种机制，它使用额外的 HTTP 头来告诉浏览器  让运行在一个 origin (domain) 上的Web应用被准许访问来自不同源服务器上的指定的资源。当一个资源从与该资源本身所在的服务器不同的域、协议或端口请求一个资源时，资源会发起一个跨域 HTTP 请求。

## 解决方案

- 新建 app\Http\Middleware\CORS.php，利用laravel中间件解决

``` php
<?php

namespace App\Http\Middleware;
use Closure;

class CORS
{
    public function handle($request, Closure $next)
    {
        $response = $next( $request );

        $response->header( 'Access-Control-Allow-Origin', '*' );
        $response->header( 'Access-Control-Allow-Headers', 'Origin, Content-Type, Cookie, Accept' );
        $response->header( 'Access-Control-Allow-Methods', 'GET, POST, PATCH, PUT, DELETE, OPTIONS' );
        $response->header( 'Access-Control-Allow-Credentials', 'false' );

        return $response;
    }
}
```
- 解决前后端分离应用跨域请求利器 —— Laravel CORS 扩展包

github 地址：https://github.com/barryvdh/laravel-cors

- 我想在我的服务器上添加CORS支持[文章](https://enable-cors.org/server.html)
    - [Apache](https://enable-cors.org/server_apache.html)
    - [App Engine](https://enable-cors.org/server_appengine.html)
    - [ASP.NET](https://enable-cors.org/server_aspnet.html)
    - [AWS API Gateway](https://enable-cors.org/server_awsapigateway.html)
    - [Caddy](https://enable-cors.org/server_caddy.html)
    - [CGI Scripts](https://enable-cors.org/server_cgi.html)
    - [ExpressJS](https://enable-cors.org/server_expressjs.html)
    - [Flask](https://enable-cors.org/server_flask.html)
    - [IIS6](https://enable-cors.org/server_iis6.html)
    - [IIS7](https://enable-cors.org/server_iis7.html)
    - [Spring Boot Applications in Kotlin](https://enable-cors.org/server_spring-boot_kotlin.html)
    - [Meteor](https://enable-cors.org/server_meteor.html)
    - [nginx](https://enable-cors.org/server_nginx.html)
    - [Perl](https://enable-cors.org/server_perl.html)
    - [PHP](https://enable-cors.org/server_php.html)
    - [ColdFusion](https://enable-cors.org/server_coldfusion.html)
    - [Tomcat](https://enable-cors.org/server_tomcat.html)
    - [Virtuoso](https://enable-cors.org/server_virtuoso.html)
    - [WCF](https://enable-cors.org/server_wcf.html)


更多参考
- https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS
- https://laravelacademy.org/post/9273.html
- https://enable-cors.org/index.html
- https://en.wikipedia.org/wiki/Cross-origin_resource_sharing
- https://en.wikipedia.org/wiki/Same-origin_policy

