# 趣头条游戏中心离线包

为了提升游戏加载速度，趣头条App会提前把**首屏游戏资源**打包到客户端，在访问游戏资源的时候直接走客户端本地。

在第一次接入离线包的时候游戏方需要提供：

1. 游戏名称（必须）：和游戏中心显示的一致。
2. 游戏链接（必须）：可以完整进入游戏。
3. 资源路径（必须）：游戏访问资源相对于域名所在的路径，游戏中心只会抓取这个路径下的资源，路径外的资源不会管。
    
**案例：**

    1. 游戏名称：发发摇钱树
    2. 游戏链接：http://newidea4-gamecenter-frontend.1sapp.com/game/prod/ffyqs/index.html?app_id=a3yYtwjTsHvJ&source=286100
    3. 资源路径：/game/prod/ffyqs/

我方(钱程 微信:MazeyQian)会根据这些信息给游戏方生成一个 `offlineId`，并且此时游戏已经拥有访问的功能，在切换底导和重启App的时候会自动检测离线包资源是否更新并下载资源。

后续游戏资源更新的时候可依照[文档](http://image-slim.qttfe.com/#/?id=_4%e3%80%81%e4%b8%bb%e5%8a%a8%e6%9b%b4%e6%96%b0%e7%a6%bb%e7%ba%bf%e5%8c%85)主动更新离线包，[推荐]也可以将资源路径下的资源打包（ZIP）给游戏中心，让游戏中心手动更新下离线包（为了更稳定的更新离线包，自动更新已关闭）~~并且我方后台会自动以**3分钟**一次的频率拉取游戏服务器资源，若对比游戏资源有更新的话会更新离线包~~。

主动更新 Postman 参考：

![update](./image/offline-update-case.png)

![query](./image/offline-query-case.png)

## Q&A

### Q: 资源路径是什么?

A: 以发发摇钱树为例。

![发发摇钱树资源路径](./image/offline-bag-path.png)

提供的路径是 */game/prod/ffyqs/*，则只会离线 */ffyqs/* 下面的 */res/* */ani/* 等资源，而 */moregame/* 路径下面的资源不离线。

### Q: 提供打包资源的时候，打包的是什么?

A：以发发摇钱树为例。

路径是 */game/prod/ffyqs/*，则提供的是 */ffyqs/* 下面的打包资源。

![发发摇钱树ZIP](./image/ffyqs-offline-zip.png)

### Q：离线包会有什么限制？

A：目前大小限制在 2.5M 之内，超过这个大小会更新失败。

### Q：打包资源有什么建议？

A：离线包更频率越小越好，建议只打包不常更新的静态资源，迭代比较快的资源建议剔除掉（例如：index.html，业务 JS 文件）。
