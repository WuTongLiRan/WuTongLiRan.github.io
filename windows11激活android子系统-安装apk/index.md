# 

# Windows11激活Android子系统 & 安装apk

## Windows11激活Android子系统

- 打开Microsoft store(微软商店)，搜索Amazon Appstore(亚马逊应用商店)并安装
  ![](https://pic.imgdb.cn/item/64acc1191ddac507cc42093d.jpg)

  > 安装Amazon Appstore后系统会自动帮你安装Android子系统，安装Amazon Appstore的目的仅仅如此，安装完Amazon Appstore后无需管它
  
- 按照系统提示安装激活Android子系统

- 通过搜索找到应用“**适用于Android的Windows 子系统**”并打开
  ![](https://pic.imgdb.cn/item/64acc3021ddac507cc488ab3.jpg)

- 在**“高级设置”**中勾选**“开发人员模式”**，并点击**“管理开发人员设置”**
  ![](https://pic.imgdb.cn/item/64acc4491ddac507cc4d19ad.jpg)

  > 点击**“管理开发人员设置”**的目的仅仅是为了启动Android子系统(为后续用adb连接Android子系统传输安装包做准备)，无其他作用

   

## 向Android子系统传输安装包(apk)

- 下载adb Platform-Tools
  [下载链接](https://developer.android.com/studio/releases/platform-tools?hl=zh-cn)(可能需要挂梯)
  ![](https://pic.imgdb.cn/item/64acc6051ddac507cc537792.jpg)

  > adb Platform-Tools用于与Android子系统连接并向其传输安装包

- 下载完后把解压后的文件夹移到固定的地方(推荐D盘)

- 移动到固定的地方后，复制adb.exe所在的路径
  ![](https://pic.imgdb.cn/item/64acc7d41ddac507cc5bf80f.jpg)

- 配置系统变量

  - 搜索**系统环境变量**
    ![](https://pic.imgdb.cn/item/64acc6bc1ddac507cc56db04.jpg)
  - 打开**环境变量**
    ![](https://pic.imgdb.cn/item/64acc6fb1ddac507cc57fdb2.jpg)
  - 双击**Path**，点击**新建**，粘贴刚刚复制的路径
    ![](https://pic.imgdb.cn/item/64acc83d1ddac507cc5e22ff.jpg)
    ![](https://pic.imgdb.cn/item/64acc8af1ddac507cc6054ff.jpg)
  - 完事之后键盘按下`Win+R`，输入`cmd`并回车打开cmd。输入`adb`并回车，如果没报错就算成功，如果报错就重启电脑
    ![](https://pic.imgdb.cn/item/64acc9601ddac507cc6341bb.jpg)

  

- 电脑先下载安装包并找到位置，复制安装包路径
  ![](https://pic.imgdb.cn/item/64accac21ddac507cc69cb02.jpg)

- 键盘按下`Win+R`，输入`cmd`并回车打开cmd，输入`adb connect 127.0.0.1:58526`回车连接上Android子系统

  > 如果报错可能是没启动Android子系统

- 连接上Android子系统后输入`adb install 安装包(apk)路径`回车后就成功了

- 在**应用**里启动应用
  ![](https://pic.imgdb.cn/item/64accc4e1ddac507cc70603a.jpg)



***Written by Tong Li***


