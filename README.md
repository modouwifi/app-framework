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

```
./___
    |--init
    |--manifest.json
    |--commands.json               // 可选的，扩展命令配置文件
```

* `init` 必须接受以下命令。`commands.json` 文件中描述可接受的扩展命令。

```
    start       : 启动，实现启动本应用的操作
    stop        : 停止， 停止本应用的操作
    status      : 查询状态，查询本应用的状态
    config      : 配置， 对本应用进行配置
    install     : 安装，本应用安装后的一次性操作
    uninstall   : 卸载，本应用被卸载前的一次性操作
```

* `manifest.json` 文件包含本APP的ID和相关信息, 相关字段参见下文样例：

```js
{
    "package_id"      : "com.modou.sample",       // 应用ID-必填字段
    "name"            : "welcome-page",           // 应用的名字
    "author"          : "魔豆开发团队",           // 应用作者
    "author_mail"     : "rd@mochui.net",          // 应用作者联系邮箱
    "homepage"        : "www.modouwifi.com",      // 应用相关的网页
    "version"         : "0.1",                    // 当前应用版本
    "os_version"      : "0.5.28",                 // 依赖的系统版本
    "release_date"    : "2014.08.08",             // 应用发布日期
    "icon"            : "./res/icon_111X111.png", // 图标文件
    "location"        : "internal",               // 安装位置 (internal,external,both)
    "description"     : "这是一个样例",           // 应用的描述
    "instruction"     : "1.xxxx; 2.xxxxx"         // 应用的安装或使用步骤指引
}
```
* `commands.json` 文件描述 `init` 脚本可接受的扩展命令, 相关字段参见下文样例：
```js
[
  {
    "name": "背光设置",
    "type": "group",
    "nodes": [
      {
        "name": "背光熄灭",
        "type": "command",
        "action": "rlease_backlight",
        "args": [
          {
            "name": "FileName",
            "type": "number"
          },
          {
            "name": "Count",
            "type": "number"
          }
        ],
        "results": [
          {
            "message": "do command success",
            "code": 0
          },
          {
            "message": "do command failure",
            "code": -1
          }
        ]
      },
      {
        "name": "背光长亮",
        "type": "command",
        "action": "lock_backlight"
      }
    ]
  },
  {
    "name": "开启背光",
    "type": "command",
    "action": "backlight_on"
  },
  {
    "name": "关闭背光",
    "type": "command",
    "action": "backlight_off"
  }
]
```


## 应用版本号规范

应用的版本号必须符合 [语义化版本2.0.0](http://semver.org/lang/zh-CN/) 规范

## 已有的应用程序列表

### [hdns](https://github.com/modouwifi/hdns)

魔豆上的高级 DNS

### [modou-samba](https://github.com/modouwifi/modou-samba)

魔豆上的 Samba 文件共享

## 让图标显示在Launcher界面的方法

Launcher 程序读取配置文件，显示 App Icon, 并且在用户点击 Icon 时执行命令。
### 1. 配置文件的格式
    以 JSON 配置格式，每个文件对应一个程序。
```js
{
    "name" : "AppName",
    "icon" : "/path/icon.png",
    "iconPressed" : "/patb/icon_p.png",
    "exec" : "/path/app",
    "msgNum" : 4
}
 ```

### 2. 配置文件的命名。
        1) 必须有 .conf 后缀.
        2) 尽量以程序名字+.conf 作为文件名，避免与其他程序的配置文件冲突。

### 3. 配置文件的目录。
       配置文件放在  /data/conf/launcher/conf.d  目录。
       从这个目录删除配置文件，Launcher不再显示相应的图标。
  
### 4. 图标规格:
    111 × 111
    不符合规格的图片显示不了。
