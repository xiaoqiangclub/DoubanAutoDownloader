
# 📖 豆瓣追剧助手（DoubanAutoDownloader） 📖

> 宝子们，追剧是不是总让你头疼？会员开了一个又一个，资源还是难找，更新也总错过。别愁！今天挖到个宝藏追剧工具，能自动追豆瓣 “想看”
> 剧，搜高清资源，下到本地或存云盘，微信通知更新。它部署超灵活，Windows、Docker 行，群晖 NAS、软路由等也没问题，这就给大家好好讲讲！

**💥 功能亮点 💥**

- 🍿 **豆瓣追剧**：自动追踪豆瓣添加的`想看`影视资源，确保每一集更新都不会错过。
- 🔍 **自动搜索**：`自动搜索`最新的高清资源，确保画质清晰，观影体验一流。
- ☁️ **添加云盘**：支持将下载内容自动保存到`云盘`，轻松管理，无需占用本地硬盘空间。
- 💾 **本地下载**：除了云盘，工具还支持将资源`下载到本地`，灵活选择保存路径。
- 📱 **微信通知**：每当新的剧集更新或下载完成时，及时通过`微信通知`你，保持随时掌握。
- ⚡ **快速部署**：支持`Windows直接运行和Docker快速部署`，方便快捷！
- 🌐 **自定义链接**：可以手动设定`追剧链接`（计划）。
- 🎈 **其他**：更多实用功能，`持续更新`，让你的追剧体验更加便捷和高效！

