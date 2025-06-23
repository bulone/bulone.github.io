# Bt-R320安装青龙面板


<!--more-->

前几天在闲鱼上淘到一台搭载 Mt7981 型号为 Bt-R320 的 Wifi6 路由器，配置为 1G+128G EMMC+USB3.0 四个千兆网口，大概率是跑 PCDN 淘汰下来的。不过才几十块就能拿到能刷 Immortalwrt 系统的千兆路由还是很香的，并且这配置跑的青龙还是轻轻松松的。

## 刷机
{{<bilibili BV1zXzEYoEHs >}}  
具体教程在这个视频中都有，所需文件请到该播主公众号自行下载。

## 配置
刷好系统后，默认访问地址为 192.168.1.1，访问密码为 admin。

在系统 - 挂载点将/mnt/mmcblk0p6 挂载上，然后在 Docker- 配置中将 Docker 根目录改成/mnt/mmcblk0p6，注册表镜像新增 https://docker-0.unsee.tech 点后面➕号。  
![](./images/index-1750659524211.webp)

在终端中拉取青龙面板。

![拉取青龙仓库](./images/index-1750651561906.webp "拉取青龙仓库")  
9730 是外部访问接口，可自行修改。对于官网的拉取命令删除了 -e 的两行环境设置，不删我运行访问网站是 Nginx 默认网页，不清楚为何这样，凑活着使用吧。

```bash title="拉取命令"
docker run -dit \
  -v $PWD/ql/data:/ql/data \
  # 冒号后面的 5700 为默认端口，如果设置了 QlPort, 需要跟 QlPort 保持一致
  -p 5700:5700 \
  --name qinglong \
  --hostname qinglong \
  --restart unless-stopped \
  whyour/qinglong:latest
```


## 网络
容器使用 bridge 模式访问网络，不清楚为什么无法访问外网，根据搜索来的解决方案，修改后是可以访问的。

1、网络 - 防火墙 - 转发：接受  
2、添加如下防火墙规则  
```bash
iptables -t nat -A POSTROUTING -s 172.31.0.0/16 ! -o docker0 -j MASQUERADE
```
   （172.31.0.0 改成自己 docker0 的网段）  
3、删除 docker0 并重启  
```bash
ip link del docker0  
reboot
```
重启后重启下防火墙，位置概览 - 防火墙 - 右上角重启防火墙。

目前只能在内网中访问青龙主页，后续有机会在进行内网穿透。最后，还是感慨下 7981 还是有点热的，期待 7986 白菜价。  
😘如果有什么问题希望慷慨指出。  
![系统概览](./images/index-1750659931740.webp "系统概览")
## 参考
1. [刷了flippy大神的openwrt固件，容器无法联网-斐讯无线路由器以及其它斐迅网络设备-恩山无线论坛](https://www.right.com.cn/FORUM/thread-4069232-1-1.html)、
2. [132元的路由器新机皇BT-R320 刷机教程\_哔哩哔哩\_bilibili](https://www.bilibili.com/video/BV1zXzEYoEHs/?spm_id_from=333.337.search-card.all.click)
3. [GitHub - whyour/qinglong: 支持 Python3、JavaScript、Shell、Typescript 的定时任务管理平台（Timed task management platform supporting Python3, JavaScript, Shell, Typescript）](https://github.com/whyour/qinglong)

---

> 作者: 吐司气泡  
> URL: https://blog.toastbubble.top/posts/mg7gc5n/  

