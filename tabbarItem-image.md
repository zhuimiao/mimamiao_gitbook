# TabbarItem NavigationItem
> TabbarItem 底部标签选项卡
> NavigationItem 顶部导航条上的条目

## TabbarItem Image 尺寸
75 × 75  二倍图就是 75 × 75

## NavigationItem Image 尺寸
50 × 50  二倍图就是 50 × 50

## 设置导航栏的返回按钮

```
  extension UIViewController {
    func showBack() {
        let barItem = UIBarButtonItem.init(image: UIImage(named:"miao_back"), style: .plain, target: self, action: #selector(backAction))
        self.navigationItem.leftBarButtonItem = barItem
    } 
}  
```


