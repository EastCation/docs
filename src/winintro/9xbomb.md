## Windows 9X如何拆弹？


如你们所知，自从Windows Chicago某个版本开始，Win9x beta也有了时间炸弹，这里让EC讲解一下如何拆解Windows 9x/me时间炸弹

原料：正式版和对应的测试版Windows系统各一个、一个可以启机的软盘镜像(要有DOS文件资源管理器，直接copy会告诉你找不到文件) 

操作步骤

1.启动正式版的Windows系统，插入软盘，并在软盘下新建一个文件夹，我这以Windows 98为例

2.复制C盘根目录下的io.sys, 到你软盘里的文件夹

>注意: 千万不要复制到软盘根目录，这可能会使你的软盘无法启动

3.关闭虚拟机，进入测试版虚拟机并载入软盘

打开DOS文件管理器，找到你复制的io.sys并复制到C盘根目录，提示覆盖确定即可

再次重启进入beta版系统，可以看到时间炸弹被拆了

提示: Windows Millennium替换后会将开机画面替换掉，属于正常现象
