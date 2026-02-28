# Notanote FAQ

## 如何迁移存档

Notanote目前只有Steam端有云存档，其他平台玩家可以手动迁移。步骤如下：

1. 找到存档文件，各设备存档文件路径如下：

    - **Windows**
      ```Notanote 安装路径\Save\PlayerPrefs_Release.xml```
      若您是通过Steam安装的Notanote，在Steam的“库”中，右键左侧游戏列表中的Notanote，点击“管理”—“浏览本地文件”即可找到Notanote安装路径。
      ![](steam_find_file_path.png)
    - **Android**
      ```/storage/emulated/0/Android/data/game.taptap.zzzProject.Notanote/files/Save/PlayerPrefs_Release.xml```
      若您的安卓设备出现没有访问权限的系统提示：
      如果当前使用的是系统文件管理器或第三方文件管理器，您可以尝试使用其他文件管理器访问。推荐使用MT管理器，并在应用顶部的地址栏中输入以下路径直接跳转（如果有多个用户，请将0改为用户ID或将```/storage/emulated/0/```改为```/sdcard/```）：
      ```/storage/emulated/0/Android/data/game.taptap.zzzProject.Notanote/files/Save/```
      也可以通过右上角添加书签，在底部上滑+号进行快速访问。也可以尝试不使用系统自带文件浏览器，使用原生安卓的文件
      文件进入方式参考：
      打开MT管理器，点击安装包提取—选择系统应用—搜索文件—点击文件，选择左下角更多—启动应用。可以在进入Notanote存档文件夹后选择复制到内置存储（```/storage/emulated/0/```，一般就是文件的根目录）后到浏览器中访问。如果还是无法访问，只能通过连接电脑或使用ADB权限访问。在较高版本的安卓系统中，可能会出现此问题。若开发者选项中启用了USB调试，并且开启了“无线调试”，可以在没有电脑的情况下，使用Shizuku进行无线ADB调试。更多说明请参见Shizuku的相关文档。
    - **iOS**
      ```Notanote 安装路径/Save/PlayerPrefs_Release.xml```
      Notanote 安装路径可在系统自带文件管理器中“我的iPhone”或者“我的iPad”中找到。

2. 将存档文件复制到目标设备的存档路径，覆盖原有的 ```PlayerPrefs_Release.xml```。

## 遇到bug如何反馈

建议您通过以下途径反馈，**反馈时请附上问题相关的截屏或录屏，并截图Notanote—设置—账号设置中的设备信息，方便工作人员更快修复**：

- 邮箱：[Feedback@notanote.cn](mailto:Feedback@notanote.cn)
- 官方QQ群：[1群](https://qm.qq.com/q/fGXBLixNe0) / [2群](https://qm.qq.com/q/9J5ZxtfQcM)
- 官方Discord服务器：[Notanote](https://discord.gg/tCqyBQPg2c)

通过以下途径反馈，工作人员难以及时收到：

- [TapTap论坛](https://www.taptap.cn/app/559514/topic)
- [Steam社区](https://steamcommunity.com/app/2687160/discussions/)

## 曲目到底是怎么分类的

re:v2.0.0后，游戏中根据谱面的表演，将不同谱面分为六种颜色，部分谱面不属于任何颜色，同一首曲目的不同谱面也可能分属于不同的颜色。[「无色之诺」](?p=chapters/Void&l=zh-CN)章节为剧情曲目，[「汇彩之霓」](?p=chapters/Neon&l=zh-CN)章节为联动曲目。

## 现在剧情曲目和剧情为什么都需要糖果解锁吗

re:v2.0版本之后，新增了“满足任意一项”的解锁方式，查看解锁条件时请注意解锁条件上方的字。

## 糖果有什么用

糖果可用于解锁曲目、角色等，通过游玩谱面可以获得糖果，详细的糖果掉落机制可查看[此处](?p=mechanics&l=zh-CN#Candy)。

## Notanote有哪几个难度

Notanote目前有 **<ruby><rb>SY</rb><rt>Story</rt></ruby>**、**<ruby><rb>SY</rb><rt>Story</rt></ruby>+**、**<ruby><rb>EZ</rb><rt>Easy</rt></ruby>**、**<ruby><rb>EZ</rb><rt>Easy</rt></ruby>**+、**<ruby><rb>TL</rb><rt>Tale</rt></ruby>**（v2.0 前叫做 **<ruby><rb>EX</rb><rt>Extra</rt></ruby>**）、**<ruby><rb>NT</rb><rt>Nightmare</rt></ruby>**、**<ruby><rb>SP</rb><rt>Special</rt></ruby>** 七个难度。SY、SY+、EZ、EZ+的谱面共用一个模板，TL难度不一定低于SY，NT仅主线有且高于SY，EZ+难度仅[「Innocent white」](?p=songs/Innocent_white&l=zh-CN)拥有。一个曲目至少拥有SY难度（有的只有SP难度，如[「The Fire」](?p=songs/The_Fire&l=zh-CN)），但不一定有其他难度。

## [糖果罐](?p=collectible_list&l=zh-CN#Candy_Can)有用吗

糖果罐在v2.7中用于解锁限时时装[「诺塔 - 蝾螈双子」](?p=character_list&l=zh-CN#Nota-Twin_Axoltls)与[「诺特 - 蝾螈双子」](?p=character_list&l=zh-CN#Note-Twin_Axoltls)，现在已无用途，无需开启。

## Nrk是怎么计算的

v2.5及之后的计算公式如下：

单首曲目Rank的计算公式如下：

$$
\text{单首曲目Rank} = \begin{cases}
  \frac{e^{\text{Acc}-0.5}-1}{e^{0.5}-1} \times (\text{定数}+5) & \text{Acc} \in [50\%, 100\%] \\
  0 & \text{Acc} \in [0\%, 50\%)
\end{cases}
$$

Nrk的计算公式如下：

$$\text{Nrk} = \frac{\sum\limits_{i=1}^{31}(R_i \times \omega_i)}{\sum\limits_{i=1}^{31} \omega_i}, \omega_i = 1 - 0.02 \times (i-1), 1 \le i \le 31$$

其中$R_i$为单首曲目Rank逆向排序的第$i$位，同一首歌多个难度的单首曲目Rank仅记录最高值。

详细的Nrk机制可查看[此处](?p=mechanics&l=zh-CN#Nrk)。

## 分数与准确率是怎么计算的

分数的计算公式如下：

$$\text{分数} = 1000000 \times \frac{\text{Perfect数}+\text{Good数}}{物量}$$

准确率的计算公式如下：

$$\text{准确率} = \frac{\text{Perfect数}+\sum\limits_{i=1}^{\text{Good数}}(40\%+\frac{120-|\text{Good偏移}_i|}{100})}{\text{物量}}$$

详细的分数与准确率机制可查看[此处](?p=mechanics&l=zh-CN#score_and_acc)。

## 如何查分

**[Notanote 查分器](http://xuziyao.com/notanote/best/)**

此工具由[大松_Dason](?p=charter_list#Dason)开发，如有相关问题请询问其个人，而非官方。
