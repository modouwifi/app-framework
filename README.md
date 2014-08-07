# 应用程序开发规范

## 概述
本文档描述魔豆路由器的应用程序开发规范。供开发者在开发适用于魔豆路由器的扩展应用程序时参考适用。

## 应用程序的定义
魔豆路由器的扩展应用程序是指：可以在魔豆路由器上运行的，独立的，可以通过魔豆路由器应用程序框架接口进行操作的软件包。

对软件包的操作包括：云端安装，本地上传安装，启动，停止，卸载，查询状态，配置。

软件包的内容可以包括：对应魔豆路由器芯片型号的二进制可执行程序，shell/lua语言的脚本，文本文件，图片、字体等资源文件，
或本用程序可以自行处理的其他文件格式。

## 应用程序需要遵循的规范

* 应用程序最终发布包的根目录下应包含以下目录树中的文件，文件名区分大小写
<pre>
    ./___
        |--init 
        |--manifest.json
</pre> 

* init脚本应能够接受以下参数并实现对应的操作：
<pre>
    start       : 启动，实现启动本应用的操作
    stop        : 停止， 停止本应用的操作
    status      : 查询状态，查询本应用的状态
    config      : 配置， 对本应用进行配置
    install     : 安装，本应用安装后的一次性操作
    uninstall   : 卸载，本应用被卸载前的一次性操作
</pre>

* manifest.json文件包含本APP的ID和相关信息, 相关字段参见下文样例：
<pre>
{
    "package_id"      : "com.modou.sample",       // 应用ID
    "name"            : "welcome-page",           // 应用的名字
    "author"          : "魔豆开发团队",           // 应用作者
    "author_mail"     : "rd@mochui.net",          // 应用作者联系邮箱
    "homepage"        : "www.modouwifi.com",      // 应用相关的网页
    "version"         : "0.1",                    // 当前应用版本
    "release_date"    : "2014.08.08",             // 应用发布日期
    "icon"            : "./res/icon_111X111.png", // 图标文件
    "location"        : "external",               // 安装位置 (internal,external,both)
    "description"     : "这是一个样例",           // 应用的描述
    "instruction"     : "1.xxxx; 2.xxxxx"         // 应用的安装或使用步骤指引
}
</pre>

## 应用版本号规范

应用的版本号必须符合 [语义化版本2.0.0](http://semver.org/lang/zh-CN/) 规范

## 已有的应用程序列表

### [hdns](https://github.com/modouwifi/hdns)

魔豆上的高级 DNS

### [modou-samba](https://github.com/modouwifi/modou-samba)

魔豆上的 Samba 文件共享