![豆瓣追剧助手](https://i-blog.csdnimg.cn/direct/1759bb9791a1469390277daab2e7fcee.png)

# 🏡 演示环境 🏡

本文演示环境如下：

- **部署环境**：Windows 11 / Docker部署
- **Python版本**：3.11.5

**注意：本文内容为个人笔记，仅供参考。[附：读者须知](https://xiaoqiangclub.blog.csdn.net/article/details/138905099)**

# 📒 豆瓣自动追剧助手 📒

> `豆瓣自动追剧助手（DoubanAutoDownloader）` 将豆瓣 + 三方资源网站 + 迅雷融合在一起，实现自动追剧功能

工具是闲暇时候写的，有很多设想还没有实现，后续会一一补充。当前版本是`0.0.1-alpha`，可能存在较多BUG，大家可以在文章下面留言，或直接邮件
`xiaoqiangclub@hotmail.com`进行反馈。

*

*

如果软件给您带来了帮助，感谢 [💋点击打赏赞助支持💋](https://gitee.com/xiaoqiangclub/xiaoqiangapps/raw/master/images/xiaoqiangclub_ad.png)
**

## 📝 使用方法

首先下载最新版本软件，下载后会有2个文件：

- `豆瓣自动下载器 vx.x.x.exe`：Windows下使用，双击打开即可
- `douban_auto_downloader vx.x.x`：Docker部署使用

![文件](https://i-blog.csdnimg.cn/direct/c5ddcecccf3044a2baaeb552a8acf94f.png)

### 🔖 Windows

直接双击运行`.exe`文件，如果出现网络请求，请`允许`。程序会自动访问`http://127.0.0.1:8000/`，你可以在这个页面进行相关的配置，详情请移步至`配置说明`。

![启动](https://i-blog.csdnimg.cn/direct/6af29a4bdfd04ed1a5cf02bb8a7b7867.png)

**注意：** 配置完成后`http://127.0.0.1:8000/`页面可以关闭，但是exe程序需要保持开启。如果要修改配置，可以手动访问`http://127.0.0.1:8000/`进行设置。

### 🔖 服务器部署

这里推荐使用`docker-compose.yml`进行快速部署，docker-compose.yml如下：

```yaml
version: '3.8'

services:
  douban_auto_downloader:
    image: xiaoqiangclub/run_app:latest  # 镜像
    container_name: douban_auto_downloader  # 容器名称
    working_dir: /app  # 容器内工作目录

    # 将本地目录挂载到容器的 /app 目录
    # 注意：这里的 /vol1/1000/docker/douban_auto_downloader 是宿主机的路径，
    #      请替换为你自己的本地路径，并且将下载的 'douban_auto_downloader vx.x.x' 文件放到这个目录。
    volumes:
      - /vol1/1000/docker/douban_auto_downloader:/app  # 本地目录挂载到容器的 /app 目录

    ports:
      - "8888:8000"  # 映射宿主机的 8000 端口到容器的 8888 端口，8888可以自行修改
      # 这样你可以通过访问宿主机的 8888 端口来访问容器中的服务

    environment:
      - TZ=Asia/Shanghai  # 设置时区为上海
      - APP_FILE=douban_auto_downloader*  # 设置可执行文件前缀为 run_app*

    restart: always  # 如果容器停止，自动重启
```

参考 [教您如何快速在群晖NAS、飞牛NAS等Nas、软路由和服务器上部署迅雷，轻松实现远程添加下载任务和远程管理！](https://xiaoqiangclub.blog.csdn.net/article/details/144750284)

部署完成以后可以通过访问 `宿主机IP:映射端口` 来进行配置。

### 🧰 配置项说明

进入设置页面，如图
![设置界面](https://i-blog.csdnimg.cn/direct/e34285fd2f10420d818e7053258ae643.png)

直接按照上面的提示填写就可以了。

- 如果您需要下载到本地，需要预先安装一个`迅雷远程设备`
  ，可以参考  [教您如何快速在群晖NAS、飞牛NAS等Nas、软路由和服务器上部署迅雷，轻松实现远程添加下载任务和远程管理！](https://xiaoqiangclub.blog.csdn.net/article/details/144750284)
  进行部署。

- `远程设备的名称` 可能不会填写，这里简单的说明一下：
  你在PC端登录迅雷，然后进入`更多——设备——点击设备名称`，进入设备管理，就可以查看 `远程设备的名称`，需要`完整的设备名称，并且区分大小写`
  ，不要弄错了，如图

![获取设备名称](https://i-blog.csdnimg.cn/direct/afa9fcd649404c20aa63e1844b29a8e2.png)

- 当然也可以直接使用电脑上的迅雷软件进行下载，首先下载并安装最新版本的迅雷并启动，然后`登录账号`，一般情况下电脑端的设备名称是：
  `电脑-用户名`，例如，我的就是`电脑-XiaoqiangClub`，当然防止错误，可以在手机端的迅雷APP中查看电脑设备名称：
  `我的——远程设备——点击设备名称`查看，如图
  ![电脑设备名称](https://i-blog.csdnimg.cn/direct/d5a9310ce42f48c4997e6d826e0674e7.png)

- `远程设备下载保存目录` 只需要在远程设备你手动添加一个任务就可以看到目录路径了，如图
  ![保存路径](https://i-blog.csdnimg.cn/direct/a6d2c00bdbea41ffa2aa8f29d6bd41b2.png)
- 微信通知的话，参考 [如何获取企业微信API接口参数](https://xiaoqiangclub.blog.csdn.net/article/details/144614019)
  ，设置完成后点击保存，如果设置正确的话您的微信会接受到类似下列的推送消息，如图
  ![消息](https://i-blog.csdnimg.cn/direct/8b08c3b5d76c4574ac7e11d1176322c1.png)

**注意**：记得最后保存

# 🚨 注意事项🚨

1. 请确保网络环境正常
2. 设置后记得点击`保存设置`
3. 确保`云盘/本地` 存储容量充足

# 🎈 获取方式 🎈

> 解压密码：xiaoqiangclub

- [下载地址：https://pan.xunlei.com/s/VOFK1rN5xBvyuR1DeqE0rtZQA1?pwd=g944](https://pan.xunlei.com/s/VOFK1rN5xBvyuR1DeqE0rtZQA1?pwd=g944#)

## 🎉 版本更新 🎉

📑 0.0.2-beta

- 修复hao6v已知错误
- 新增添加`hao6v自定义`页面
- 日志级别修改为INFO
- 新增`自动获取`远程设备名称
- 新增`立即搜索`功能
- 新增`赞助`链接

📑 0.0.3-beta

- 完善自定义链接数据获取
- 修复下载空数据的错误问题
- 修复已知错误
- 新增自动检测豆瓣文件
- 保存已完成任务数据，防止重复下载
- 将自定义动漫/动画集数设置为99999
- 跳过下载过的远程设备任务

# ⚓️ 相关链接 ⚓️

- [我的主页](https://xiaoqiangclub.us.kg/)
- [如何用Python优化剧集自动下载工具](https://xiaoqiangclub.blog.csdn.net/article/details/138905099)
- [豆瓣API 使用教程](https://xiaoqiangclub.blog.csdn.net/article/details/137290877)
- [教您如何快速在群晖NAS、飞牛NAS等Nas、软路由和服务器上部署迅雷，轻松实现远程添加下载任务和远程管理！](https://xiaoqiangclub.blog.csdn.net/article/details/144750284) 
