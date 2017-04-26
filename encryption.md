#加密算法
>算法常常是APP的核心内容。密码喵界面很简单，那些都是次要的，主要内容是加密算法。

##大纲
1. 字符串倒序
2. 字符串与操作
3. md5 加密
4. 32位二进制串，分成4段，每段截取成30位串，每10位串算出一个密码字符


###1. 字符串倒序
原字符串："干扰串"+“用户名”+"密码"+"baidu"
```
// "干扰字符串usernamepasswordbaidu"
        let reverseString = String(self.characters.reversed())
        
//"udiabdrowssapemanresu串符字扰干"
```

###2. 字符串与操作
> 原理：用for 循环 每次在 原字符串 和  倒序字符串 各取一个字符，用这两个字符进行与操作得到结果(字符的与操作，本质上是字符对应的ASCII码进行与操作)

```
    func yihuo() -> String {
        //倒序字符串
        let reverseString = String(self.characters.reversed())
        
        //原字符串
        let s = self as NSString
        //倒序字符串
        let ss = reverseString as NSString
        //用来保存与操作结果字符串
        let sss = NSMutableString()
        
        for i in 0...s.length - 1 {
            //本质是取字符的ASCII码 与运算
            let str = s.character(at: i) ^ ss.character(at: i)
            sss.append("\(str)")
        }
        return sss as String
    }
    //得到结果："240712510823358315592004817110529181221211218295101172004831559233582510824071"
```

###3. md5 加密
>* 从可逆不可逆角度看，加密算法分两种，一种是可逆的，一种是不可逆的。可逆意味着可解密，可解密就有有被解密的风险，一旦被解密，用户的用户名和密码就会泄露 。不可逆的算法更加安全。md5 加密不可逆
>* 从固定长度与不固定长度角度看，加密算法分两种，一种是定长，一种是非定长。密码喵生成的密码都是12位的，需要采用定长算法。md5 加密长度可固定为32位


### swift 项目添加 md5 加密算法

1. 引入头文件
  
    ```
在 miao_password-Bridging-Header.h里
添加：
#import <CommonCrypto/CommonCrypto.h>
```
2. 方法具体实现

    ```
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
```

3. md5 加密结果
>"00f851596ba2fa3da677fa2a5b7d8a85"

###4. 32位二进制串，分成4段，每段截取成30位串，每10位串算出一个密码字符
1. md5 加密结果串 是 32 位长度的16进制字符串。将其分为四段
2. 取第一段："00f85159"，将字符串转变成long类型的证书，与30位二进制1进行与操作得到30位二进制串

    ```
        long lHexLong = strtol("00f85159", NULL, 16) & 0x3FFFFFFF;
        结果：十进制：16273753 二进制：111110000101000101011001
```

* 分别取出前10位，前20位，全30位与8位二进制1（127）进行与操作
    * 前10位：十进制：15 二进制：0000001111
    * 0000001111 & 11111111 = 0000001111 (15)
    * 判断上一步结果是否大于密码集最大索引，如果大与: 上一步结果 & 密码集最大索引
    * 取上面操作结果，查询出对应的密码："p"

    

```
char pwd[13] = {};

char *  jiami(const char* md)
{
    char c[] = {
        'a' , 'b' , 'c' , 'd' , 'e' , 'f' , 'g' , 'h' , 'i' , 'j' , 'k' , 'l' , 'm' ,
        'n' , 'o' , 'p' , 'q' , 'r' , 's' , 't' , 'u' , 'v' , 'w' , 'x' , 'y' , 'z' ,
        '0' , '1' , '2' , '3' , '4' , '5' , '6' , '7' , '8' , '9' , 'A' , 'B' , 'C' ,
        'D' , 'E' , 'F' , 'G' , 'H' , 'I' , 'J' , 'K' , 'L' , 'M' , 'N' , 'O' , 'P' ,
        'Q' , 'R' , 'S' , 'T' , 'U' , 'V' , 'W' , 'X' , 'Y' , 'Z' , '~' , '`' , '!' ,
        '@' , '#' , '$' , '%' , '^' , '&' , '*' , '(' , ')' , '_' , '+' , ':' , ';' ,
        ',' , '.' , '/' , '?' , '<' , '>' , '-'};
    
    int index = 0;
    //加密结果串赋初始值，防空处理
    for (int i = 0; i < 12; i++) {
        pwd[i] = md[i];
    }
    
    for (int i = 0; i < 4; i++) {
        
        char subMd[9] = {};
        for (int j = 0; j < 8; j++) {
            subMd[j] = md[i * 8 + j];
        }
        
        char *str = subMd;
        
        //结果为 30位二进制对应的 long 值
        long lHexLong = strtol(str, NULL, 16) & 0x3FFFFFFF;
        
//        printf("value %ld \n",lHexLong);

        for (int j = 0; j < 3; j++) {
        
            //取30位二进制的，前10位，前20位，全30位
            long hexValue = lHexLong >> 10 * (2-j);
//            printf("hexValue %ld \n",hexValue);

            //8个二进制1（127）与操作，得到结果不超过127
            long value = 0x0000007f & hexValue;
//            printf("value %ld \n",value);
            //如果大于密码集中最大索引
            if (value > 84) {
                value = value & 0x00000054;
            }
//            printf("value %ld \n",value);
            pwd[index] = c [value];
            index++;
        }
    }
//    printf("\n%s\n",pwd);
    
    return pwd;
}
 ```





