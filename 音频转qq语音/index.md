# 

# 音频转QQ语音

## 效果：（把音频通过QQ语音发出去）

![](https://pic.imgdb.cn/item/64a79ff41ddac507cc5593ee.jpg)

## 教程

### 原理

QQ录音的时候会在路径：

`/storage/emulated/0/Android/data/com.tencent.mobileqq/Tencent/MobileQQ/1152077073（你的QQ号）/ptt/202307（年月）/7（日）/`

生成一个`.slk`文件如：`stream_2023070712244224.slk`，把要发送的音频文件重命名为这个`.slk`文件名(后缀也要一样)，这时候再回到QQ发送语音就可以把音频文件以QQ语音的形式发出去

### 实践

*推荐使用**MT管理器**(左右双页面管理方便操作)*

- 来到QQ语音生成文件的位置
  <img src="https://pic.imgdb.cn/item/64a7a4481ddac507cc6ba0d0.jpg" style="zoom:67%;" />
- 小窗口打开QQ(接下来所有的操作，QQ都必须保持小窗口状态挂在屏幕上)
  <img src="https://pic.imgdb.cn/item/64a7a4bc1ddac507cc6e0e73.jpg" style="zoom:67%;" />
- 点击录音，过个几秒后点击暂停
  <img src="https://pic.imgdb.cn/item/64a7a5501ddac507cc70dbcc.jpg" style="zoom:67%;" />
- 可以看到语音文件已经生成了
  <img src="https://pic.imgdb.cn/item/64a7a59d1ddac507cc7271d5.jpg" style="zoom:67%;" />
  <img src="https://pic.imgdb.cn/item/64a7a5e81ddac507cc73f4e6.jpg" style="zoom:67%;" />
- 把要发送的音频复制过去
  <img src="https://pic.imgdb.cn/item/64a7a65a1ddac507cc766d66.jpg" style="zoom:67%;" />
- 把这个`.slk`文件的名字复制起来(包括后缀`.slk`)
  <img src="https://pic.imgdb.cn/item/64a7a6bf1ddac507cc787e6d.jpg" style="zoom:67%;" />
- 复制完就删除掉这个`.slk`文件
  <img src="https://pic.imgdb.cn/item/64a7a6fd1ddac507cc79cc54.jpg" style="zoom:67%;" />
- 把这个要发送的音频文件重命名为刚刚复制的文件名(包括后缀)
  <img src="https://pic.imgdb.cn/item/64a7a74e1ddac507cc7b9829.jpg" style="zoom:67%;" />
- 最后点击发送即可
  <img src="https://pic.imgdb.cn/item/64a7a7811ddac507cc7c9b55.jpg" style="zoom:67%;" />


