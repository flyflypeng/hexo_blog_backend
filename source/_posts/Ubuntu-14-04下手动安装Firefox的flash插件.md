title: "Ubuntu 14.04下手动安装Firefox的flash插件"
date: 2015-07-15 08:43:29
categories: 技术
tags:
- Firefox
- flash
- Ubuntu 14.04
---
## 前言

以前自己一直使用的是Google的Chrome浏览器，主要是因为Chrome浏览器很简洁并且响应速度快。但是后来当自己想在不同的系统平台下的Chrome浏览器同步自己保存的书签时就遇到了一个很让人蛋疼的问题，Chrome只支持Google账户同步，而在俺们大天朝，Google是黑户，如果不通过某种方法是根本无法享受到Google的一系列服务，包括Chrome的书签同步功能。山不转水转，我就重新寻找一个可以在多平台进行书签同步的浏览器了，这不，Firefox就出现在我的世界中了。

用了一段时间的Firefox感觉还不错，功能也很全面，还有很多的插件扩展浏览器的功能，关键它也支持不同平台的书签同步功能。但是，但是，但是，直到有一天当我打开Firefox想看视频时，它却给我弹出这个东东。
![flash_problem][flash_problem_url]
好不，不懂只能文度娘了。在度娘上找到了“病因”，原来是Firefox上面的flash插件版本太低不安全了，尼玛，老子在其他浏览器上都没遇到过这种问题啊，没办法，这不得解决嘛。后来的后来，我几乎每个月都能碰见这种情况一会，以前找到的解决方案也忘了，所以今天就把这法子给记下来，以备后用。

在Ubuntu下这种插件也没有apt包可以直接安装，只能自己手动下载安装，下面就详细说明下整个安装过程。

<!--more-->

### 1. 下载flash插件

flash 插件的官方下载地址为: [https://get.adobe.com/flashplayer/?loc=cn][flash_download_url]

然后选择.tar.gz压缩文件格式下载，如下所示:
![flash_download_pic][flash_download_pic_url]

### 2. 解压文件

将下载好的flash插件压缩包，解压到一个文件夹中，如下所示:
![flash_decompress][flash_decompress_url]

### 3. 将解压出来的libflashplayer.so拷贝到/usr/lib/mozilla/plugins/下
```
$ sudo cp libflashplayer.so /usr/lib/mozilla/plugins/
```

### 4. 将解压出来的/usr目录下的所有文件拷贝到/usr/*下
```
$ sudo cp -r usr/* /usr/
```

### 5. 最后重新启动下firefox就可以了


[flash_problem_url]:http://7xj51c.com1.z0.glb.clouddn.com/flash_problem.png
[flash_download_url]:https://get.adobe.com/flashplayer/?loc=cn
[flash_download_pic_url]:http://7xj51c.com1.z0.glb.clouddn.com/flash_download.png
[flash_decompress_url]:http://7xj51c.com1.z0.glb.clouddn.com/解压flash插件.png

