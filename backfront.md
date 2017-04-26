# 回到前台逻辑处理
> APP 退出后台，回到前台，处理逻辑，仿照招商银行掌上生活，APP进入后台，过一段时间，回到APP，需要重新登录

## 大纲
1. 逻辑
2. 处理

### 1. 逻辑
* APP进入后台时，记录下当前的时间戳
* APP返回前台时，获取当前的时间戳。 
* 如果这俩时间戳差值> 300 (即进入后台时间大于5分钟)，进行下面操作
* 清除之前的登录信息，产生的密码信息
* 切换到首页，弹出登录界面

### 2. 处理
* APP进入后台时，记录下当前的时间戳

    ```
       func applicationWillResignActive(_ application: UIApplication) {
        let date = Date()
        let timeInterval = date.timeIntervalSince1970
        UserDefaults.standard.set(timeInterval, forKey:"miaoLeave" )
    }
    ```
* APP 回到前台

    ```
        func applicationWillEnterForeground(_ application: UIApplication) {
        let date = Date()
        let nowTimeInterval = date.timeIntervalSince1970
        
        let leaveTimeInterval = UserDefaults.standard.double(forKey: "miaoLeave")
        
        let cha =   nowTimeInterval - leaveTimeInterval
        if cha > 30 {
                logout(isAnimation: false)
        }
    }
    ```
* 退出登录，清除密码，切换到首页，弹出登录

    ```
    func logout(isAnimation:Bool) {
    let tabbarVC = UIApplication.shared.keyWindow?.rootViewController as! UITabBarController
    
    //当前就是登陆页
    if (tabbarVC.presentingViewController != nil) {
        return;
    }
    let homeVC = tabbarVC.viewControllers?[0] as?HomeController
    homeVC?.homeResetData()
    tabbarVC.selectedIndex = 0
    let loginSB = UIStoryboard(name:"Login",bundle:nil)
    let loginVC = loginSB.instantiateInitialViewController()
    tabbarVC.present(loginVC!, animated: isAnimation, completion: nil)
}
    ```
    
    ```
        func homeResetData() {
        user = nil
        pwdLabel.text = "密码显示区："
        copyBtn.setTitle("点击复制密码", for: UIControlState.normal)
    }
    ```


