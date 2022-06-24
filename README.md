![cover](/Users/rin/Documents/Develop/Projects/compose/yabai/art/cover.png)

> 桌面 QQ 客户端**复活计划**
>
> ~~是的没错，这是又一个第三方 QQ！~~ 现在只是一个**计划，尚未开始**



## 概念

![preview-light](/Users/rin/Documents/Develop/Projects/compose/yabai/art/preview-light.png)

##### ▲ 亮色设计图

------

![preview-dark](/Users/rin/Documents/Develop/Projects/compose/yabai/art/preview-dark.png)

##### ▲ 暗色设计图



## 动机

macOS 上的 QQ 客户端太糟糕了。

> 曾经带给我希望的 catalyst 版本在 0202 年底被抛弃了，而 mac 商店版本都 2202 年了还不添加夜间模式，大晚上所有 App 都是黑色的就这个破 QQ 还是亮色，🥷听了都忍不了
>
> ------
>
> 近期在玩 [mirai](https://github.com/mamoe/mirai) 机器人时偶然发现了 [oicq](https://github.com/takayama-lily/oicq) 存在一个 **vscode** 版本的 ~~QQ 客户端~~ [vscode-qq](https://github.com/takayama-lily/vscode-qq)，并借此线索发现了另一个同样基于 **oicq** 的第三方 QQ 客户端 [Icalingua++](https://github.com/Icalingua-plus-plus/Icalingua-plus-plus). 经过体验，发现完全可以满足日常使用，但还有很多地方不适合我自己，因此本着~~“我行我上”~~的理念决定自给自足，因为不熟悉也不想写 **typescript** 的原因，所以这个创建项目的想法诞生了。



## 想法记录

> 以下想法是为了避免在开始这个项目时遗忘而记录的

#### UI

- 使用 [**Compose**](https://github.com/JetBrains/compose-jb)

  > 难点：
  >
  > 1. 一个易用像 coil 一样的图片加载框架
  >
  > 2. 一个“能用”的视频播放框架
  > 3. 一个用于加载网页的 webview 框架
  >
  > 以上这些在 compose 甚至 swing/javafx 体系中似乎并不存在广泛的流行库，也许只能用很古老的内置解决组件，或者找到一种办法和系统互操作以使用 macOS/Windows 中的原生视图

- 使用 [**Flutter**](https://flutter.dev/)

  > 问题：
  >
  > 1. Flutter 的桌面支持似乎不太友好
  > 2. Dart 是我蛮讨厌的糟糕语言
  >
  > 优势：
  >
  > 1. 生态完善，几乎所有需要用到的东西都存在
  > 2. 性能似乎也不错

- 使用 [**React Native**](https://reactnative.dev/) 或者 [**Electron**](https://www.electronjs.org/)

  > 虽然似乎很好，但是不想写 ts/js

#### 协议

- 使用 [**Mirai**](https://github.com/mamoe/mirai)

  > 几乎是想法诞生之初的首选，因为是最爱的 Kotlin
  >
  > 但作为开发 bot 来说很好，对于开发客户端来说的话存在一些问题：
  >
  > 1. 无法获取收藏的漫游表情
  > 2. 无法获取 Cookie，意味着有一些很多存在的 QQ Http Api 都无法直接使用
  > 3. 似乎缺少一些个人 Api，例如修改昵称、修改头像等
  > 4. 似乎缺少一些主动拉取消息的 Api，例如首次最近消息、或者获取历史消息等

  > 作为解决方案，也许可以
  >
  > - 将 mirai 同步为子模块以修改不满足的部分（需要学习协议的时间）
  > - 找到一种可以让多个协议库协同运行的办法，因为有一些 API 在其他协议库中是提供的，例如 oicq 就不存在上面的问题

- 使用 [Oicq](https://github.com/takayama-lily/oicq)

  > 协议似乎比 mirai 更丰富，作为客户端来说是一个很好的选择
  >
  > 但使用该协议意味着需要维护一个 HTTP API 后端？



## 思路

```flow
start=>start: 登陆
cache=>condition: 读取数据库消息缓存

empty=>subroutine: 创建一个在后台填充的空列表
empty-fill=>operation: 主动获取每个好友以及群聊的最后一条消息

show=>inputoutput: 显示消息列表
sync=>operation: 开始监听并缓存接收的消息
end=>end: 登陆步骤完成

start->cache
cache(yes)->show
cache(no)->empty->empty-fill->show
show->sync->end

```

> and more...



## 留言

想法很多，精力很少；创建这个项目纯粹是心血来潮和兴趣使然，因此这会是一个哪天有空了就写一点的长期蜗牛项目。

同时欢迎任何感兴趣和有能力的大佬来一起复活 QQ for desktop, 可以在 issue 或 [tg@chachako](https://t.me/chachako) 和我交流想法
