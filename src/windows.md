# 在X64 Windows上运行Windows RT

---

这几天实在闲的没事干，于是乎就在网上翻镜像找,结果找到了Windows RT 8.1的完整镜像，但是是个ESD，为了大家方便，我已经把ESD镜像改成了ISO，里面是wim，可以直接下载。

## 创建硬盘

这里引用BetaWorld百科安装Windows 10 ARM64的方法，依葫芦画瓢，安装Windows RT 8.1。

首先创建硬盘，管理员cmd输入diskpart，回车。
依次输入以下命令：

~~~bat

create vdisk file="C:\test\usb.vhd"（这里是镜像位置，什么地方都行） maximum=32000 type=expandable （镜像大小必须是2的n次方，这里指定32GB）
select vdisk file="C:\test\usb.vhd"
attach vdisk
convert gpt
create partition efi size=100（创建EFI分区）
format fs=fat32 quick
assign letter=p（指定卷标）
create partition primary（创建系统分区）
format quick
assign letter=w
exit
~~~

## 释放镜像

将ISO里的install.wim解压到某个位置，或者直接挂载ISO，假设盘符为D，以管理员身份运行cmd，执行以下命令：

~~~bat

dism /apply-image /imagefile:D:\install.wim /index:1 /applydir:W:
bcdboot W:\Windows /s P: /f uefi
~~~

完成后弹出P或W盘。注意，P盘符会有残留，不用担心，重启就会消失。

## 启动安装

普通的QEMU运行这个会出问题，所以这里要使用QEMU-winrt，这个项目是开源的，项目链接[在这](https://github.com/binarymaster/qemu/releases)

### 启动模拟器

进入QEMU-winrt安装目录，执行以下命令：

~~~bat

G:\qemuwinrt\qemu-system-arm -M virt,highmem=false ^
-cpu cortex-a15 -accel tcg,thread=multi -m 2G ^
-bios QEMU_EFI_1512_driver1998.fd -device VGA ^
-device usb-kbd,id=kbd -device usb-tablet,id=mice ^
-drive id=usbcdrom,if=none,media=cdrom,format=raw,file=winpe_arm32.iso.uibak,readonly=on ^
-device usb-storage,drive=usbcdrom -drive id=mydrive,if=none,file=C:\test\usb.vhd ^
-device sd-card,drive=mydrive -nic user,model=virtio
~~~

### 安装系统

开机后按F2，进入EFI设置，如果要修改分辨率，依次进入Device Manager、OVMF Platform Configuration，将Change Preferred项改为你想要的分辨率（例如1024x768），然后选Commit Changes and Exit，在QEMU菜单中选Machine下的Reset重启。

### 进入桌面

耐心等待系统安装完成，通过OOBE并进入Press Ctrl-Alt-Delete to Unlock界面；

或者强制退出QEMU，并挂载C:\test\usb.vhd；

在宿主机打开注册表编辑器，单击HKEY_LOCAL_MACHINE，在菜单中选择“文件(F)→加载配置单元(L)...”，打开W:\Windows\System32\config\SOFTWARE，任意命名项（例如，arm），展开该项并定位至\Microsoft\Windows NT\CurrentVersion\Winlogon（不展开），在右侧新建字符串值，名为AutoAdminLogon，值为1，新建字符串值，名为DefaultUserName，值为Administrator，新建字符串值，名为DefaultPassword，值为在OOBE中设置的密码，完成后回到本机HKEY_LOCAL_MACHINE，在菜单中选择“文件(F)→卸载配置单元(U)...”，在弹出的对话框中选择“是(Y)”；

### 安装完成

到这里系统就安装完成了。

## 下载链接

[ISO文件](https://starmoe-my.sharepoint.com/:u:/g/personal/starmoe_starmoe_onmicrosoft_com/EcWz9HyQAndEt-gZyumG2rgBDlI73b779t1-xh0mUljfpw?e=be5jzj)，大小3.24GB，SHA256: B7AC0BDBD409AC0A0DE0066D233CEBF957D70B5D8F88A4133BEF663A890E0174
（Host：OneDrive for Business - 亚太地区 - Starmoe-暮光工作室）  
[已经做好的vhd](https://starmoe-my.sharepoint.com/:u:/g/personal/starmoe_starmoe_onmicrosoft_com/EfVzz4Qze2lFoRXJmXgpkHIBivWIj-Tfcq7IcDAQbn7u1w?e=5RUy7O)，大小6.36GB，SHA256: 3453876D50097366C53EFADC699F2F3BD9C5142780C4F917E38B58843E8FCD72
(Host：OneDrive for Business - 亚太地区 - Starmoe-暮光工作室)
