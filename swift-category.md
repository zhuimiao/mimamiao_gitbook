# swift 类目
>swift 里没有类目。用 extension 关键字替代

## 大纲
1. 新建普通方法
2. 新建初始化方法 : convenience

###1. 新建普通方法

```
import Foundation

extension String {
func MD5() -> String? {
        let length = Int(CC_MD5_DIGEST_LENGTH)
        var digest = [UInt8](repeating: 0, count: length)
        if let d = self.data(using: String.Encoding.utf8) {
            d.withUnsafeBytes { (body: UnsafePointer<UInt8>) in
                CC_MD5(body, CC_LONG(d.count), &digest)
            }
        }
        return (0..<length).reduce("") {
            $0 + String(format: "%02x", digest[$1])
        }
    }
}
```

### 2. 新建初始化方法

```
import Foundation
import UIKit

extension UIColor {
    convenience init(r:Int ,g:Int ,b:Int, a:CGFloat) {
            self.init(red: CGFloat(r) / 255.0, green: CGFloat(g) / 255.0, blue: CGFloat(b) / 255.0, alpha: a)
        }
}



