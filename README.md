# 禁漫天堂下载器

使用Github Actions的禁漫天堂爬虫🤡

java这门语言能让小项目变成中项目, 中项目变成大项目 🤡

没什么牛逼的地方, 就是春节假期图一乐🤡

## 更新记录

- 2021/3/27 14:04 v3.0.0 放弃springboot框架, 换成使用响应式框架quarkus, 感觉应该是更快了的.
- **2021/2/25 15:16 v2.0.0重大更新, 支持直接使用Github Action自动爬取漫画, 不需要本地部署, 直接输入漫画url然后等待Github Action爬取完成然后下载压缩包即可.**
- 2021/2/18 22:15 v1.2.0支持下载整本只有一章的无章节漫画
- 2021/2/18 16:35 添加下载单独某个章节的功能.
- 2021/2/18 14:27 修复保存失败时引发的程序停止运行.
- 2021/2/18 15:38 确保配置文件中配置为空时程序能够正常识别.
- 2021/2/17 20:51 修复由于章节列表格式不规范导致的获取章节名称失败.

## Github Action使用方法

v2.0.0之后现在支持直接使用Github Action进行下载, 不需要手动部署了.

Github Action是微软收购github之后推出的CI/CD工具, 你可以理解为这是一台微软免费给你白嫖的2核7G内存的服务器, 每次提交代码都可以触发运行一次服务器.

现在程序支持提交代码之后直接通过这个服务器帮你下载完成漫画, 然后打成一个压缩包, 提供给你下载.

**感谢微软, 微软大法好🙌**

通过以下步骤即可在GitHub Action上运行程序

1. 点击图中fork按钮, fork一份我的项目给你自己.![image.png](https://i.loli.net/2021/02/25/r1EzkUtY4agP3sA.png)

2. 进入`/src/main/resources/downloadPath.json`目录, 点击箭头所指的编辑按钮,
   对该文件进行编辑.![image.png](https://i.loli.net/2021/02/25/gxre6j2PVYnl53d.png)

3. 按照json格式填入漫画链接, 如果要下载一本, 那格式为 :
    ```json
    [
      "https://18comic.vip/album/180459/"
    ]
    ```
   如果要下载两本或者多本, 格式为(注意逗号) :
    ```json
    [
      "https://18comic.vip/album/180459/",
      "https://18comic.vip/album/182168"
    ]
    ```

   注意尽量不要添加太多漫画, 否则下载起来时间要很久, 压缩包也会很大, 并且添加的链接要是直接能访问到禁漫天堂的网站, 而不是各种镜像站, 根据禁漫天堂的公告板提示, 你所添加的漫画域名应该是以下几个网站 :

   > [https://18comic.vip](https://18comic.vip/)

   > [https://18comic.org](https://18comic.org/)

   > [https://18comic1.one](https://18comic1.one/)

   > [https://18comic2.one](https://18comic2.one/)

   添加完成之后, 点击下方提交按钮 :

   ![image.png](https://i.loli.net/2021/02/25/O745iyUbfZvBDSN.png)

4. 提交完成之后进入Actions页面查看程序运行状况 :![image.png](https://i.loli.net/2021/02/25/2h4n9q1LuFKCeB6.png)

   ![image.png](https://i.loli.net/2021/02/25/BgwedXxFGtThRC9.png)

   绿色说明运行成功, 黄色说明正在运行, 红色说明运行失败. 运行成功之后, 点击对应的任务 : ![image.png](https://i.loli.net/2021/02/25/gFdOoTW4vtrU9zS.png)

   点击箭头所指的**finder-result**文件, 即可下载已经打包好的爬虫图片, 注意这里下载github文件取决于你访问github的速度, 如果没有科学上网可能需要下载很久.

## 本地打包

1. 安装jdk16.

2. 安装maven.

3. 下载源代码并且修改**application.properties**文件中的以下3个配置 :

    - comic.download.path : 下载到本地的目录
    - comic.proxy.host : 科学上网的ip
    - comic.proxy.port : 科学上网的端口

```properties
comic.download.path=C:\\Users\\jiayao\\Pictures
comic.proxy.host=127.0.0.1
comic.proxy.port=10808
   ```

4.执行`mvn clean package` 得到最后的jar包

如果直接下载我提供的jar包, 请在jar包所在的目录**新建一个config文件夹**, 在里面新建一个**application.properties**文件, 然后粘贴并且修改上述三个参数

## 运行程序

程序现在支持两种运行模式, 作为单次运行的前台模式, 和作为持续运行服务的后台模式

### 前台模式

前台模式是指程序完成下载任务之后会自动关闭, 通过读取**downloadPath.json**文件内的链接或者启动时传入参数来进行下载, 每一次下载都要运行一次程序.

如果有条件手动打包程序, 则进入`/src/main/resources/downloadPath.json`目录, 按照json格式填入漫画链接, 如果要下载一本, 那格式为 :

```json
[
  "https://18comic.vip/album/180459/"
]
```

如果要下载两本或者多本, 格式为(注意逗号) :

 ```json
 [
  "https://18comic.vip/album/180459/",
  "https://18comic.vip/album/182168"
]
 ```

前台模式与后台模式都支持下载整本漫画或者单独某个章节.

添加数据之后, 打包, 然后在确保已经有jdk 16之后, 命令行中进入jar包所在的目录, 执行`java -jar ./*.jar`即可按照前台模式运行程序,程序会自动下载json文件中的所有漫画 , 当下载完成之后, 程序会自动退出.

如果没有条件打包程序, 但是有条件运行程序, 例如只装了jdk16但是没有装maven等, 那就直接下载我提供的jar包, 然后命令行进入jar所在的目录, 执行`java -jar ./*.jar 漫画路径1 漫画路径2`
,例如 `java -jar ./*.jar https://18comic.vip/album/180459` , 这时候这本漫画就会被下载, 多本漫画请用空格隔开.

### 后台模式

后台模式是指程序将会作为一个服务持续运行, 通过等待接口请求来下载漫画, 每一次请求接口就会进行一次下载, 程序完成下载后不会自动关闭.

程序打包或者下载我提供的jar包, 然后在确保已经有jdk 16之后, 命令行中进入jar包所在的目录, 执行`java -jar ./*.jar -s`(注意-s参数)即可按照后台模式运行程序 , 当下载完成之后, 程序会继续等待服务.

运行程序之后打开浏览器, 在地址栏输入 :

```url
http://localhost:7788/finder/download?homePage=你想要下载的漫画主页
```

即可开始下载整本漫画, 例如

```
http://localhost:7788/finder/download?homePage=https://18comic.vip/album/177680
```

如果想要下载单独的某一个章节, 只需要输入对应的章节主页即可, 例如

```
http://localhost:7788/finder/download?homePage=https://18comic.vip/photo/211115
```

## 项目特点

- 使用quarkus框架
- 支持github actions
- 没有用到爬虫库, 纯用hutool对html页面进行切分.
- 多线程异步爬虫.
- 对2020-10-27之后的反爬虫图片进行了反反爬虫处理.

## TODO

- [x] 支持github action
- [x] 支持单独下载某个章节而不是每一次都下载完整的一本漫画.
- [x] 支持下载整本只有一章的无章节漫画.
- [x] 通过指定外部配置文件来覆盖内部配置文件, 对于直接下载jar包的用户不需要先解压修改配置文件后再压缩回去.