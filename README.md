## SSPanel-Metron主题，目前由@BobS9维护开发中。

联系方式：[@BobS9](https://t.me/BobS9)

#### 1.连接 SSH 安装宝塔面板

#### 2.宝塔面板安装环境, 推荐使用 PHP 7.2、MySQL 5.6、Nginx 1.16

#### 3.宝塔面板创建网站, 域名等信息自行填写

#### 4.连接 SSH 下载源码

`cd /www/wwwroot/你的网站文件夹名`

#### 5.使用composer安装依赖


```shell
git config core.filemode false && wget https://getcomposer.org/installer -O composer.phar && php composer.phar && php composer.phar install
```


#### 6.复制配置文件

```shell

cp config/.config.example.php config/.config.php

cp config/.metron_setting.example.php config/.metron_setting.php

cp config/appprofile.example.php config/appprofile.php
```

.config.php设置后执行`php xcat initQQWry` 下载IP解析库

#### 8.网站设置

打开 宝塔面版 > 网站 > 你的网站


    在 网站目录 里取消勾选 防跨站攻击，运行目录里面选择 /public，点击保存。

在 伪静态 中填入下面内容，然后保存


```shell
location / {
try_files $uri /index.php$is_args$args;
}
```

#### 9.在SSH里的网站目录下执行，给网站文件755权限

```shell
cd ../
chmod -R 755 你的文件夹名/
chown -R www:www 你的文件夹名/
```

#### 10.数据库操作

首次迁移: 导入网站目录下的`sql/metron.sql` 文件

将数据库user表里的全部用户的theme列改为metron，使用phpmyadmin执行这条sql语句:
```sql
UPDATE user SET theme='metron'
```

#### 11.自行编辑config文件

.metron_setting.php 中务必设置授权码 (从bot获取)

    .config.php 中设置 $_ENV['theme'] = 'metron';


### 使用宝塔面板的计划任务配置
```
每日任务 (必须)
任务类型：Shell 脚本
任务名称：自行填写
执行周期：每天 0 小时 0 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat Job DailyJob

检测任务 (必须)
任务类型：Shell 脚本
任务名称：自行填写
执行周期：N分钟 1 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat Job CheckJob

用户账户相关任务 (必须)
任务类型：Shell 脚本
任务名称：自行填写
执行周期：每小时
脚本内容：php /www/wwwroot/你的网站目录/xcat Job UserJob

定时检测邮件队列 (必须)
任务类型：Shell 脚本
任务名称：自行填写
执行周期：N分钟 1 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat Job SendMail

每日流量报告 (给开启每日邮件的用户发送邮件)
任务类型：Shell 脚本
任务名称：自行填写
执行周期：每天 0 小时 0 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat SendDiaryMail

审计封禁 (建议设置)
任务类型：Shell 脚本
任务名称：自行填写
执行周期：N分钟 1 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat DetectBan

检测被墙 (可选)
任务类型：Shell 脚本
任务名称：自行填写
执行周期：N分钟 1 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat DetectGFW

Radius (可选)
synclogin
任务类型：Shell 脚本
任务名称：自行填写
执行周期：N分钟 1 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat SyncRadius synclogin

syncvpn
任务类型：Shell 脚本
任务名称：自行填写
执行周期：N分钟 1 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat SyncRadius syncvpn

syncnas
任务类型：Shell 脚本
任务名称：自行填写
执行周期：N分钟 1 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat SyncRadius syncnas
自动备份 (可选)

整体备份
任务类型：Shell 脚本
任务名称：自行填写
执行周期：自己设置, 可以设置每30分钟左右
脚本内容：php /www/wwwroot/你的网站目录/xcat Backup full

只备份核心数据
任务类型：Shell 脚本
任务名称：自行填写
执行周期：自己设置, 可以设置每30分钟左右
脚本内容：php /www/wwwroot/你的网站目录/xcat Backup simple
财务报表 (可选)

日报
任务类型：Shell 脚本
任务名称：自行填写
执行周期：每天 0 小时 0 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat FinanceMail day

周报
任务类型：Shell 脚本
任务名称：自行填写
执行周期：每星期 周日 0 小时 0 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat FinanceMail week

月报
任务类型：Shell 脚本
任务名称：自行填写
执行周期：每月 1 日 0 小时 0 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat FinanceMail month
```
