#内购
>在 APP 内部使用的虚拟物品，需要走内购。

## 大纲
1. 协议、税务和银行业务
2. 添加内购项目
3. 添加沙箱测试员
4. 代码实现

### 1. 协议、税务和银行业务
* 进入itunes connect
* 选择协议、税务和银行业务
* 填写联系人信息
* 填写银行信息，银行的码可以联系银行在线客服获取。
* 填写税务信息。不要选择是美国人

###2. 添加内购项目
* 进入itunes connect
* 选择:我的 App 
* 选择：密码喵
* 点击功能
* 点击 App 内购买项目
* 点击 + 添加项目
* 审核信息：屏幕快照 和 审核备注（协商沙箱测试账号和密码）一定要填写，不然会被拒

### 3. 添加沙箱测试员
* 进入itunes connect
* 选择：用户和只能
* 选择沙箱技术测试员
* 点击 + 添加

### 4. 代码实现

```
   // 如果设备支持内购
            if checkPay() {
                //请求内购商品
                requestProducts()
            }
            
```

```
 func checkPay() -> Bool {
        if(SKPaymentQueue.canMakePayments()){
            return true
        }
        else{
            HHToast.shared.toast(title: "请先允许付费")
            return false
        }
    }
    
    func requestProducts() {
        let productID = NSSet.init(object: "mimamiao")
        let request = SKProductsRequest.init(productIdentifiers: productID as! Set<String>)
        request.delegate = self
        request.start()
        LoaderOverlay.shared.show()
    }
    
    func productsRequest(_ request: SKProductsRequest, didReceive response: SKProductsResponse) {
        request.delegate = nil
        LoaderOverlay.shared.hide()
        let products = response.products
        let product = products.first
        
        let pay = SKPayment(product: product!)
        
        SKPaymentQueue.default().add(pay)
        SKPaymentQueue.default().add(self)
        //Code here
    }
    

    func paymentQueue(_ queue: SKPaymentQueue, updatedTransactions transactions: [SKPaymentTransaction]) {
  
        let trans = transactions.first
        
        switch (trans?.transactionState)! {
        case SKPaymentTransactionState.purchased:
            showAlert(title: "赞助成功", subTitle: "感谢您的赞助")
            SKPaymentQueue.default().finishTransaction(trans!)
            SKPaymentQueue.default().remove(self)
        case SKPaymentTransactionState.failed:
            showAlert(title: "赞助失败", subTitle: "很抱歉，应用未能完成购买，烦请重新尝试")
            SKPaymentQueue.default().finishTransaction(trans!)
            SKPaymentQueue.default().remove(self)
        case SKPaymentTransactionState.restored:
            showAlert(title: "恢复购买成功", subTitle: "")
            SKPaymentQueue.default().finishTransaction(trans!)
            SKPaymentQueue.default().remove(self)
        default:
            print("日了购")
        }
    }
```

