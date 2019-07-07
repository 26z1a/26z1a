
#   使用nginx实现多平台直播教程  


本教程主要介绍如何在Windows使用nginx实现多平台直播,主要介绍步骤,并不太介绍原理

## 需要工具

- 文本编辑器 (推荐使用[nodepad++][np])

##  过程

####    nginx 部份

首先,下载 **[illuspas][作者]** 制作的*nginx* + *nginx rtmp module*整合包并解压.  [原地址][git_link]/[度盘][baidu_link]

然后, 进入*conf*文件夹, 用文本编辑器打开 *nginx.conf* , 你将会看到如下代码: 

```
worker_processes  1;

error_log  logs/error.log debug;

events {
    worker_connections  1024;
}

rtmp {
    server {
        listen 1935;

        application live {
            live on;
        }
		
        application hls {
            live on;
            hls on;  
            hls_path temp/hls;  
            hls_fragment 8s;  
        }
    }
}

...
```

我们将要改动rtmp里面的"application live"模块:

在
```
live on;
```
下面依次按照格式添加你要直播的平台(**注意最后的分号**):

```
push <直播平台url(rtmp地址)>/<直播key>;
```
如:
``` 
...

application live {
    live on;

    push rtmp://ps3.live.panda.tv/live_panda/484..cf1?sign=923..843&time=15..99&wm=2&wml=1&extra=0;
    push rtmp://live-cdg.twitch.tv/app/live_8.8_gm.A3; 
}

...
```
然后保存退出

点击主目录的 *nginx.exe* 运行nginx(可以通过*stop.bat* 停止运行nginx)

**注意: 永远不要把你的直播key泄露出去!!!**

各大平台的直播url和直播key都能在账号的仪表板/直播设置内找到

*注: twitch的直播url可以通过[这里][twitch_ingest]找到*

####    obs 部份

打开obs->设置->流, 流类型选择自定义媒体服务器,

url:

    rtmp://localhost:1935/live

*假如你有多台电脑 可以在另一台电脑上运行nginx,然后把url里的"localhost"换成运行nginx的电脑ip地址*

流名称:

    你可以随意填写

保存->开始推流

##  监控及带宽计算

[illuspas][作者] 制作的整合包里内置的监控工具, 有需要的可以在浏览器打开 [localhost:8080/stat](localhost:8080/stat) 

关于带宽计算:

        需要的带宽 ≈ (obs设置的码率 * 1.2) * 转推的平台数

到此, 本教程基本完结

最后, 可以关注下我的直播间 :

-   [~~熊猫tv~~](https://panda.tv/1847357) RIP panda.tv
-   [斗鱼tv](https://www.douyu.com/5866422)
-   [twitch](https://twitch.tv/26z1a)   


如果有任何关于直播和电脑/网络的问题,可以来我的[Discord][dis]或者加QQ: 1508727612(请备注)









[np]: <https://notepad-plus-plus.org/download/>
[作者]: <https://github.com/illuspas>
[git_link]: <https://github.com/illuspas/nginx-rtmp-win32>
[baidu_link]: <https://pan.baidu.com/s/1MP-ygJehLyaBZyEBbc3dXg>
[twitch_ingest]: <https://stream.twitch.tv/ingests/>
[dis]:  <https://discord.gg/ZkJFNVZ>
