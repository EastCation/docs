# 远程使用Linux服务器桌面

相信大家都有一个疑问：如何在浏览器里进行远程桌面访问，并且还要将对端的音频传输进来，否则只有视频，没有音频会令人很难受。

那么本节内容就针对此问题提供了一个解决方案。

## Step 1.部署桌面和novnc

> 如果您在此之前已完成了此步骤，那么直接看第二步吧！

### 使用tmoe脚本（推荐）

安装tmoe脚本的命令如下：

```bash
echo "$(curl -L l.tmoe.me)" >> tmoe.sh
bash tmoe.sh
```

>在安装过程中可能会用到管理员/root权限，请准备好管理员/root的密码。

之后此脚本会引导您配置语言和设置，并且有全中文的安装过程，如果您还是不懂，请跟紧以下步骤：

1.进入选择Manager还是Tools界面时，选择Tools选项，之后选择“ 1 🍭 GUI:图形界面(桌面,WM,登录管理器)” > “1 🍰 rootless_DE(无root容器可运行:xfce,kde,mate,lxqt)” > “1 🐭 xfce(兼容性高,简单优雅)”

>注意：在选择桌面时，可以根据自己喜好和服务端压力酌情选择，一般来讲xfce是最好的选择。

2.停顿到“按回车键继续，输n返回”时，请按回车键继续。

3.您可根据喜好和服务器负载酌情选择是否安装Electron工具箱和Chromium浏览器。

4.停顿到“您是否需要配置X11VNC服务？”时，请按回车键继续。

5.*重要* 停顿到“您是否需要配置novnc？”时，请按回车键继续。

6.设置好vnc密码，退出tmoe脚本。

7.在命令行下键入“novnc”命令后，应该会出现以下文本

```text
Using installed websockify at /usr/bin/websockify
Starting webserver and WebSockets proxy on port 36080


Navigate to this URL:

    http://Starmoe-NuanR:36080/vnc.html?host=Starmoe-NuanR&port=36080

Press Ctrl-C to exit


WebSocket server settings:
  - Listen on :36080
  - Web server. Web root: /usr/share/novnc
  - No SSL/TLS support (no cert file)
  - proxying from :36080 to localhost:5902
```

>在此实例中，“Starmoe-NuanR”为主机名

之后在浏览器键入http://<你的Linux主机的局域网地址>:36080/vnc.html，您应该会看到novnc的连接界面，单击“连接”，输入vnc密码后即可进入桌面。

### 手动安装

>真没想到这段教程被辉夜给咕了，愿有志之士前来补充。

## Step 2.配置音频服务器

我相信大多数阅读此文档的用户使用的是Windows 操作系统，接下来介绍在Windows 端设置PulseAudio服务器以及将Linux音频远程连接到此。

>本段文章参考了[此博客](https://ciallo.work/archives/756)

由于PulseAudio官方服务端不维护了，有一个版本更新的第三方维护的，版本比较新的客户端：PulseAudio-win32可以使用。

您可以[点此](https://ciallo.work/wp-content/uploads/2024/03/1711208370-pulseaudio-win32-installer-v5.zip)进行下载（如果前者链接下载缓慢或者无法访问，请使用此[镜像](https://ciallo.work/wp-content/uploads/2024/03/1711208370-pulseaudio-win32-installer-v5.zip)）

下载完成后打开安装包安装，在配置界面确保你选中了如下图所示的选项。

![PulseAudio Server的配置页面](https://ciallo.work/wp-content/uploads/2024/03/1711209767-image.png)

之后此服务将会随计算机自动启动，请您放心，它是不吃资源的。

## Step 3.将Linux音频连接到此电脑

您可以使用如下命令将Linux的音频连接到此电脑：

```bash
export PULSE_SERVER=tcp:<你电脑的地址>:4713
```

如果您觉得每次都要这么载入一遍，嫌麻烦的话，你也可以将此命令写入你的shell变量里（例如.bashrc）

## Step 4.大功告成

到这里教程基本就结束了，您仅需要重新载入您的novnc即可。接下来就在您的web端桌面好好享受多媒体带来的乐趣吧！
