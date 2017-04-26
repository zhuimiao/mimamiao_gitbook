# swift、 c、 objc 混编
>swift 里面如何调用 objc 方法， 如何调用自己写的 c 方法

## 大纲
1. swift、 objc 混编
2. swift、 c    混编

### 1. swift、 objc 混编
> swift 文件里不能 import oc 类库

解决方法：

```
1. 新建 miao_password-Bridging-Header.h
2. #import <CommonCrypto/CommonCrypto.h>
```

### 1. swift、 c 混编
> swift 文件里不能 import c 文件

解决方法：

```
1. 新建 miao_password-Bridging-Header.h
2. #import "jiami.h"
```
有个大坑：
>
Xcode 新建 c 文件的时候，一定要勾选上 同时创建 .h文件。 我因为没有勾选上，只创建了一个.c 文件，c方法调用起来，一直提示有错误，害我改了一天，后来添加上.h 文件，直接就没错误了。

