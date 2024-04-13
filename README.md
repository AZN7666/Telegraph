# 主要增加的功能
本分支在原版的基础上增加图片引用的限制，避免部署后任何知道服务地址的人均可使用，避免被上传“敏感”内容后的风险。

在cloudflare中部署非常简单，部署后只需要增加一个变量，将允许访问的域名以逗号间隔的方式写入即可。（后面有具体的操作步骤，删减了图片敏感检查服务的内容，补充了部分图示讲解）


[English](README-EN.md)|中文

# Telegraph-Image
免费图片托管解决方案，Flickr/imgur 替代品。使用 Cloudflare Pages 和 Telegraph。

## 特性
1.无限图片储存数量，你可以上传不限数量的图片

2.无需购买服务器，托管于 Cloudflare 的网络上，当使用量不超过 Cloudflare 的免费额度时，完全免费

3.无需购买域名，可以使用 Cloudflare Pages 提供的`*.pages.dev`的免费二级域名，同时也支持绑定自定义域名

4.支持图片审查 API，可根据需要开启，开启后不良图片将自动屏蔽，不再加载

5.支持后台图片管理，可以对上传的图片进行在线预览，添加白名单，黑名单等操作

#### 6.支持图片引用的域名判断，避免图片滥用。（2024年4月12日增加）

--------------------------------------
# 如何部署 
操作视频链接：
<a href="https://www.youtube.com/watch?v=vDQvdcmxkcw" title="部署视频介绍"><img src="https://i9.ytimg.com/vi_webp/vDQvdcmxkcw/mqdefault.webp?v=661a0d8d&sqp=CLSH6bAG&rs=AOn4CLCFfNTGbcfsLiGQI5CjpY8kWP06Zg" alt="Alternate Text" /></a>

## 提前准备

你唯一需要提前准备的就是一个 Cloudflare 账户。

## 手把手教程

简单 3 步，即可部署本项目，拥有自己的图床

1.Fork 本仓库 ：登录自己的GitHub账户，在浏览器中打开本项目，点击导航条中的Fork按钮，拉取到自己的GitHub下。

2.打开 Cloudflare Dashboard，进入 Pages 管理页面，选择创建项目，选择`连接到 Git 提供程序`
![1](https://telegraph-image.pages.dev/file/8d4ef9b7761a25821d9c2.png)

3. 按照页面提示输入项目名称，选择需要连接的 git 仓库，点击`部署站点`即可完成部署

4. 配置图片管理

1）支持图片管理功能，默认是关闭的，如需开启请部署完成后前往后台依次点击`设置`->`函数`->`KV 命名空间绑定`->`编辑绑定`->`变量名称`填写：`img_url` `KV 命名空间` 选择你提前创建好的 KV 储存空间，开启后访问 http(s)://你的域名/admin 即可打开后台管理页面

| 变量名称 | KV 命名空间 |
| ----------- | ----------- |
| img_url | 选择提前创建好的 KV 储存空间 |

![](https://im.gurl.eu.org/file/a0c212d5dfb61f3652d07.png)
![](https://im.gurl.eu.org/file/48b9316ed018b2cb67cf4.png)

2） 后台管理页面新增登录验证功能，默认也是关闭的，如需开启请部署完成后前往后台依次点击`设置`->`环境变量`->`为生产环境定义变量`->`编辑变量` 添加如下表格所示的变量即可开启登录验证

| 变量名称 | 值 |
| ----------- | ----------- |
|BASIC_USER = | <后台管理页面登录用户名称>|
|BASIC_PASS = | <后台管理页面登录用户密码>|

![](https://im.gurl.eu.org/file/dff376498ac87cdb78071.png)

3） 增加获取的图片的网站域名限制：

| 变量名称 | 值 |
| ----------- | ----------- |
|DOMAIN_LIST = | <以英文逗号分隔的域名清单>|

域名需要完整登录，如果你的网站xxx.com和sub.xxx.com都可以访问，则需要xxx.com和sub.xxx.com都以逗号分隔的方式维护到域名清单中。

4） 参数维护后重新发布项目

针对环境变量所做的更改将在下次部署时生效，如更改了`环境变量`，针对某项功能进行了开启或关闭，请记得重新部署。
![](https://im.gurl.eu.org/file/b514467a4b3be0567a76f.png)

5）绑定自定义域名（可选）

在 pages 的自定义域里面，绑定 cloudflare 中存在的域名，在 cloudflare 托管的域名，自动会修改 dns 记录
![2](https://telegraph-image.pages.dev/file/29546e3a7465a01281ee2.png)

## 一些限制：

1.由于图片文件实际存储于 Telegraph，Telegraph 限制上传的图片大小最大为 5MB

2.由于使用 Cloudflare 的网络，图片的加载速度在某些地区可能得不到保证

3.Cloudflare Function 免费版每日限制 100,000 个请求（即上传或是加载图片的总次数不能超过 100,000 次）如超过可能需要选择购买 Cloudflare Function 的付费套餐，如开启图片管理功能还会存在 KV 操作数量的限制，如超过需购买付费套餐


### 感谢

@cf-pages 提供Telegraph-Image功能，在我vps空间不足的时候帮我解决图片存储的问题。
