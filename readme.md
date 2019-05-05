<p align="center"><img src="https://laravel.com/assets/img/components/logo-laravel.svg"></p>

## Supervisor配置
<p>
Supervisor为Linux操作系统提供的进程监视器，将会在失败时自动重启queue:listen或queue:work命令，要在Ubuntu上安装Supervisor，使用如下命令：
</p>

``` php
sudo apt-get install supervisor
```
<p>
Supervisor配置文件通常存放在/etc/supervisor/conf.d目录，在该目录中，可以创建多个配置文件指示Supervisor如何监视进程，例如，让我们创建一个开启并监视queue:work进程的laravel-worker.conf文件：
</p>

``` php
[program:laravel-worker]
process_name=%(program_name)s_%(process_num)02d
command=php /home/forge/app.com/artisan queue:work sqs --sleep=3 --tries=3 --daemon
autostart=true
autorestart=true
user=forge
numprocs=8
redirect_stderr=true
stdout_logfile=/home/forge/app.com/worker.log
```

<p>
在本例中，numprocs指令让Supervisor运行8个queue:work进程并监视它们，如果失败的话自动重启。配置文件创建好了之后，可以使用如下命令更新Supervisor配置并开启进程：
</p>

``` php
sudo supervisord -c /etc/supervisord.conf
sudo supervisorctl -c /etc/supervisor/supervisord.conf
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl start laravel-worker:*
```

## 邮件拉取服务配置
``` php
[program:email-imap]
process_name=%(program_name)s_%(process_num)02d
command=/export/servers/php/bin/php /export/data/tomcatRoot/customer.orderplus.com/artisan queue:work --queue=imap --sleep=1 --tries=3 --daemon
numprocs=2
autostart=true
autorestart=true
startsecs=3
startretries=3
user=nobody
log_stdout=true
log_stderr=true
logfile=/export/logs/support.orderplus.com/email-imap.log
```

# 常见错误

- email-imap:email-imap_00: ERROR (spawn error) / FATAL     Exited too quickly (process log may have details)

#### 排查启动过程中出现的错误引起启动失败
``` php
supervisorctl tail -f email-imap:email-imap_00 stdout
```
<p>该命令用于排查启动失败引起的错误</p>
<p>问题解决后<b>必须</b>以此执行以下命令：</p>

``` php
supervisorctl update
supervisorctl reload
supervisorctl status
```
