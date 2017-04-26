#xib实时显示子控件
>之前给 UIView 添加圆角都是通过运行才能看具体效果，这个方式效率太低了。

## 大纲
1. IBDesignable 实时显示效果
2. IBInspectable xib 直接修改自定义属性

### 1. IBDesignable
OC 使用宏：IB_DESIGNABLE

```
import UIKit

@IBDesignable
class HHTextField: UITextField {
    override func layoutSubviews() {
        super .layoutSubviews()
        self.layer.borderColor = UIColor.init(rgb: 0x9B9B9B).cgColor
        self.layer.borderWidth = 0.5
        self.layer.cornerRadius = 10
    }
}
```

### 2. IBInspectable xib 直接修改自定义属性

```
@IBInspectable var firstColor: UIColor = UIColor.blackColor() {
   // 值改变时更新UI
}
```


