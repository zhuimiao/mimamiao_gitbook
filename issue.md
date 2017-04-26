#遇到的问题
>总结下遇到的坑，吸取教训，以后就不耽误事了

##大纲
1. swift String 转 c char *
2. 原先打的包手机能安装，突然间不能安装了
3. ipa 测试包 7.1m 发布包 62.5m
4. html 加载不出来

### 1. swift String 转 c char *
>把 swift String 传给 c 方法的 char * ，一直提示 unsafepoint，unsafemutablepoint 转换错误。废了一天时间也没解决好。

* 加密方法实现 c 语言更方便，所以用了c方法
* 怎么改怎么不对，改到我怀疑人生
* 原因是：用Xcode 新建 c 文件，没勾选同时创建.h 文件。 直接 import jiami.c

### 2. 原先打的包手机能安装，突然间不能安装了
* 首先我先看了看我的证书有没有过期
* 我又看了看我的 mobileprovision ，检查了下是不是失效了，设备是不是被移除了
* 我又看了看我的 app id，看看是不是被删除了
* 发现都没问题，我又清空了本地的mobileprovision，重新下载了一遍
* 打包的时候，我又换了 ad hoc 模式
* 折腾了好久，打了好几次包。最少用了两个小时，都没解决
* 重启 xcode ，重新打包OK了
* xcode 8.3 真坑

### 3. ipa 测试包 7.1m 发布包 62.5m
>swift-ObjC 混编 会使 ipa包 增大不少。所以现阶段 ObjC 开发是主流。

* 解压 发布包 ipa
* SwiftSupport 这个文件 72.3m

### 4. html 加载不出来
>折腾了将近一小时，怎么都不出来。快怀疑人生了

* webview 忘记添加到 self.view 上


