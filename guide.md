#引导页
>也叫介绍页：APP 首次安装或者升级后，打开APP先显示出来的界面，一般会有三屏或者四屏。

## 大纲
1. 怎么设计引导页？
2. 什么时候显示引导页？
3. 引导页显示结束了，怎么处理？

### 怎么设计引导图？
密码喵采用的是：文字加截图的方式
底部标志当前页的点：当前页，黄色，其它灰色

### 什么时候显示引导图？
* 逻辑
    * 之前采用：首次安装，或者APP升级显示。有缺陷：如果APP 版本从 1.0 升级到 1.1，引导页没不改变，这个时候按逻辑引导页应该显示
    * 本次采用：当前引导图版本和已经显示的引导图版本不一致，显示。
    

```
let guideNowNumber =  "1.0"
let guideShowedNumberKey =  "guideShowedNumberKey"
    //获取已显示引导页版本
        let guideShowedNumber = UserDefaults.standard.value(forKey: guideShowedNumberKey)
        //如果已显示引导页版本找不到（初次安装）
        if guideShowedNumber == nil {
            //展示引导页
            showGuide()
        }else {
            //如果当前引导页版本版本 等于 已显示引导页版本
            if (guideNowNumber == guideShowedNumber as! String) {
                // 展示登录页
                showLogin();
            }else{
                //展示引导页
                showGuide()
            }
        }
```

```
func showGuide() {
    UIApplication.shared.keyWindow?.rootViewController = HHGuideController()
}
```

### 引导页显示结束了，怎么处理？
* 设置按钮：点击开始体验
* 向左滑动：根据偏移量，进入主程序（密码喵采用的是这种）

```
    // UIScrollView 代理方法，滑动时调用
    func scrollViewDidScroll(_ scrollView: UIScrollView) {
        // 密码喵有4张引导图， 滑动到最后一张是滑动了3个屏幕的宽度，继续滑动30的距离，出发
        if scrollView.contentOffset.x > 3 * screenW + 30 {
            scrollView.delegate = nil
            // 设置 已显示引导页版本 为 当前引导页版本
            UserDefaults.standard.setValue(guideNowNumber, forKey: guideShowedNumberKey)
            showLogin()
        }
    }
```

```
func showLogin()  {
    let miaoSB = UIStoryboard(name:"Miao",bundle:nil)
    let miaoVC = miaoSB.instantiateInitialViewController()
    UIApplication.shared.keyWindow?.rootViewController = miaoVC

    let loginSB = UIStoryboard(name:"Login",bundle:nil)
    let loginVC = loginSB.instantiateInitialViewController()
    miaoVC?.present(loginVC!, animated: false, completion: nil)
}
```


