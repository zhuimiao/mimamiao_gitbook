#AppIcon
> iPhone 屏幕显示的图标， 搜索图标等

## 大纲

1. 如何设计 AppIcon
2. 如何画 AppIcon
3. 如何使用插件一键导出所有iOS 所需的Icon
4. 如何使用生成好的 AppIcon

### 1. 如何设计AppIcon
 * 查看 iPhone 屏幕上的其他APP 的 Icon。
 * 发现个性图标不会弄，所以选择了，颜色+汉字方案
 
### 2. 如何画 AppIcon
 1. 下载并安装 sketch
 2. New Document (新建空白文档) 快捷键: command + n
 3. Insert Artboard (插入画板)，设置尺寸为 1024 * 1024
 4. 设置画板填充色（即设置 AppIcon 背景色），密码喵 Icon 的填充色为：F0FFF0
 5. Insert text （插入文字），并拖动到画板里，密码喵Icon 上的文字是：密。 并调整到合适的大小
 6. 选中画板，右下角选择1x 图片导出。结果得到1024 * 1024 分辨率的图片。
 
### 3. 如何使用插件一键导出所有iOS 所需的Icon
1. 插件下载地址：https://github.com/work4blue/sketch-app-asset-export
2. [下载 zip文件](https://codeload.github.com/work4blue/sketch-app-asset-export/zip/master) 并解压
3. 双击 AppAssetsExport.sketchplugin
4. 用 sketch 打开上面导出的图片，并选中
5. 顶部点击：Plugins -> App Assets Export -> Export App Icons
6. 稍等片刻，弹出的提示框，得到 App Icons

### 4. 如何使用生成好的 AppIcon
1. finder 前往文件夹：/Users/Shared/AppIcon/Assets.xcassets/
2. 将 AppIcon.appiconset  拖动到 Assets.xcassets 里面
3. Target -> General -> App Icons and Launch Images  -> App Icons Source

