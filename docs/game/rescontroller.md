# 关于游戏资源版本控制的思路  

<div class="book-tip">
游戏发布上线之后，如果要更新某个js代码、资源（图片，json，js）等，由于浏览器缓存原因，在服务器上更新资源后，客户端不会下载最新的资源。
</div>

## 方法一：重写白鹭的`getVirtualUrl`方法  
- 优点：
    1. 一处修改，所有资源可以得到更新
- 缺点：
    1. 每次修改过后，客户端加载游戏都要重新加载所有资源；

## 方法二：修改 `default.res.json`  
- 优点：
    1. 每次修改过后，客户端加载游戏只需加载更新的资源；
- 缺点：
    1. 需要对每次更新的所有资源都进行修改；

## 方法三：插件控制流  
- 优点：
    1. 不需要修改任何地方，只要该文件路径；
    2. 增加版本号或crc32校验码比对；



## 参考文章  
* [白鹭官方文档（旧）- RES版本控制](http://developer.egret.com/cn/article/index/id/671)
* [白鹭官方文档（新）](http://developer.egret.com/cn/github/egret-docs/extension/RES/RESVersion/index.html)
* [另外一位大神提供的思路](https://blog.csdn.net/sujun10/article/details/77231215)