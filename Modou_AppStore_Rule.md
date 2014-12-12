# 概述
本文详细描述了应用商店的运作规则。
# 应用商店详细规则
## 应用在商店中的整体流程
1. App提交到BBS应用中心 http://bbs.modouwifi.cn/forum-47-1.html ，应用提交时指明是否上AppStore
2. 人工审核，如果通过，并且应用需要上AppStore，使用脚本推送到开发板AppStore
  - 人工审核（审核方式需完善）
  - 推送方式：登陆到i7服务器，执行脚本`/data/appstore/main/pushmain.sh xxx.mpk`推送App文件包到开发版AppStore
3. 展示应用。
  - 通过开发版，内部版路由器Web，TP的应用商店能浏览开发版应用商店里的应用
  - 在BBS应用页面 http://bbs.modouwifi.cn/appstore/ ，能浏览，下载应用商店的应用
4. 下载，安装开发版应用
  - 通过开发版，内部版路由器Web，TP的应用商店能下载开发版应用商店里的应用
  - 从BBS应用页面 http://bbs.modouwifi.cn/appstore/ 或者论坛应用中心版面下载App文件，然后在路由器Web端的应用栏里上传App文件进行安装
5. 应用从开发版毕业到稳定版
  - 人工审核（审核方式需完善）
  - 推送方式：登陆到i7服务器，执行脚本`/data/appstore/stable/pushstable.sh xxx.mpk`推送App文件包到稳定版AppStore
6. 下载，安装稳定版应用
  - 通过稳定版路由器Web，TP的应用商店能下载，安装稳定版应用商店里的应用
7. 升级，卸载应用
  - 通过路由器的Web，TP应用商店界面进行App升级，卸载操作

## App和开发者的绑定
- 开发者在论坛里提交应用时所使用的论坛ID作为开发者唯一识别号
- `manifest.json.author_forum_id`必须与App提交到论坛时所使用的论坛ID号相同
- 开发者必须保护自己的论坛账户，账户被盗，意味着别人可以以开发者的名义提交新应用或老应用的升级版

## App的唯一性
- 在开发版或稳定版AppStore里，`manifest.json.packege_id`具有全局唯一性
- 在开发版或稳定版AppStore里，`manifest.json.name`具有全局唯一性
- 在开发版或稳定版AppStore里，`manifest.json.author_forum_id`和`manifest.json.author_name`的对应关系有唯一性

## App提交时的规则
- 如果`manifest.json.name`在应用商店里已经存在，而且`author_forum_id`不同，拒绝提交
- 如果`manifest.json.name`在应用商店里已经存在，而且`version`低于等于现有的`version`，拒绝提交
- 如果`manifest.json.name`在应用商店里已经存在，而且`display_name`和其他应用的`display_name`相同，拒绝提交
- 自动生成`manifest.json.package_id = "com." + author_forum_id + "." + name`
- 自动生成`manifest.json.author = author_forum_id`对应的论坛用户名
