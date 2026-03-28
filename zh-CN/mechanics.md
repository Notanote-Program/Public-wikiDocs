# 机制

<a id="judgement"></a>

## 判定

Notanote中会根据玩家打击note的时间偏差分为四种判定：

- Perfect：打击偏差在±70ms内
- Good：打击偏差在±120ms内
- Bad：打击偏差在±130ms内
- Miss：打击偏差超过±130ms或未打击

<a id="score_and_acc"></a>

## 分数与准确率

分数与准确率（俗称Acc，Accuracy的缩写）用于衡量玩家每局游戏的表现。玩家的Nrk跟准确率相关，而非分数。

分数的计算公式如下：

$$\text{分数} = 1000000 \times \frac{\text{Perfect数}+\text{Good数}}{物量}$$

准确率的计算公式如下：

$$\text{准确率} = \frac{\text{Perfect数}+\sum\limits_{i=1}^{\text{Good数}}(40\%+\frac{120-|\text{Good偏移}_i|}{100})}{\text{物量}}$$

<a id="Nrk"></a>

## Nrk

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

<a id="drop"></a>

## 掉落物

<a id="Candy"></a>

### [糖果](?p=collection_list&l=zh-CN#Candy)

糖果为游戏中的货币，可以用于解锁曲目、解锁角色等。

一次游玩获得的糖果数为：

$$
\text{获得糖果} = \begin{cases}
  \lfloor (G+70 \times S \times Acc \times D \times  T \times P) \times R \rfloor & \text{非Autoplay} \\
  1 & \text{Autoplay}
\end{cases}
$$

不同评级对应的$G$值如下：

$$
G = \begin{cases}
  40 & \text{X或S} \\
  34 & \text{A+} \\
  30 & \text{A} \\
  26 & \text{A-} \\
  22 & \text{B} \\
  18 & \text{C} \\
  10.8 \times \frac{\text{分数}}{1000000} & \text{F}
\end{cases}
$$

$S$的计算公式如下：

$$
S = \begin{cases}
  0.4 \times (\frac{s-0.9}{0.1})^2+0.6 & s \in [0.9, 1] \\
  0.6 \times \frac{s}{0.9} & s \in [0, 0.9)
\end{cases}, s = \frac{\text{分数}}{1000000}
$$

$D$的计算公式如下：

$$
D = \begin{cases}
  \max(1,0.2+0.1 \times {\text{定数}}) & \text{定数} \ge 5.0 \\
  1-0.12 \times (5-{\text{定数}}) & \text{定数}<5.0 \\ 1 & \text{SP}
\end{cases}
$$

$T$的计算公式如下：

$$T = 2^{\frac{\text{时长}}{60}-2}$$

目前确保曲目时长在100秒～4分钟内$T$的数据性质良好。

$P$的计算公式如下：

$$P = 1+\text{首次游玩加成}+\text{FC/AP加成}$$

$$
首次游玩加成 = \begin{cases}
  0.5 & \text{首次游玩} \\
  1 & \text{首次游玩，且有版本限定活动的首次游玩加成} \\
  0 & \text{非首次游玩}
\end{cases}
$$

$$
\text{FC/AP加成} = \begin{cases}
  0.2 & \text{非首次FC} \\
  0.5 & \text{首次FC} \\
  0.4 & \text{非首次AP} \\
  1 & \text{首次AP}
\end{cases}
$$

$R$为倍率，计算公式如下：

$$
R = \left( \begin{cases}
  1 & \text{\text{拥有糖果}}\le 20000 \\
  1-0.01\times\lfloor\frac{\text{拥有糖果}-20000}{1000}\rfloor & 20000<\text{拥有糖果}\le 100000 \\
  0.2 & \text{\text{拥有糖果}}>100000
\end{cases} \right) \times \text{版本活动加成}
$$

<a id="avatars"></a>

### [头像](?p=avatar_list&l=zh-CN)

头像掉落概率的计算公式如下：

$$
\text{掉落概率} = \begin{cases}
  0.5 \times \left(\max \left( \frac{s-0.95}{0.05}, 0 \right)^3+0.5 \times \max \left( \frac{acc-0.94}{0.06}, 0 \right)^2 \right) \times 30\% & \text{非满分} \\
  25\%+0.5 \times \max \left( \frac{acc-0.94}{0.06}, 0 \right)^2 \times 25\% & \text{非首次满分} \\
  50\%+0.5 \times \max \left( \frac{acc-0.94}{0.06}, 0 \right)^2 \times 25\% & \text{首次FC} \\
  75\%+0.5 \times \max \left( \frac{acc-0.94}{0.06}, 0 \right)^2 \times 25\% & \text{首次AP}
\end{cases}
$$

头像分为曲目头像和全局头像（非曲绘截取的头像），选择掉落头像规则如下：

- 若游玩曲目拥有可掉落的对应曲目头像，则掉落对应曲目头像，有多个时随机掉一个。
- 若无，有40%概率掉落其他头像。
- 若通过上一条检验（即掉落其他头像），则有33%或67%的概率随机掉落一个全局头像或其他曲绘头像。
- 若本次仅掉落糖果，则额外有 $\frac{\max ({\text{Acc}}-0.92, 0)^2}{0.08^2} \times \text{定数}$ 的概率掉落随机头像。

<a id="collections"></a>

## [收集物](?p=collection_list&l=zh-CN)

<a id="Candy_Can"></a>

### [糖果罐](?p=collection_list&l=zh-CN#Candy_Can)

可以往糖果罐里装糖果，根据当前糖果量分为空、少量、中量、大量四种状态。糖果罐最多能储存3000糖果，1-999为少量，1000-1999为中量，2000-3000为大量。

当糖果罐根据是否触发分为开关两种状态，当装满时必定为关闭状态,使用糖果罐可切换糖果罐的开关状态。当为打开状态时，每次游玩结算时获得的糖果将进入糖果罐（掉落部分），同时额外将当前持有糖果的3%（下取整），最多为300颗糖果放入糖果罐（转移部分）。

使用糖果罐可选择清空糖果罐，此时糖果罐中的所有糖果将移入糖果数，进入正常统计。

装满的糖果罐有特殊用途：

- <span style="color: gray">（已结束）v2.7万圣节活动，兑换时装[「诺塔 - 蝾螈双子」](?p=character_list&l=zh-CN#Nota-Twin_Axoltls)与[「诺特 - 蝾螈双子」](?p=character_list&l=zh-CN#Note-Twin_Axoltls)</span>

<a id="Four_Leaf_Clover"></a>

### [四叶草](?p=collection_list&l=zh-CN#Four-Leaf_Clover)

使用后，获得「不稳定幸运」状态：期间，掉落物（收集物、头像等）掉落概率提升，糖果掉落数量降低。当第一次遍历后未掉落物品（除糖果外），开启第二次遍历，糖果掉落数量降低20%。在「不稳定幸运」状态下，第二次遍历获得的收集物与降低掉落数量的糖果右上角会标有四叶草图片。
