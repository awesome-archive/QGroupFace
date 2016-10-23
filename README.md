# Q群爆照图片查找

通过 Face++ API 实现在QQ群图片记录里查找爆照图片，**包括被撤销的图片**，学校新生咨询群必备良品。  

实测不会有漏网之鱼，不过会有一些表情包混进来，谁让表情包也是人脸。![](image/sad.jpg)  

## 实现
Windows版QQ在收到Q群图片后会将图片存在 `数据目录\Q号\Image\Group\` （该目录默认在*我的文档* 里），即使对方撤销也不会被删除。所以遍历该目录下的图片，初步过滤后将剩余文件交给人脸识别API，把带人脸的图片复制到新目录。

### 过滤
1. 文件大小 < 12KB 或 > 1MB
2. gif 后缀
3. < 50KB 且 含大量白色像素的表情包，默认白色像素超过50%为表情包

## 用法
打开[release下的work.bat](release/work.bat)修改其中的图片路径，保存后双击运行。

参数：

	java -jar QGroupFace.jar 源图片目录 输出目录 [复制后是否删源文件（Y/N）] [线程数（默认10）]

如：

	java -jar QGroupFace.jar E:\Software\QQ_data\10001\Image\Group\Image7 E:\test

如需将输出重定向到当前目录文本里则加上 `>> log.txt` 。

然后慢慢等待，最好去吃个饭，10个线程情况下100个文件大概1分钟，10个线程比较合适，线程过多会出现Face++的异常。

## 注意事项
1. 使用前建议先用QQ自带的消息删除器删除很久前的图片，可以的话也用[批处理文件（记得改路径）](release/doFliter.bat)过滤删除一些不太可能是皂片的文件。 
2. 调用API需要 `API Key` 和 `API Secret`，代码里已使用官方的测试Key，貌似是上线版（不限制并发数），而自己注册的账号默认是并发数限制为3。
3. 线程数设置太高可能导致face++反馈异常。
4. QQ有些图片是 `.null` 为后缀，实际上是图片，默认当作 `jpg` 处理。
5. QQ有些动图也被保存为 jpg 格式。

## License
**The MIT License**