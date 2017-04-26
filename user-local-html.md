#使用本地 html
>关于、如何使用?、建议反馈 这三个页面都是用 html 实现的。因为比较方便。

## 大纲
1. 黄色文件夹
2. 蓝色文件夹

### 1. 黄色文件夹
```
let fileUrl = URL.init(string: Bundle.main.path(forResource: "miao_help.html", ofType: "")!)
```

### 2. 蓝色文件夹
```
   let fileUrl = URL(string:Bundle.main.path(forResource: "miao_help.html", ofType: nil ,inDirectory:"htmlFiles")!)
```
## 相关代码
```
 webController.htmlUrl = fileUrl
 webController.navItemTitle = "如何使用？"
 self.navigationController?.pushViewController(webController, animated: true)
```

HHWebController.swift

```
import UIKit

class HHWebController: UIViewController {

    var navItemTitle:String! = nil
    var htmlUrl:URL! = nil
    override func viewDidLoad() {
        super.viewDidLoad()
        self .showBack()
        self.navigationItem.title = self.navItemTitle
        self.view.backgroundColor = UIColor.init(rgb: 0xFAFFF0)
        let webView = UIWebView(frame:self.view.frame)
        webView.backgroundColor = UIColor.init(rgb: 0xFAFFF0)

        self.view.addSubview(webView)
        
        let request  = URLRequest.init(url: htmlUrl)
        webView .loadRequest(request)
    }
}
```










