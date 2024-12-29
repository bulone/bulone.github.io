# 小米4A百兆版刷机记录


&lt;!--more--&gt;

刷机时没有截图的习惯所以文中部分图片来源已标出。本次刷机小米路由器版本 2.18.58

---

## Telnet开启
### 启用Telnet客户端

基于Windows10，其他版本请自行查询。
![Telnet开启](pics/recording.gif &#34;Telnet开启&#34;)
1. win键&#43;s 输入控制面板并点击进入；
2. 选择 程序与功能；
3. 点击 启用或关闭Windows功能；
4. 选择 Telnet客户端 点击勾选；
5. 等待配置完成后重启生效。
### 开启路由Telnet
1. 在您的电脑上下载并安装[Python](https://www.python.org/downloads/)，**安装时请勾选 Add Python To Path**；

![python](pics/python_install.webp &#34;python&#34;)

2. 下载OpenWRTInvasion-0.0.8并进行解压；
3. 在文件管理器中打开刚才解压的文件，并在地址栏输入 cmd 回车进入Windows命令窗口；

![cmd](pics/cmd.webp &#34;cmd&#34;)

4. 在命令行中输入
```
pip install requests
python remote_command_execution_vulnerability.py
```

5.  输入路由器管理地址 192.168.31.1， 然后回车;
6. 输入路由器管理密码，然后回车；
7. 输入 1 ，然后回车等待结束。

![没钱了咋办@right.com.cn](pics/unlock_telnet.webp &#34;unlock_telnet&#34;)

## Breed刷入

上一节已经将路由器Telnet模式开启。

1. 命令行输入 ；
```
telnet 192.168.31.1

```
2. 用户名和密码均为root；
3. 连接成功后备份相关文件。
```
dd if=/dev/mtd3 of=/tmp/eeprom.bin
```
4. 下载[WinScp](https://pc.qq.com/detail/11/detail_2331.html)，协议选择 Ftp，用户名和密码均为 root ；
5. 登陆完成右侧部分找到 tmp 文件夹并将其中的 eeprom.bin 右键下载到电脑上；
6. 下载对应的 Breed并改名 breed.bin ,拖拽到 tmp 文件夹内；
7. 命令行中输入
```
mtd write breed.bin
```
8. 等待刷写完成。

## 固件刷入

本固件来自恩山论坛，仅有基础功能，没有其他的附加功能。

1. 将第三方固件下载到电脑上等待使用；
2. 首先用网线一端连接路由器的Lan口，另一端连接电脑。电脑不要连接网络；
3. 将路由器电源断开，按住 reset 键插入电源，持续时间5秒以上松开即可；
4. 电脑显示连接以太网，打开路由器并在地址栏输入 [192.168.1.1](192.168.1.1)；
5. **点击固件更新，在 EEPROM 一栏选择刚才备份的 eeprom.bin ,然后点击上传**；
6. 点击固件备份，选择编辑器固件然后文件现在下来备份好；
7. 点击固件更新，在固件一栏选择下载的第三方固件，点击上传，等待更新完成即可；
8. 刷好估计固件后，在固件的管理后台进行双清操作，避免部分功能无法使用。

## 附件下载

- OpenWRTInvasion-0.0.8: [第6](https://www.right.com.cn/FORUM/thread-6074185-1-1.html)
- breed: [密码2333](https://pan.baidu.com/s/1MqmhIQNmv2VMhHleIDp-QQ?pwd=2333)
- 第三方Pavadan: [小米4A百兆版padavan精简固件r4ac](https://www.right.com.cn/forum/forum.php?mod=viewthread&amp;tid=8225989&amp;highlight=r4ac)
- 原固件完整备份:  [Bulone Share](https://web.hifo.fun/Application/share/r4ac100m%E5%A4%87%E4%BB%BD.zip)
## 参考
- [【教程】傻瓜版无需拆机 u盘 ssh小米3G 3a 3c 4c 4a百兆千兆版本等开启telnet刷breed-小米无线路由器及小米网络设备-恩山无线论坛](https://www.right.com.cn/forum/thread-4017759-1-1.html)
- [小米路由器4A千兆版(R4A)刷openwrt官方固件(附Telnet连接失败解决方案)-小米无线路由器及小米网络设备](https://www.right.com.cn/FORUM/thread-6074185-1-1.html)
- [WSL\_openwrtinvasion\_0.0.10\_哔哩哔哩\_bilibili](https://www.bilibili.com/video/BV1wq4y1c7Sc?p=1)
- [恩山论坛](https://www.right.com.cn)

---

> 作者: bulone  
> URL: http://blog.hifo.fun/posts/c5ef51d/  

