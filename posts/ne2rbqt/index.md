# 解决Github访问等问题



&lt;!--more--&gt;

&gt; [!IMPORTANT]
&gt; 《国际联网暂行规定》第六条规定：“计算机信息网络直接进行国际联网，必须使用邮电部国家公用电信网提供的国际出入口信道。任何单位和个人不得自行建立或者使用其他信道进行国际联网。”
由于网络原因，访问 Github 网页一直时好时坏。要么无法访问，要么加载速度特别漫长。使用 git 命令进行 pull、push 操作时经常会遇见超时现象，特别折磨。于是乎在网络上寻求解决方案，以下是两种正规的解决方法。
##  Watt Toolkit
原本叫 Steam&#43;&#43;，也不清楚为什么改名。挺之前的名字就知道这大概率是一款游戏加速器。不过也挺 &#34; 不务正业 &#34;，对于一些相关特定网站也有一定的加速效果。启用软件后可以检测是否连通及网站延迟。
![官网介绍](/images/优化Github访问-20250220221642061.webp)

软件可以通过 [官网](https://steampp.net/) 或者 [Microsoft Store](https://apps.microsoft.com/detail/9mtcfhs560ng?hl=en-US&amp;gl=US) 进行下载，整体界面延续 Windows 风格很美观。

\*Watt Toolkit 可在所有支持的 Windows 、macOS 、iOS 和 Android 版本上使用，同步令牌、账号信息。
## FastGithub

&gt; [!IMPORTANT]
&gt; - **fastgithub 不具备特殊功能,也没有相关的计划**
&gt; - **fastgithub 不支持 Windows7 等已被发行方停止支持的操作系统，并且也不会主动提供支持**
&gt; -  **fastgithub 不能为您的游戏加速**
&gt; - **fastgithub 没有主动在 github 之外的任何渠道发布**

FastGithub 本地代理使用的都是“公用电信网提供的国际出入口信道”，从国外 Github 服务器到国内用户电脑上 FastGithub 程序的流量，使用的是正常流量通道，其间未对流量进行任何额外加密（仅有网页原有的 TLS 加密，区别于 VPN 的流量加密），而 FastGithub 获取到网页数据之后发生的整个代理过程完全在国内，不再适用国际互联网相关之规定。

整体很简单小巧的软件，下载后运行 FastGithub.UI.exe 即可进行 Github“加速”效果，这个加速要加双引号，毕竟请求 Github 在当前网络下能够访问的 IP 地址。

Windows 设置开机自启在程序文件夹运行 `.\fastgithub.exe start` 该命令即可，关闭则为 `.\fastgithub.exe stop`

### 遇到问题
 git

git 操作提示 `SSL certificate problem`  
让 Git 使用系统的证书库：`git config --global http.sslbackend schannel`

firefox

firefox 提示 `连接有潜在的安全问题`  
设置 -&gt;隐私与安全 -&gt;证书 -&gt;查看证书 -&gt;证书颁发机构，导入 cacert/fastgithub.cer，勾选“信任由此证书颁发机构来标识网站”

&gt; [!TIP]
&gt; FastGithub 为每台不同的主机生成自颁发 CA 证书，保存在 cacert 文件夹下。客户端设备需要安装和无条件信任自颁发的 CA 证书，请不要将证书私钥泄露给他人，以免造成损失。

下载链接：
- [清华云盘](https://cloud.tsinghua.edu.cn/d/df482a15afb64dfeaff8/?p=/&amp;mode=list)
- [Github](https://github.com/WangGithubUser/FastGitHub)

---

> 作者: 蓝莓吐司  
> URL: http://localhost:1313/posts/ne2rbqt/  

