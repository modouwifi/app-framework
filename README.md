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
    |--commands.json                // 可选的，扩展命令配置文件
    |--conf/
           |--data.json             // 应用的配置数据文件
           |--actions.json          // 应用支持的命令操作
           |--tp_views.json         // 屏幕上应用的视图定义文件
           |--mobile_views.json     // 手机路由宝应用中，本应用的视图定义文件
           |--web_views.json        // 浏览器中本应用的视图定义文件(暂不支持)
```

* `init` 必须接受以下命令。`commands.json` 文件中描述可接受的扩展命令。

```
    start       : 启动，实现启动本应用的操作
    stop        : 停止， 停止本应用的操作
    status      : 查询状态，查询本应用的状态
    set_config  : 对本应用重新进行配置，生效配置数据应该进行的操作
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

* `data.json` 描述本应用暴露出来可以进行配置修改的数据，样例如下：

```js
{
  "id"      : "data1",                        // 配置数据的ID
  "name"    : "username",                     // 配置数据的名字
  "value"   : "middle",                       // 配置数据的值
  "group_id": "group1",                       // 配置数据的组ID
  "type"    : {
                 "class" : "ENUM|BOOL|INT|STRING|FLOAT|SET",
                 "items" : ["high", "middle", "low"],  // ENUM，SET适用
                 "min"   : 0,                          // INT, FLOAT, STRING使用
                 "max"   : 100,                        // INT, FLOAT, STRING使用
                 "multiple" : true|false
              }
}
```

* `actions.json` 描述本应用支持或封装的命令，可供前端进行调用。样例如下：

```js
[
    {
      "id"      : "command1",                 // 应用的命令ID
      "is_sync" : true,                       // 是否需要同步阻塞执行,默认阻塞同步
      "input"   : "100",                      // 应用的命令参数
      "cmd"     : "./sbin/run_priv_cmd1",     // 应用的命令
      "relative": true                        // 应用的命令适用的是相对路径
    },
    {
      "id"      : "command2",                
      "is_sync" : true,                     
      "input"   : "",                      
      "cmd"     : "/bin/reboot", 
      "relative": false
    }
]
```

* `*_views.json` 描述在屏幕、路由宝、浏览器中本应用所展示的视图，样例如下：

```js
mobile_views.json 
{
  "id"      : "view1",                        // 配置视图的ID
  "name"    : "viewname",                     // 配置视图的名字
  "data"    : [
                {"id" : "data1",              // 视图需要展示的数据的ID
                 "acess" : "RO|RW"            // 数据是否可修改
                },
                ...
               ],
  "menu"    : [                               // 主视图适用
                  { "index"  : 1, 
                    "text"   : "contract", 
                    "type"   : "COMMAND|MENU|VIEW",
                    "action" : actionid,      // COMMAND适用
                    "input"  : data1,         // COMMAND适用
                    "viewid" : viewid,        // VIEW适用
                    "items"  : [              // MENU适用
                                 {
                                    "index"  : 1,
                                    "text"   : "item1",
                                    "type"   : "COMMAND|VIEW",
                                    "action" : actionid, // COMMAND适用
                                    "input"  : data1,    // COMMAND适用
                                    "viewid" : viewid    // VIEW适用
                                 },
                                 ...
                                ]
                   },
                   ...
               ]
}
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

## APP相关的扩展命令

### appInfo.sh 用于查询、设置APP相关的信息
语法 : appInfo.sh &lt; get_status &lt;package_id&gt; | set_status &lt;package_id&gt;  &lt;ISRUNNING | NOTRUNNING&gt; &gt;

| 参数 | 参数名                        |    意义                         |
| ---- | ----------------------------- | ------------------------------- |
|      | get_status, set_status        | 命令                            |
|      | &lt;package_id&gt;            | 应用程序的包ID                  |
|      | ISRUNNING, NOTRUNNING         | 正在运行, 不在运行              |

| 命令       | 退出码 | 输出                |               意义         |
| ---------- | ------ | ------------------- | -------------------------- |
| get_status | 0      | ISRUNNING           | 该应用正在运行             |
|            | 1      | NOTRUNNING          | 该应用不在运行             |
|            | 2      | NOTFOUND            | 应用不存在或未找到         |
|            | 3      | PARAMETER MISSING   | 缺少参数                   |
| set_status | 0      | OK                  | 设置成功                   |
|            | 1      | ERROR               | 设置失败                   |
|            | 2      | PARAMETER MISSING   | 缺少参数                   |
