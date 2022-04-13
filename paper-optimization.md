# **介绍/背景**

你好呀！欢迎阅读 EterNity 的非正规 Paper 优化指南。运行服务器并非易事，互联网上也在共享相互矛盾的信息，有时甚至是完全错误的指导；因此，我决定编写本指南以帮助澄清一些误解并纠正许多腐竹和其他优化指南所犯的常见错误。我绝不是这个这方面的大手子，如果你发现任何错误，请帮助我纠正我的错误。

### 最后更新时间：2022 年 3 月 21 日，针对 Paper 版本 1.18.2 Build #274

### **[本指南适用于 Paper 1.18 版 如果您正在寻找 1.16/1.17 版指南，请查看此处。](https://eternity.community/index.php/paper-optimization117/)**

### 我建议运行最新版本的 Paper 服务器以接收性能补丁并避免漏洞利用和崩溃。

---

# **必备软件**

### Windows 用户可使用的软件

| 软件名| 备注|
| --- | --- |
| [Visual Studio Code](https://code.visualstudio.com/) | 免费，易用的代码编辑软件（npp作者的妈寄了） |
| [WinSCP](https://winscp.net/eng/index.php) | SFTP 文件传输客户端(可用于面板服用户) |

WinSCP **强制启用二进制模式** : 选项 > 首选项 > 传输 > 二进制

---

# **入门**

* **初始设置**
  * 使用[本指南](https://paper.readthedocs.io/en/latest/java-update/index.html)安装 JAVA
  * 请注意，最新的 Paper 1.18 版本需要 JAVA 17。
  * 从现有的 官方服务端、bukkit 或 spigot 服务器迁移
  * 无需额外操作！只需用 Paper 替换服务器核心。
* **获取服务器 Jar**
  * 从[Paper 官方下载页面](https://papermc.io/downloads)下载服务器 jar
    ~~或使用[Paper Download api](https://papermc.io/api)请求 jar 链接~~
* **设置根目录并同意 EULA** （仅适用于首次启动）
  * 为服务器创建一个根文件夹
  * 创建一个名为 **eula.txt** 的文本文件并填写<strong>eula=true</strong>。
    * *如果您在 Windows 上托管，请避免将根文件夹放在桌面或启用 OneDrive 同步的任何位置。当你这样做时，神奇的事情就会发生，而你不会喜欢的。（变相备份了属于是）*
* **启动服务器**
  * 通过终端或使用启动脚本文件启动。
    * [**通过 startmc.sh 为您的服务器生成启动脚本**](https://startmc.sh/)
    * 在下面阅读有关 JVM 参数重要性的更多信息
    * 高级阅读[Prof_Bloodstone 的指南](https://gist.github.com/Prof-Bloodstone/6367eb4016eaf9d1646a88772cdbbac5#file-start-sh)

#### 第一次在 Windows 上运行并且上面的说明难懂？

#### 来查看[Paper 酱的 Minecraft 服务器开服小教程](https://eternity.community/index.php/minecraft-self-hosting)吧

# **配置和优化**

### **在我们开始之前……**

**并没有适用于每台服务器的固定参数值。** 你应该阅读并理解每个可用的配置选项，并相应地调整相应值以适应你自己的情况。服务器的最佳配置将根据服务器硬件、平均玩家数量和运行的游戏模式类型而有所不同。下面显示的任何值都作为示例，请进行测试和实际操作，然后得出您自己的一组参数。

**随着世界被开发和玩家进入游戏的后期，在服务器上的工作量会随着时间的推移而逐渐增加，因此服务器优化不是一次性的任务，而是持续的努力。**

使用最新 Minecraft 版本，在默认（官方服务端）设置下运行需要不错的硬件，并且对于服务器资源非常有限的一些中小型服务器可能不可行。除了[选择信誉良好的服务商和合适的硬件](https://eternity.community/index.php/paper-optimization/#Hosting-Options)之外，优化配置和对原版游戏玩法做出妥协也变得至关重要。仔细阅读下面提供的配置选项，您将确保顺利运行！

## **预生成地图**

生成新区块会消耗大量资源，如果您要添加新地图/开新服务器，建议您预先生成地图。

如果你不打算设置世界边界，从世界出生点中为中心预先生成 5000-10000 个区块仍然是一个好主意，因为这将有助于缓解你服务器刚开荒时的压力。更不用说，它会在实际开荒日之前定位任何潜在的未遇到的错误。

* 通过 [pop4959](https://github.com/pop4959) 制作的 **[Chunky](https://www.spigotmc.org/resources/chunky.81534)** 和 **[​​ChunkyBorder](https://www.spigotmc.org/resources/chunkyborder.84278)** 插件进行实现
  * Chunky 是最简单的预生成插件，结合 ChunkyBorder，您可以根据自己的喜好自定义边界形状。
  * 选择边界时请尽量合理一些，设置边界越远，文件大小将呈指数增长，这可能会导致以后的[存储和备份](https://eternity.community/index.php/paper-optimization/#Backup-and-Recovery-Best-Practice)问题。

## **选择最佳视距和模拟距离**

**simulation-distance** 确定玩家周围有多少区块处于活动状态（ticking）。
**view-distance** 确定玩家可以看到多少区块（地形）。

**simulation-distance对性能有极大影响，** 因此具有较低的值将有助于保持无延迟环境。原版服务端对 Minecraft 的默认值是 10，YouTube 上的大多数生电装置设计都是基于这个值。降低此值将影响我们稍后将讨论的那些装置。就我个人而言，我不会低于 **5** 不会影响玩家的游戏体验；但是，如果您在玩小游戏或空岛生存，则可以降低很多。

**view-distance** 提供进一步的区块地形视野，虽然性能不如 **simulation-distance** 但占用更多运行内存；如果您决定增加该值，请一点点调高该值以在您的服务器上以找到您的最佳设定。

* 此外，你可以在 **spigot.yml** 中以每个世界为基础定义单独的值来覆盖 **server.properties**。
  (我们将在后面的每个世界的配置部分介绍如何正确地设置这个问题）。

## **控制实体数量**

**当前 Minecraft 版本中的实体是资源密集型的，** 如果你不控制实体，即使是当前市面上最强悍的 CPU 也会超负荷。

* 理想情况下，您希望将 **整体实体tick** 保持**在 30% 以下**
  （假设存在合理数量的玩家活动，而不仅仅是服务器无人的情况下）
* [Timings 是一个很好的工具](https://eternity.community/index.php/paper-optimization/#timings)，可以找到卡服的根源并比较优化的结果。

## **请注意变更之后的影响**

**每一次的改变都伴随着妥协!** 你的大多数玩家只是在逐块复制流行的生电设计，而不知道它究竟是如何运作的，所以作为一个服务器的所有者，了解你的改动的影响是很重要的，这样你才能更好地向玩家解释。在下面的介绍中，我将试图解释每一个变化的影响，以及如何应对妥协的问题。

---

## **了解生物生成机制**

下面是一个关于如何在玩家周围生成生物的演示。图表和指示值是根据原版/paper默认值制作的。

![图片设计：Niome#7667 翻译：WhkSoft](https://pic.whksoft.cn/2022/04/03/87536706df6ae.png)

```
View Distance (视野距离): 10 (区块)
Simulation Distance (模拟距离): 10 (区块)
Mob Spawn Range (生物生成范围): 8 (区块)
Despawn range (软清除范围): 32 (方块)
Despawn Range (硬清除范围): 128 (方块)
entity activation range (生物激活范围): 32 (方块)
```

```
- 棕色圆柱体表示生物生成范围
- 红色球体表示生物生成区（介于 24 到 128 方块之间）
- 黄色球体表示没有生物区，因为没有生物会在玩家附近生成（24 格）
- 任何实体落在 32 方块的环形（实体激活范围）之下，将以正常速度被 tick。
- 任何实体位于 32 - 128 块之间的环形将以降低的速率 tick。
- 任何落在第 128 块之外的实体都会立即消失。
```

**对上述设置的任何改动都需要你相应地调整农场的整体规模和指定的挂机点。**

上述 5 个配置选项彼此密切相关，确保正确设置每个值至关重要。

* **模拟距离**决定了一个农场的最大可能大小。
  * **一个农场不能超过（模拟距离-1）x16块的半径**。
  * **模拟距离**之外的所有东西都不会被 tick，所以任何大于这个值的农场都不会有见效。
* **生物生成范围**确定了农场收集平台的最大尺寸。
  * **生物生成范围应介于（模拟距离 -1） 到 3 之间。**
    * **如果您正在运行 10 的原版默认模拟距离，您可以将生物生成范围保持在 8** 或更低。
    * 生物只能在离玩家至少 24 个方块的地方生成，所以强烈不建议将生物生成范围设置在 **3** 以下，除非你的模拟距离是 3 或更低。
  * 每个生物农场都有一个指定的生物收集平台，平台的大小仅由该值决定（以块为单位）
* **硬清除区域** 或者 **hard despawn-range** 是生物在没有立即消失的情况下可以存在的最远距离，而不是立即清除。
  * **hard despawn-range** 的值决定了农场的理想挂机点。
    
    * 这个值是 **128** ，原版默认的（块）。
      （大多数农场设计会将指定的挂机点设置为稍微在此值范围内）
    * 将此值降低到原始默认值以下意味着生物农场上的挂机点也需要在新 **hard despawn-range** 的范围内进行调整，因为范围限制适用于水平和垂直。
      [（其他重要说明位于后面的部分……）](https://eternity.community/index.php/paper-optimization/#hard-despawn-choice)
  * **hard despawn-range** 应该始终**等于**您的(**mob-spawn-range**)x16 块**并且永远不会低于此值**。
    （这可以防止服务器做无用功，来生成一个生物只是让它立即消失，因为它超出了硬清除范围）
* **entity-activation-range 应该是最后改变的选项，** 因为它对游戏玩法有重大的行为影响。

在对 **农场不工作的原因** 进行故障排除时，请务必检查上述每个配置，并对农场设计进行相应的必要修改。不要在不考虑这些配置的情况下逐块照搬 YouTube 与 B 站上的教程。

查看 [[Paper 酱的修复您的小黑塔的小指南]](https://eternity.community/index.php/nether-troubleshoot)，了解常见故障排除步骤的示例演练。

* 本节中所有提到的 **模拟距离** 是指实际的 **ticking 距离** .
* 利用 **/paper mobcaps** 和 **/paper playermobcaps** 来了解玩家周围生成的额外细节。它对于查找无法生成的错误特别有用。[关于这个功能的更多细节，请点击我。](https://github.com/PaperMC/Paper/pull/6470)

![世界 world 的 mobcaps](https://pic.whksoft.cn/2022/04/03/3b9e57ba7a670.png)

---

## **1.18中的新功能：为什么我在1.17中建立的农场在什么也没动的情况下变慢了？**

**Minecraft 在最低的区块和最高的区块之间运行生成检查（在1.17中从 Y = 0 到 Y = 265），以检查并查看该区块是否有资格进行生成尝试，然后它有 24% 的概率成功在该特定 Y 位置生成。**

#### 农场是根据这一规则设计的，它们的理想位置也相应地被选择。

* **大包围圈** – 在你的农场周围做一个大包围圈，除了农场平台上的指定地点外，其他所有可能的生成地点都被消除。(这可以在 SciCraft, TIS 服务器和大多数农场教程中看到，这些教程使用空旷的超平坦世界进行演示）。
* **Y=0 小黑塔** – 建议将小黑塔建在Y=0，因为这是最有效的位置。
* **在下界基岩层上的下界农场** – 下界农场通常建在下界基岩层的高处，因为在下面的话由于存在大量的熔岩，一般来说做大面积的包围圈是很痛苦的（痛苦面具），而且通过进入下界基岩层的高处，你把你的清除区域移到了天空的高处，以消除生物在你的农场平台之外任何地方生成的可能性。

**尽管如此，由于世界高度的变化，你的农场在 1.18 版本中可能不太有效。它不再检查从 Y = 0 到 Y = 265 的标准生成尝试，而是从 Y = -64 到 Y = 320 来寻找最低和最高的区块来进行生成尝试。你在 1.17 版本中位于 Y = 0 的农场现在多了 64 个额外的方块，这使得生物的生成速度减慢。**

#### 如何缓解这些问题...

* **在世界的最低点重建你的农场，现在是 Y = -64。**
  (这是最理想的解决方案，但同时也是最痛苦的。)
* **挖出更大的范围，并清空农场下面从 Y = -64到 Y = 0 的所有东西。** (就剩下空气了)
* 在 **bukkit.yml** (或 **paper.yml** ) 中定义了 **生怪限制** 的情况下，生物生成会在所有**加载的区块**上进行尝试。
  接受并理解，在多人服务器中，生物生成是有内在缺陷的。
  详细的解释见 [**per-player-mob-spawns**](https://eternity.community/index.php/paper-optimization/#per-player-mob-spawns) 部分。

#### 此外，在 1.18 版本中，生物生成所需的光照度也被改为 0 了。

### ****太长不看版本： 最有效的农场位置是在最低的 Y 层，上面只有空气方块。由于 1.18 版本的世界高度变化，你的农场不再是最理想的位置。****

---

# **server.properties**

### server.properties 的基本配置

```
view-distance=10
```

除非 **spigot.yml** 中另有说明，否则这将设置服务器的**视距**（仅指地形）。

```
simulation-distance=10
```

除非 **spigot.yml** 中另有说明，这设置了服务器的 **模拟距离**（ticking）。

[更多信息请参考《选择最佳模拟和视野距离》。](https://eternity.community/index.php/paper-optimization/#view-distance)

**选择 [一个合适的视距和模拟距离的组合](https://eternity.community/index.php/paper-optimization/#view-distance) 是非常重要的。**

* **如果你决定降低你的视距和模拟距离**…
  *[**请参考《生物生成如何工作》**](https://eternity.community/index.php/paper-optimization/#mobspawn)*，以确保所有其他配置得到相应的调整。
  * **simulation-distance** 应该总是小于等于 **view-distance** .
    （如果模拟距离被设置为高于视距，那么只有视距以内的区域才会被应用）
  * **强烈不建议将这些值降低到 5 以下。**  (在 #203 版本之后，最小值是 **2** )
* **如果你想为玩家提供更多的地形视野** , 请注意，在 10 之后每增加 1，玩家周围加载的区块的总量将成倍增加。
  * 计算单个玩家所加载的总区块数的公式是 **[（视距 +2）x2+1]^2**
    * 对于 **view-distance=10** （纯净版默认），一个的玩家将加载 625 区块。
    * 对于 ****view-distance=5**** ，一个的玩家将加载 225 区块。
    * 对于 ****view-distance=15**** ，一个的玩家将加载 1225 区块。
  * 尽管 **view-distance** 比 **simulation-distance** 使用的资源要少得多，但请注意它对性能的影响，特别是在大型服务器上，每节省一点资源都会有帮助。
* 此外，你可以在 **spigot.yml** 中为每个世界重写/指定视图距离和模拟距离。 我们将在稍后的 [每个世界的配置部分](https://eternity.community/index.php/paper-optimization/#Per-world) 讨论这个问题。
  * 例如，你可以选择一个较高的 **view-distance** 在 **the_end** 维度，这将使你的玩家在虚空中用鞘翅进行滑翔时有更愉快的体验。

---

```
allow-flight=true
```

这可以防止玩家在骑马或爬上脚手架时因为 “飞行”而被服务器踢出。这个选项虽然为 **true**，但并不意味着每个人都可以飞行，它只是意味着如果服务器认为玩家在飞行，他们就不会被踢了。（反作弊都比这个铸币好使）

---

# **bukkit.yml**

### bukkit.yml 的基本配置

```
spawn-limits:
  monsters: 70
  animals: 10
  water-animals: 5
  water-ambient: 20
  water-underground-creature: 5
  ambient: 15
```

这一部分决定了你服务器中的生物上限。降低 `monster` 的数值对服务器的性能有最直接的影响，因为实体是服务器必须处理的最密集的资源任务之一。

[改变生物的出生范围需要你查看其他相关的配置。更多的细节请参考了解生物生成部分。](https://eternity.community/index.php/paper-optimization/#mobspawn)

为了帮助保持 **生物的感知密度** 与原版的默认值一致，也请相应地改变 spigot.yml 中的`mob-spawn-range`，以达到这种幻觉。

下面是关于如何计算这个数字的实际计算。请记住，这是假设理想的生成条件，也就是所有的区块都是可生成生物的区块。

下面还包括一个预设值的表格，以方便您的使用。

```
例如，如果我想将我的怪物上限设置为45，同时保持暴徒密度与以前大致相同，我将解决以下数学方程式

(默认生物上限) : (默认生成区域) = (新生物上限) : (新生成区域)

其中的常数如下
 
默认生物生成范围 = 8 区块
生物可以生成的最小距离 = 离玩家 24 格远的距离

默认生成区域 = [ (生物生成范围 x 2 x 16) +1]^2 - ( 24 x2 +1 )^2 = (8x2x16+1)^2 - 49^2 = 66049 - 2401 = 63648

70:63648 = 45:b ; 其中 b = 新的生成区域 (以方块为单位)

63648 x 45 = 70b

b = 40916
令 a = 新的生物生成范围，其中 b = [ (a x16 x2) +1]^2 - (24 x2 +1)^2, 并且 b = 40916

(32a +1)^2 - 2401 = 40916

(32a +1)^2 = 43317

32a +1 = 208

32a = 207

a = 6.46


然后，我将在 spigot.yml 中把生物的范围设置为6（或7）。
```

**这里有一张预填建议的小抄**，供那些懒得锻炼数学或小学数学不好的人使用，请仔细阅读并应用相应的配置。

| 期望的整体实体数 （%）（对原版而言） | bukkit.yml 推荐的 **spawn-limit （monster）** | spigot.yml 推荐的 **mob-spawn-range** | 实际计算的数量                                              |
| -------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------- | ----------------------------------------------------------------------- |
| 100% (原版)                             | 70 (默认)                                            | 8 (默认)                                       | 8 (默认)                                                           |
| 90%                                        | 63                                                      | 7 或者 8                                            | 7.60                                                                  |
| 80%                                        | 56                                                      | 7                                                 | 7.18                                                                  |
| 70%                                        | 49                                                      | 6 或者 7                                            | 6.74                                                                  |
| 60%                                        | 42                                                      | 6                                                 | 6.26                                                                  |
| 50%                                        | 35                                                      | 5 或者 6                                            | 5.75                                                                  |
| 40%                                        | 28                                                      | 5                                                 | 5.18                                                                  |
| 30%                                        | 21                                                      | 4 或者 5                                            | 4.55                                                                  |
| 20%                                        | 14                                                      | 4                                                 | 3.81                                                                  |
| 10%                                        | 7                                                       | 3                                                 | 2.89                                                                  |
| 3%                                         | 2                                                       | **请升级你的服务器硬件**     | 我小米路由器都能扛得住 |

上面的建议值是为了保持生物密度与原版一致，请随意填写你自己的值，以适用于你的服务器。如有需要，请仔细了解[生物生成部分](https://eternity.community/index.php/paper-optimization/#mobspawn)，以再次验证其他配置。

* **请注意，* **在1.18版本中，怪物生成所需的光照度已被改为 0。*****
  越来越常见的情况是，怪物的可用生成区非常小；因此，它们似乎更集中在一个黑暗的地方，特别是在你的照明基地的边缘或地下的封闭洞穴。
  ***相应地适应这种变化，并根据你的服务器的情况降低这个值！***
* 在最繁忙的时候[分析你的 timing 报告](https://eternity.community/index.php/paper-optimization/#timings)，在最多玩家在线的时候。
  * 整体的实体应该少于你 tick 的 30%，如果你看到两位数的 tick 花在了生物生成上，这意味着你的实体上限太高了，需要相应地减少。
    (理想情况下，你想让你的生物密度尽可能地接近原版，**同时在高峰时段保持低于 50 mspt 的平均水平。** 如果你不能用上面的预设阈值来维持这个值，就继续降低这个值，直到你能实现这个目标)
* 只要生物被立即杀死，而不考虑生物的聚集时间，鉴于农场设计的相应调整，农场的产量应该与原版 Minecraft 大致相同。
* 请注意，目前在 1.18 版本中，**铁傀儡**， **守卫者**不受此配置的限制。需要一个插件来正确限制它们。
  * 更多信息请见下面的[生存质量和必须的插件](https://eternity.community/index.php/paper-optimization/#Quality-of-Life)部分。

除了上面提到的铁傀儡和守卫者之外，游戏中的每个生物都属于以下 7 个类别中的一个。

* **monster** 类别包括烈焰人、洞穴蜘蛛、苦力怕、溺尸、远古守卫者、末影龙、末影人、末影螨、唤魔者、恶魂 、巨人、疣猪兽、尸壳、幻术师、岩浆怪、幻翼、猪灵、猪灵蛮兵、掠夺者、劫掠兽、潜影贝、蠹虫、骷髅、史莱姆、蜘蛛、流浪者、恼鬼、卫道士、女巫、凋灵、凋灵骷髅、僵尸疣猪兽、僵尸、僵尸村民、僵尸猪灵。
* ****animals**** 或者 **creature** 分类包括蜜蜂、猫、鸡、牛、骡子、狐狸、山羊、马、羊驼、驴、哞菇、豹猫、熊猫、鹦鹉、猪、北极熊、兔子、绵羊、骷髅马、炽足兽、行商羊驼、海龟、流浪商人、狼、僵尸马。
* **ambient** 分类包括蝙蝠，仅仅是因为蝙蝠是最没用的生物。
* **water-animals** 或者 **water_creature** 分类包括鱿鱼和海豚。
* **water-ambient** 分类包括鲑鱼，鳕鱼，热带鱼，河豚。
* **water-underground-creature** 或者 **underground_water_creature** 分类只包括发光鱿鱼。
* **axolotl** 分类(paper.yml专属)只包括美西螈。

由于 **paper.yml** 中使用了正确的名称，而 **bukkit.yml** 仍然使用旧的名称，因此有些类别列出了两个不同的名称。

每个实体都会在 timings 报告中单独显示出来，你可以根据上面的类别进行相应的必要调整。

---

```
ticks-per:
  animal-spawns: 400
  monster-spawns: 1
  water-spawns: 1
  water-ambient-spawns: 1
  water-underground-creature-spawns: 1
  ambient-spawns: 1
  autosave: 6000
```

这一部分决定了每一类实体尝试生成的频率（单位为 ticks）。

Minecraft 会一直尝试生成实体，直到它达到上一部分的 **spawn-limits** （生成限制）。改变这里的数值应该是你的次要选择，因为生物上限几乎总是会被达到。你所做的一切只是推迟即将到来的毁灭，**请先在前面提到的生成限制上建立一个适当的实体上限。**

* 计算和验证生物的生成需要耗费资源，最好的办法是先**降低整个实体的生成限制上限**，然后在这里调高 **ticks-per** 配置作为辅助缓冲。请利用 **/paper mobcaps** 和 **/paper playermobcaps** 来监控并确保生物上限始终被达到。如果你的生物上限不能及时达到，这可能表明你的 **spawn-limits** 太高和/或你的 **mob-spawn-range** 太低。
  * 如果你注意到在你的 timings 报告中，花在 **生物生成 (Mob Spawning)** 上的时间比平时多，那么你的 **spawn-limits** 可能太高了，在上面的部分中减少你的生物上限，这样就正常的达到能达到期望值了。
  * 对于 空岛生存/单方块生存 模式来说这一点尤其重要，因为服务器往往很难达到生物上限，因为可生成的区域有限（这意味着服务器不断尝试生成更多生物）。请相应调整配置以解决这个问题。
* 这个选项直接**削弱了所有生物农场的收益率**，因此在这里选择一个理想的数值非常重要，因为它在优化和平衡游戏体验方面是一个很好的调整。

# **spigot.yml**

### spigot.yml 的基本配置

```
view-distance: default
simulation-distance: default
```

> 这是对server.properties中相同配置的重写

* 在这里输入一个值将覆盖 server.properties 里面的值
* 值 **default** 表示服务器使用 server.properties 中的值。
* 如果创建了额外的类别，[该值可以按世界设置](https://eternity.community/index.php/paper-optimization/#Per-world)

---

```
mob-spawn-range: 8
```

> 正如上面 bukkit.yml 部分提到的，这个值可以修改，以保持[生物的感知密度](https://eternity.community/index.php/paper-optimization/#perceived-mob-density)不变。

* **这个值应该总是设置为最大（模拟距离 - 1）** 最小为 3，因为任何实体落在模拟距离之外，与其接壤的区块就不会被 tick。**(见下面的说明)**
  * 如果你使用的是**原版**的 **simulation-distance** 默认值 10，你可以在 **3~8** 之间调整这个数字，而不必遵循上述规则。
    (如果你降低了 [bukkit.yml 的生物上限](https://eternity.community/index.php/paper-optimization/#perceived-mob-density)，则可以调整得更低。)
  * 例如，如果您的 **mob-spawn-range** 高于 **simulation-distance**，那么生物的感知密度就会降低，因为怪物会试图在你的**模拟距离**之外生成。 设置正确的阈值是至关重要的。
  * 例如，如果您的 **simulation-distance** 为 **6** ，则您的 **simulation-distance** 可以设置在 **3~5** 之间。
  * 从技术上讲，**3** 不是最小值，但由于在玩家周围 **24 个方块**内不会有生物生成，所以低于 **3** 是毫无意义的，因为它大大减少了可产生的区域，在极端情况下可能无法达到生物上限。
* [请参阅生物生成的工作原理部分以阅读完整的详细信息](https://eternity.community/index.php/paper-optimization/#mobspawn)。

---

```
nerf-spawner-mobs: false
```

> 此设置会移除游戏中的刷怪笼生成的生物 AI，如果您的服务器允许玩家重新放置刷怪笼，则将此选项设置为 true 可以减少延迟。

* 如果您决定启用此功能，请在 **paper.yml** 中将 **spawner-nerfed-mobs-should-jump** 设置为 true。这将允许生物进行跳跃，可以使某些农场会功能性的提醒人们。
* 请阅读[要避免的事情部分](https://eternity.community/index.php/paper-optimization/#Things-to-Avoid)，了解为什么精准采集刷怪笼不是一个好主意。

---

```
max-entity-collisions: 8
```

> 该值是实体碰撞检查中应包含的最大数量。在这个阈值之后，服务器将停止处理任何额外的实体碰撞。

* 降低此值将有助于提高性能，因为动物 AI 会在狭窄的狭小空间内试图寻找远离彼此的路径。
* **不要将该值设置为低于 3**，因为它会对依赖碰撞才能正常工作的事物产生破坏性影响。
* 这与游戏规则 **maxEntityCramming** 不能混为一谈。
  
  * **maxEntityCramming** 游戏规则规定了在实体开始受到窒息伤害之前可以挤在一起的最大数量。
  * 要降低此值，请使用 **/gamerule maxEntityCramming [值]**（默认为 **24** ）
  * 这个游戏规则对**单方块空间动物农场**有直接影响，因为一个方块空间内可以容纳的最大动物数量由这个值决定。

---

```
entity-tracking-range:
  players: 48
  animals: 48
  monsters: 48
  misc: 32
  other: 64
```

这决定了一个实体在多远的方块内应被追踪并被发送到客户端，以便玩家看到它们。

* Paper提供了这些选项，让你决定应该追踪多远的实体（显示给客户端） 而不是跟踪直到边界区块的所有内容（**simulation-distance**  - 1）。这是对抗大型实体的优化解决方案。
  * 您可以在 **Chunk provider tick** 下列出的 timings 报告中找到相关的性能优点。
    * **tracker stage 1** 是跟踪实体。
    * **tracker stage 2** 是广播实体跟踪变化。
      （如果 Chunk provider tick 占用大量资源，您应该首先尝试减少总实体的数量，然后降低模拟距离，如果所有其他方法都失败，则减少跟踪范围作为最后一个选项）
* 对于小型生存服务器或硬件过于强大的服务器，您可以在上述类别中增加这些配置，以通过隐含的性能权衡来增强玩家的游戏体验。
  * **player** 由 **玩家** 组成。
  * **monster** 由 **怪物**、**村庄袭击者**，和**飞行怪物** 组成。
  * **animal** 由 **村民**、**水生动物**，和 **普通动物** 组成。
  * **misc** 由 **物品展示框** 、**画**、**掉落物**、**经验球**  组成。
  * **other** 由上述未列出实体组成 (例如 **盔甲架** ).
* 该值以**方块**为单位，应始终设置为最大 **(simulation-distance -1)x16** 最小 **1**。
  （彩蛋：将值设置为 **0** ，所有实体都将被隐藏；将值设置为 **-1** ，所有实体本质上都将像忍者一样！请不要这样做）
* 如果您决定对此部分进行更改（不包括**players**），请同时匹配下面相应 **entity-activation-range** 类别中的值。
* 如果您经历过隐形恶魂攻击伤害，这可能是你对 **monster** 的 **entity-tracking-range** 设置得太低的问题。
  * 此外，检查客户端设置 选项 > 视频设置 > 实体渲染距离，并确保它设置得足够高以显示恶魂。

---

```
entity-activation-range:
  animals: 32
  monsters: 32
  raiders: 48
  misc: 16
  water: 16
  villagers: 32
  flying-monsters: 32
  villagers-work-immunity-after: 100
  villagers-work-immunity-for: 20
  villagers-active-for-panic: true
  tick-inactive-villagers: true
  wake-up-inactive:
    animals-max-per-tick: 4
    animals-every: 1200
    animals-for: 100
    monsters-max-per-tick: 8
    monsters-every: 400
    monsters-for: 100
    villagers-max-per-tick: 4
    villagers-every: 600
    villagers-for: 100
    flying-monsters-max-per-tick: 8
    flying-monsters-every: 200
    flying-monsters-for: 100
```

**entity-activation-range** 确定应该激活实体的范围（以方块为单位）。

任何落在此区域之外的实体都将以低频率进行 tick。

* 如果您在服务器上应用极低的 **simulation-distance/view-distance**，则这些值应始终设置为最大（**simulation-distance** - 1）x16 并且不小于 16。
* **更改激活范围会影响游戏的方方面面，因此请谨慎行事。**
  * **-1** 将禁用此行为并将行为恢复为原版； 但是，这会对您的服务器造成巨大的性能影响。 这样做需要您自担风险，并且仅在绝对必要时才这样做。
* 如果您对前一部分的 **entity-tracking-range** 进行了更改，请同时增加 **entity-activation-range** 上的相应类别，以便玩家实际上不会看到冻结的实体。
* **减少实体激活范围是最后的手段**
  * 您应该首先降低整体实体数量和模拟距离/视图距离。 仅当所有其他措施都被证明不足以提高性能时才更改此配置。
* 如果您不希望 tick 所有已加载但超出激活范围的村民，则可以将 **tick-inactive-villager** 更改为 **false**。 （也就是说，只有处于激活范围内的村民才会激活。
  * 但是，如果玩家不在附近，这样做会降低刷铁塔的产量。
  * 如果附近没有玩家，村民交易的冷却时间不会减少。
* **如果您遇到矿车不在轨道上时传输率不稳定**，请将 misc 更改为 -1 以解决问题。 **entity-activation-range** 在此用例中可能会产生负面影响并破坏红石装置。

![点击放大](https://pic.whksoft.cn/2022/04/06/b60b40ec7754e.png)

**work-immunity** 和 **wake-up-inactive** 由 Paper 实现的，其目标是通过允许某些实体“醒来”并在设定的时间内做一些工作来为服务器带来持久性。它允许村民补货、获得职业等……

* 如果您不想要这种行为，请将 **max-per-tick** 更改为 0。
* [单击此处在 Paper Github 上阅读 EAR Commit。](https://github.com/PaperMC/Paper/commit/e0ea2e0e147875b5b0eeab747d19fa00a5e19ef2)

> 为了更详细地说明，我们将使用这个 wake-up-inactive 的片段作为示例，以帮助您更好地了解该机制。

```
wake-up-inactive:
  animals-max-per-tick: 4
  animals-every: 1200
  animals-for: 100
```

上述设置转化为以下行为......

对于每 1200 tick，最多有随机 4 个的动物有机会唤醒 100 tick，它们能够做事并免受离玩家太远的冻结影响。

---

```
merge-radius:
  item: 2.5
  exp: 3.0
```

Paper 会更积极地合并掉落物和经验球，以减少地面上有大量物品对性能的影响。

* 该值以**方块**为单位，根据你的需要进行相应调整。
* 如果数值为 **-1**，则会被禁用。请注意，这会降低性能。
* 另外，如果你仍然希望合并**经验球**，但有一个设定的最大值，你可以在 **paper.yml** 中调整**experience-merge-max-value** 。
* 如果您希望恢复**末影龙被击败时的经验雨**，或者只是喜欢在坐在农场上时看到一堆经验球向您走来，你可以禁用这个选项，这意味着性能上的折中。
* 如果你遇到了经验球在合并与组合时没有流向玩家的问题，可以像上面提到的那样执行一个最大的合并值，或者禁用它来补救这个问题。

---

# **paper.yml**

### paper.yml 的基本配置

```
despawn-ranges:
  monster:
    soft: 32
    hard: 128
  creature:
    soft: 32
    hard: 128
  ambient:
    soft: 32
    hard: 128
  axolotls:
    soft: 32
    hard: 128
  underground_water_creature:
    soft: 32
    hard: 128
  water_creature:
    soft: 32
    hard: 128
  water_ambient:
    soft: 32
    hard: 64
  misc:
    soft: 32
    hard: 128
```

这个值决定了一个生物会概率性消失（软清除）或立即消失（硬清除）的距离。

它分为 8 个类别，为所有类型的实体消失提供更精细的控制选项。

* 请参阅上一节中 [关于 bukkit.yml 中的 mob-limits 配置项](https://eternity.community/index.php/paper-optimization/#mob-categories) 以查看每个实体属于什么类别。
* 如果您将 **simulation-distance** 保持为默认值，则降低 hard 值将增加生物的感知密度，但需要在服务器上执行额外的 清除/生成 操作。 （见下面的注释）

如果您的 **simulation-distance** 设置为低于 10…
（如果您正在使用默认的 **simulation-distance: 10**，请忽略下一部分）

* **despawn-range** 中 **hard** 的值应该是 **(simulation-distance -1)x16** 方块。
  * 这确保了所有实体在碰到边界块之前都有机会消失，从而保持自然的生物密度。
    
    * 更改此配置将影响农场上的**默认挂机点**，因为范围限制是在水平和垂直方向上强制执行的。需要相应地调整农场设计。
  * 如果您确实选择应用默认值 128 以在运行较低的 **simulation-distance** 和/或 **mob-spawn-range** 时保持对原版的理想挂机点持久性，您将体验到奇怪的生物密度、极其不均匀的生物产卵和/或超过生成限制的过量生物。
    
    * 为了帮助虚拟出保持 **hard despawn-range** 高于 **ticking view-distance** 的副作用，请阅读以下场景......
      
      一个名叫 **BumbleTree** 的玩家正在他的 Minecraft 基地里放松。 他在他的床边做了一个简单的怪物磨床，因为他喜欢在僵尸咆哮的声音中入眠，所以他通常在研磨机内保持几十个僵尸的生命，直到他需要经验。
      
      在一个星期六的晚上，**BumbleTree** 决定去拜访他的 Minecraft 女友 **Naomi**，所以他步行离开他的基地。当他离开他的基地时，***他磨床内的所有僵尸都被卸载了。*** 当他不在的时候，一个名叫 **Jerry** 的玩家碰巧经过他的基地，并决定从箱子里偷一些小麦。杰瑞也注意到了一些奇怪的事情！ 现在是晚上，但他周围几乎没有任何生物生成！？
      
      ***这是因为已经到达了生物上限(mobcap)，而僵尸仍在磨床。***  僵尸从来没有机会正确地消失，所以当新玩家访问该区域并加载区块时，由于 mobcap 已经饱和，因此不会产生新的生物，从而产生没有生物实际产生的错觉。 这就是为什么确保 **hard despawn-range** 与上述建议值一致至关重要的原因。它确保所有生物都被正确地消失和重新分配。
      
      **现在您了解了 hard despawn-range 的作用。 那么请明智的选择属于你自己的数值吧！**
* **hard despawn-range** 也应该始终**等于** spigot.yml 中的 **mob-spawn-range** **且不能更低**。
* 请仔细阅读 **[了解生物生成](https://eternity.community/index.php/paper-optimization/#mobspawn)** 部分以验证相关配置。

---

```
per-player-mob-spawns: true
```

Paper 会尝试在所有在线玩家中更均匀地生成生物。

* 确保此选项设置为 **true**。这对大多数服务器都是有益的。
* 启用后，**全服生物上限将根据在线玩家数量进行调整。**
  要访问生物上限 (mobcaps) 的详细分类，请使用以下指令...
  * **/paper mobcaps** 用于全局上限和总可生成区块
  * **/paper playermobcaps** 用于玩家生物上限
    （这对于排除生物农场的故障特别有用，如果你站在你的挂机位置并且当上限已经满时农场内没有生成任何东西，这意味着你没有通过 spawn-proofing）
* 可生成区块基于**已加载区块的数量**和 **bukkit.yml** （或 **paper.yml** ）中定义的 **spawn-limits**。请在必要时进行必要的调整，特别是如果您之前没有启用它。
  在没有 **per-player-mob-spawns** 的多人服务器中，原版生物生成不仅复杂而且存在固有缺陷。
  * 例如，我们在下界维度中有两个玩家——玩家 A 在下界基岩层猪灵塔的高处挂机，而玩家 B 只是在下界荒地生物群系中放松。
  * 即使尝试生成动作是围绕两个玩家周围的所有**已加载区块**进行的；大多数成功的生成生物的尝试都将在具有最有利生成条件的玩家周围结束。在这种情况下，玩家 B 将获得**大部分生物**，而玩家 A 只获得一点点，因为玩家 B 周围有更多可生成的区块。有关更多详细信息，请参阅前面的[生物生成方式部分](https://eternity.community/index.php/paper-optimization/#mobspawn)。
* 如果此选项设置为 **false**，服务器将回退到 Mojang 原版的每个玩家的生物生成实现。

---

```
prevent-moving-into-unloaded-chunks: true
```

> **请将此选项设为 true**
> 
> 这可以防止玩家移动到未加载的区块中，否则会导致同步区块加载。

* 当玩家进入未加载的区块时，服务器会将加载区块的任务的优先级设为最高，从而降低 TPS。

---

```
use-faster-eigencraft-redstone: true
```

**请将此选项设为 true**
此选项使用了优化更好的红石逻辑，对现有红石机器的影响最小。

---

```
enable-treasure-maps: true
treasure-maps-return-already-discovered: false
```

> 这可以防止服务器在玩家使用地图时停止查找宝藏
> 然而，Mojang 自 1.18 起将该问题标记为已解决，现在使用藏宝图应该被认为是安全的。

* 在此处查看 Mojang 漏洞追踪器 [MC-236740](https://bugs.mojang.com/browse/MC-236740).
* 如果您想更加安全，可以将上述参数之一设置为 true/false，它将禁用藏宝图的功能，从而防止与它们相关的任何可能的问题。

---

```
fix-climbing-bypassing-cramming-rule: true
```

> **将 fix-climbing-bypassing-cramming-rule 切换为 true**
> 
> 此项针对攀爬的生物的攀爬规则进行修改，在攀爬时也会受到实体挤压。
> 如果您有使用此行为的农场（非常不可能），请将其保留为 **false**。

---

```
keep-spawn-loaded: true
keep-spawn-loaded-range: 10
```

> 如果您的世界出生点不经常使用，您可以将此选项切换为 **false**。
> 
> 设置后，服务器不会加载世界出生点，从而节省资源。

* 此选项可以[按世界定义](https://eternity.community/index.php/paper-optimization/#Per-world)，如果您添加了未大量使用的其他世界，这是一个很好的资源节省措施。
* 范围值以区块为单位，要**加载的总区块**的公式是 [((keep-spawn-loaded-range+1)x2)+1]^2 = 加载的总出生点区块。
  
  * 令 **keep-spawn-loaded-range** 等于 **10** ， **[(10+1)x2+1]^2=529** ，您将不断加载 529 个区块。

---

```
entity-per-chunk-save-limit:
  experience_orb: 50
  snowball: 20
  ender_pearl: 20
  arrow: 20
  fireball: 10
  small_fireball: 10
  dragon_fireball: 5
  egg: 20
  area_effect_cloud: 10
  llama_spit: 5
  shulker_bullet: 8
  spectral_arrow: 5
  potion: 5
  experience_bottle: 5
  trident: 10
  wither_skull: 10
```

> 这决定了每个区块指定实体保存的最大数量。

* 如果您正在运行极低的**模拟距离**，则应用它们以及上面列出的其他附加功能尤其重要
* 该限制是必不可少的，因为它可以防止服务器在尝试加载包含大量这些实体的区块时卡住。
  （有时投掷物被玩家故意发射到未加载的区块中以使服务器崩溃，或者由于纯粹的运气而无意中达成。这两种情况都可以通过设置这些限制来避免）
  
  [特别感谢 Puremin0rez 的条目更正](https://github.com/Puremin0rez) 爱你奥 <3 2021.8.21

---

```
alt-item-despawn-rate:
  enabled: true
  items:
    COBBLESTONE: 600
    COBBLED_DEEPSLATE: 900
    NETHERRACK: 600
    ROTTEN_FLESH: 600
    ENDER_PEARL: 600
    BONE: 600
    CACTUS: 600
    EGG: 600
```

启用此选项将允许您更快地消除你通常丢弃的废块/垃圾物品

`仙人掌 (CACTUS) 被添加到列表中是为了减少普通仙人掌农场的影响，由于农场设计，许多仙人掌将留在地面静等消失`

`此处还列出了 鸡蛋 (EGG)，以防止僵尸捡起它们，从而防止它们消失。 这是生存服务器中的一个常见问题，其中玩家可能会在很长一段时间内处于挂机状态，并导致成群的僵尸都持有玩家基地周围常见的掉落物品。`

* 该值以 **游戏刻 (tick)** 为单位， **20** tick 等于 **1** 秒。
* 您可以在列表中添加更多满足您需求的项，因为每个服务器都有自己独特的情况。
  * 可以在 [Material 枚举类列表](https://papermc.io/javadocs/paper/1.18/org/bukkit/Material.html) 上找到完整的物品列表。
* 此外，还有一个类似的配置 **item-despawn-rate** 来控制 spigot.yml 中所有丢弃的物品消失计时； 然而，这是一个非常具有侵入性的更改，它打破了大多数 Minecraft 玩家熟悉的 5 分钟消除时间规定，因此这里省略了它。

优化服务器的目标是让您的玩家更享受游戏而不是游戏享受他们！

---

```
tick-rates:
  sensor:
    villager:
      secondarypoisensor: 40
  behavior:
    villager:
      validatenearbypoi: -1
```

> 设置实体的 sensor tick rate（在本例中为村民）

* 较高的值将以实体行为度为代价减少实体的资源使用。
* 如果想应用原版默认的话，请将数值调整为 **-1**。
* 此配置适用于所有类型的实体，请查阅实体名称的timings，并根据需要为它们手动创建条目。
* 如果您的服务器遇到村民相关的性能问题，请先尝试调整配置，然后再使用[插件解决方案](https://eternity.community/index.php/paper-optimization/#Quality-of-Life)。
  * 例如，您可以将 **secondarypoisensor** 调整为 240 并将 **validatenearbypoi** 调整为 120 以节省资源。 负面的行为变化可能并不明显。

---

```
grass-spread-tick-rate: 1
```

> 调高这个数值会使泥土变成草的速度变慢。

* 如果您周围有很多动物，则将此值增加太多会导致草地上出现许多丑陋的泥土块。

---

```
optimize-explosions: false
```

> 将此设置为 **true** 将减少计算大量爆炸的影响。

* **在实际的应用中通过爆炸能从中受益的场景微乎其微**；然而，如果你的熊孩子朋友们喜欢用大量的 TNT 炸毁东西，这种设置可能适合你。
* 如果你真的很喜欢炸毁东西并注意到你的 TNT 爆炸不像普通的那样，增加 **spigot.yml** 中的 **max-tnt-per-tick** 阈值来解决这个问题。
  * 请注意，该值是为了保护您的服务器，增加数值会增加您的服务器在大量 TNT 引爆时崩溃的风险。

---

```
armor-stands-tick: true
```

> 这决定了是否应该 tick 盔甲架。

* 将此设置为 **false** 将会...
  * 完全移除任何与盔甲架相关的卡服机器。
  * 它将破坏使用盔甲架的插件。
  * 它将破坏自动制冰机等农场 YT: HosZhk_gJ5c。

---

## **请确保您的 Paper 高于 build #202 更新，因为以下配置现已在 1.18 版本上实装。**

```
chunk-loading:
  min-load-radius: 2
  max-concurrent-sends: 2
  autoconfig-send-distance: true
  target-player-chunk-send-rate: 100.0
  global-max-chunk-send-rate: -1.0
  enable-frustum-priority: false
  global-max-chunk-load-rate: -1.0
  player-max-concurrent-loads: 20.0
  global-max-concurrent-loads: 500.0
```

> 上面列出的默认值应该适用于大多数服务器。

* **如果您正在解决区块加载问题，请先在 DC 咨询#paper-help 频道以获取建议，不要在未完全了解其作用的情况下更改数值。** 不要遵循任何随便一个指南来获取推荐值，有很多鱼龙混杂的信息！
* 如果您刚刚从 Tuinity 迁移到 Paper，请先使用默认配置运行服务器，然后再更改任何设置。 之前的 Tuinity.yml 中的某些配置选项不再工作和/或在 Paper 上工作不同。
* 对于具有高并发玩家数量的服务器，如果在高峰时段加载区块的速度明显变慢，请尝试逐渐增加 **max-concurrent-sends** 和 **global-max-concurrent-loads** 以解决问题。
  * 如果您启用了 anti-xray（反矿透）并采用了engine-mode: 2（假矿模式） ，可能会导致区块加载问题。
  * 如果你有 protocollib 或任何可能阻止 netty 线程的东西，它也可能会停止区块加载。

[**附加说明在官方 PaperMC 文档中列出。 点击此处查看详情**](https://paper.readthedocs.io/en/latest/server/configuration.html#chunk-loading)

---

## **附加选项和改善生存质量的配置**

这些附加配置由 Paper 提供，强烈推荐，因为它们增强了整体玩家体验。

```
lootables:
  auto-replenish: true
  restrict-player-reloot: true
  reset-seed-on-fill: true
  max-refills: -1
  refresh-min: 12h
  refresh-max: 2d
```

> 如果您计划运行长期生存服务器，请将其切换为 **true**。
> 
> 战利品箱一旦被洗劫一空，它就会补充宝箱，并为您的旧世界带来一些生机。

* **战利品补充**功能仅适用于原始世界生成的箱子。
  * 请让您的玩家知道并了解不要破坏它们。
* **restrict-player-reloot** 将阻止同一玩家多次驻扎并搬空宝箱。
  * 场景 1：如果**玩家**掠夺了一次宝箱，然后再回来，则不会有新的战利品。
  * 场景 2：如果**玩家 A** 掠夺了一个宝箱，后来**玩家 B** 又来了，这个箱子将被重新填满，玩家 A 可以再次掠夺。
* **reset-seed-on-fill** 将在每次重新填充时重置战利品表种子，因此箱子内的物品总是不同的。
* **max-refill** 表示最大补充次数，设置为 **-1** 将使其无限制。
* **refresh-min/max** 定义了重新填充箱子之前需要冷却的时间。
  * 此操作不需要加载区块。
* 这里的度量单位是 **m** 代表**分钟**，**h** 代表**小时**，**d** 代表**天**。 **m 不代表月！！**
* **无论里面的剩余物品如何，除非箱子是满的，否则箱子将重新填充。** .
  * 场景1：如果 **玩家 A** 打开箱子（生成了战利品）并将战利品留在里面，那么后来过来的 **玩家 B** 将获得一组新生成的战利品以及 **玩家 A** 之前留下的任何东西。
  * 场景 2：如果**玩家 A** 打开箱子并用闪长岩填满，那么后过来的**玩家 B** 将不会得到任何新的东西（除了闪长岩），因为箱子已经满了，因此不会生成新的战利品。

---

```
generate-random-seeds-for-all: true
```

> 将此项切换为 **true** 将使[种子破解工具](https://github.com/KaptainWutax/SeedCracker)更难确定您的世界种子。
> 
> 如果您的目标是迁移玩家在设法找出您的世界种子时可能拥有的不公平优势，这是一个非常有用的功能。

* 请注意，此配置仅涵盖**此功能**，对于**生成的结构**，请手动更改 **spigot.yml** 中的种子
* **spigot.yml** 中提供了除 **Mineshafts** （废弃矿井）之外的所有结构种子。
* 请注意，这不会影响已经生成一次的区块。如果您想充分利用此功能，它将在全新的世界中发挥最佳作用。

**要正确启用此功能，请按照以下步骤操作……**

1. 启动服务器以生成 **paper.yml** 文件，然后**停止服务器**。
2. 打开 **paper.yml** 并将 **generate-random-seeds-for-all** 设置为 **true**。
3. 打开 **spigot.yml** 并根据自己的喜好手动输入结构种子。
4. 启动服务器，然后你就可以开始了！
5. 奖励：如果您想定义单个 **feature-seeds** （特征种子），您可以返回到 **paper.yml** 并手动设置它们。6. 请记住在正确生成生成区域之后删除世界
   （或者，在第 3 步之后将 **keep-spawn-loaded** 设置为 **false**。）

* 要手动定义世界种子，请将 **level-seed=[种子代码]** 添加到 **server.properties** 或使用 [NBT 编辑器](https://irath96.github.io/webNBT)编辑您的 **level.dat**，然后执行上述步骤。
* **[以下是 generate-random-seeds-for-all 的所有可用选项的列表，请单击此处。](https://eternity.community/index.php/all-feature-seeds-options)**
* 请注意，这可能无法阻止种子破解器残酷地扒出您的世界种子；但是，此选项确实使了解世界种子的优势变得毫无用处，因为所有要素和结构的位置都不会与世界种子对齐。然而，完全防止种子破解的唯一真正方法是自定义世界生成。

启用该功能后，您将在 **paper.yml** 中的 **world **、 **world_nether** 和 **world_the_end** 下找到所有 **feature-seeds**，如果您有任何其他特定于每个世界的配置，请务必将它们添加到相应的类别下。可以在此处找到有关[每个世界配置的详细信息](https://eternity.community/index.php/paper-optimization/#Per-world)。

---

```
log-player-ip-addresses: true
```

> Paper 提供了切换玩家 IP 地址记录的选项。
> 对于那些想要保护玩家隐私的人，您可以将此选项切换为 **false**。

* 这在需要与第三方审计员、维护人员或开发人员共享日志文件以进行一般故障排除时特别有用。

---

```
book-size:
  page-max: 2560
  total-multiplier: 0.98
```

> Paper 提供了调整成书整体大小的选项。 这是一个有用的功能，可以防止故意或意外的 bookban （不太可能）。

* **page-max** 的值以字节为单位。 可以根据自己的喜好将它**减少一半或更多**（**即 640~1280**）
* [点击这里，进入官方文档页面。](https://paper.readthedocs.io/en/latest/server/configuration.html#book-size)
* 此外，您可以使用插件来禁止书籍中的非 ASCII 字符（国内环境不可能禁用），这将在[后面的部分](https://eternity.community/index.php/paper-optimization/#Quality-of-Life)中列出。

---

```
monster-spawn-max-light-level: -1
```

> Paper 提供了一个可选配置来调整生成生物所需的光照水平。

* 值为 **-1** 时遵循原版默认值。
  * 在 1.18 中，生物只能在亮度为 **0** 时生成。
  * 在 1.17 或更早的版本中，生物可以在最高 **7** 的亮度等级下生成。
* 该值可以是从 **0** 到 **15** 的任何整数。

---

## **每个世界的生成限制和其他世界的配置**

> Paper 允许您对每个世界（例如在下界、末地或其他自定义世界中）强制执行自定义生物上限，您可以在 **world-settings** 下创建一个新对象。
> 
> 此外，paper.yml 和 spigot.yml 中 **world-settings** 类别下的所有配置选项都可以单独定义为每个世界进行配置。
> 请查看下面的格式化示例。

```
world-settings:
  default:
    spawn-limits:
      monster: 70
      creature: 10
      ambient: 15
      axolotls: 5
      underground_water_creature: 5
      water_creature: 5 
      water_ambient: 20
  world_nether:
    spawn-limits:
      monster: 80
      creature: -1
      ambient: -1
      axolotls: -1
      underground_water_creature: -1
      water_creature: -1
      water_ambient: -1
  resource_world:
    spawn-limits:
      monster: 5
      creature: 30
      ambient: -1
      axolotls: 10
      underground_water_creature: -1
      water_creature: -1
      water_ambient: -1
    keep-spawn-loaded: false
 
# 本节作为示例来帮助您虚拟化结构。
# 其他配置选项在此省略。
```

> 在上面的例子中，通过在 **resource_world** 中设置一个较低的怪物值并让 **keep-spawn-loaded: false** ，你只是让资源世界对矿工来说更安全一些，并且通过不保持出生点区块加载来减少服务器上的过载，因为它没有被积极使用。

> 通过像这样巧妙地使用每个世界的配置以及[前面提到的覆盖示例](https://https://eternity.community/index.php/paper-optimization/#server.properties)，您将能够在服务器上节省一些资源，同时为您的玩家保持最佳的游戏体验。

* Paper 在这里使用正确的类别名称，而不是旧 bukkit 名称。
  * **creature** 与 bukkit.yml 中的 **animals** 相同
  * **water_creature** 与 bukkit.yml 中的 **water-animals** 相同
* **default** 是适用于每个世界的选项，除非世界类别中另有说明。
  * **world** 代表主世界
  * **world_nether** 代表下界维度
  * **world_the_end** 代表末地维度
  * 此外，如果您有其他自定义世界，也可以在 **world-settings** 下添加。
* 当值为 **-1** 时表示它将遵循默认定义的值。 （在 **spawn-limits** 制约的情况下，由 bukkit.yml 中的 mob-cap 决定）
* **YML 文件使用 2 个空格作为缩进。** 千万不要使用 tab 键缩进！
* 请确保您的缩进属于正常的树状结构。 （每个缩进需要步进两个空格）
* [适当的 yml 编辑器将有助于确保格式不被破坏。](https://eternity.community/index.php/paper-optimization/#Prerequisites)
* 如果出现错误，控制台将在启动时出现错误。请对其相应地进行纠正。

---

## **Paper 内置的反矿透功能**

> Paper 内置了反矿透 (Anti-xray) 功能，如果您选择启用它，请仔细阅读以下部分。
> 
> [推荐阅读 stonar96 的 Paper 反矿透设置](https://gist.github.com/stonar96/ba18568bd91e5afd590e8038d14e245e)

> 下面是一个关于如何在下界和其他世界中启用 Anti-Xray 的示例。

```
world-settings:
# 直接丢在 default: 之前就行
  world_nether:
    anti-xray:
      max-chunk-section-index: 7
      max-block-height: 128
      hidden-blocks:
      # 见上面关于空气的注释
      - ancient_debris
      - bone_block
      - glowstone
      - magma_block
      - nether_bricks
      - nether_gold_ore
      - nether_quartz_ore
      - polished_blackstone_bricks
      replacement-blocks:
      - basalt
      - blackstone
      - gravel
      - netherrack
      - soul_sand
      - soul_soil
  # world_the_end: 如果你的世界名称不同，则需要更改。
  world_the_end:
    anti-xray:
      enabled: false

  # 直接粘贴到 default: 之前
  default:
  # 其他配置在这里省略 
  # 这样做是为了展示缩进的样子
```

* 请仔细检查以确保缩进是正确的，**world_nether** 和默认世界应该有相同的缩进量（距离 **world-settings** 2 个空格）
* 请务必重启服务器以应用新配置。 不要使用 /reload
* 如果你完全不知道如何添加它，[这里有一个带有默认设置的 paper.yml 文件供你下载。](https://drive.google.com/file/d/1C4YXXSXSVu0RGeSkPibA39YEOHMLxsHK/view)
  （通常情况下，我强烈反对从互联网上随便下载来路不明的文件，但这里是……）
  * 如果您之前更改过一些内容，请替换现有的 paper.yml 并更改一些必要的配置。
* 请注意，**engine-mode: 2 在大多数服务器上都能正常工作**； 但是，在具有高并发玩家数量（100+）的服务器上，engine-mode: 2 的使用有时可能会使网络通道饱和，因为正在向玩家发送更多数据。 如果您认为这种情况正在发生，请咨询您的网络管理员并相应地解决问题。

---

# **附录**

> 我遗漏了很多东西，并不是没有原因的。 那些其他设置通常会在大多数中小型服务器上引入太多的游戏玩法妥协或最低限度的性能改进。 有关可用配置的完整列表，[您可以查看 PaperDocs。](https://paper.readthedocs.io/en/latest/server/configuration.html)

> **现如今 Minecraft 对资源的要求很高，只有这么多你可以优化，直到你扪心自问，这还值得吗？**

你只能从汽车中取出这么多东西来让它跑得更快，直到你只剩下一个方向盘和一个光秃秃的底盘，这同样适用于优化你的服务器。

> 如果调整上述配置不足以帮助在您的服务器上保持稳定的 TPS (18~20)，我强烈建议考虑直接硬件升级，因为这将是查看任何进一步性能改进的最直接方法。[请参阅下面的服务器托管部分](https://eternity.community/index.php/paper-optimization/#Hosting-Options)。
> 
> 在撰写本文时，即使有可用的最新硬件，原版默认配置可容纳 60-80 名玩家或优化后容纳 100 名在玩法上妥协的玩家是硬上限。 对服务器施加一个合理的最大玩家限制，并开始考虑网络扩展（多服务器），并祝贺你的成就！

---

# **Timings 工具**

> Paper 内置了工具来帮助解决性能问题

* 在游戏内或控制台（不带 /）中输入 **/timings report** 以生成报告。
  * 您将获得一个看起来像这样的链接……
    [https://timings.aikar.co/?id=14a142949f42468f81bdb49613a2abdd](https://timings.aikar.co/?id=14a142949f42468f81bdb49613a2abdd)
  * 此报告包含解决问题所需的所有信息。
  * 如果您打算在 Discord 上寻求帮助，请务必准备好报告。
  * Aikar 制作了一个视频在且涵盖了所有内容 YT: v=T4J0A9l7bfQ
* 要生成有意义的 timing 报告，您应该……
  * 避免在服务器启动后立即生成 timings 报告，因为数据会出现偏差。
  * 允许记录更长时间的 timings 数据
    （虽然 paper 默认为最短 3 分钟，但是这里建议为了数据准确，最短为 10 分钟，但越长越好）
  * 生成报告的理想时间是在玩家活动高峰期间或服务器卡顿时。

> 红色的大数并不总是意味着您的服务器卡顿

* Timings 页面有 **Lag view** 和 **All view** 选项卡。 总体而言，几千个 tick 的几个红色卡顿 tick 是非常可以接受的。
* ***如果您在理解 Timings 报告时遇到困难，请前往 Paper Discord 寻求更多支持。***

---

# **常见误区**

* **GHz 神话**
  
  * 为您的 Minecraft 服务器选择 CPU 时，请勿使用时钟速度等级来比较两个 CPU，除非它们的型号和制造商相同。 更多信息请参考 [Gigahertz Myth](https://en.wikipedia.org/wiki/Megahertz_myth)。
  * 简而言之，选择**最新的 CPU 架构和可用的最高单线程单核**模型。
* **分配更多的 RAM 不等于更好的性能**
  
  * 服务器性能很大程度上取决于您的 CPU 而不是 RAM。
  * 无论玩家/插件数量如何，大多数服务器都可以在分配 10GB 的情况下正常运行。
  * 为每个玩家分配 1GB  内存的言论是错误的。
  * 话虽如此，未使用的 RAM 是浪费的 RAM； 但是，任何超过 10GB 的空间为您提供的好处微乎其微。
  * 任何声称更多 RAM 会提高您的服务器性能（在 10 GB 之后）的服务商都试图向您追加销售以获取自己的利润。不要落入这个陷阱。
* **RAM 使用情况并不表示性能问题。** 除非您遇到 OOM（内存不足）或内存泄漏。
  
  * 在正确设置的 JVM 上，从 性能面板/htop 读取的 RAM 使用情况非常没有意义。 我们将在下面的[运行参数部分](https://eternity.community/index.php/paper-optimization117/#JVM-Flags)对此进行介绍。
  * 如果您希望 JVM 使用一定数量的 RAM，请正确设置您的 **Xmx 参数**
* **高内存使用不等于内存泄漏**
  
  * 这可能是内存泄漏的症状，但在大多数情况下并非如此。
  * 在怀疑期间，当您认为使用 **/paper heap dump** 命令发生泄漏时生成堆报告，然后通过 Eclipse Memory Analyzer 或类似工具提供报告以解决问题。 您也可以在 Paper Discord 中寻求帮助。
* **TPS 不是性能的准确衡量标准**
  
  * 相反，您应该注意 **MSPT** (**m**illi**s**econds **p**er **t**ick). Minecraft 以每秒 20 tick 的固定速率运行，因此只要您的 MSPT 低于 50，您将保持 20 TPS。
  * 服务器可能显示平均 20 TPS，但由于 TPS 损失的百分比很高，玩家在这种情况下仍可能会遇到延迟。
* **Minecraft 服务器的最低推荐线程/核心数为 4。**
  
  * 虽然主游戏确实在 1 个线程上循环完成，但有许多任务可以从多个线程中受益，例如 Netty、插件、SQL 数据库等。
  * 建议大多数服务器至少有 4 个线程/核心。 如果您正在[购买主机](https://eternity.community/index.php/paper-optimization/#Hosting-Options)，请考虑到这一点并相应地选择您的计划。

---

# **JVM 参数 (必须要有的东西)**

* 为 Minecraft 应用程序使用**优化的 JVM 参数**。请参考 [Aikar 的 JVM 调优指南](https://https://aikar.co/2018/07/02/tuning-the-jvm-g1gc-garbage-collector-flags-for-minecraft)以获取更多信息。
* 通过你可爱的 [Bluely](https://github.com/kadenscott) 制作的 [**starmc.sh**](https://startmc.sh/) 轻松获取上述参数。
* **确保 Xms=Xmx**。这允许 JVM 完全控制分配的 RAM，并且有利于性能。
  （我会亲自反对任何提出其他建议的无良服务商的言论。设置 xmx=xms 会阻碍服务商将尽可能多的服务器塞入物理机器的能力并降低其利润率。有关更多信息，请参阅下面的[托管建议](https://eternity.community/index.php/paper-optimization/#Hosting-Options)）
* **ZGC 或 Shenandoah 垃圾回收器怎么样** ?
  * 关于 ZGC，如果您想深入了解，Krusic 有一篇 **[很棒的博文](https://krusic22.com/2020/03/25/higher-performance-crafting-using-jdk11-and-zgc)**。
  * 我不知道手头的 Minecraft 应用程序对 Shenandoah 的任何有效分析，如果您确实有关于此主题的资源，请与我联系，以便我将其包含在此处。
  * ZGC、Shenandoah 和其他现代收集器都是为高线程环境设计的，这些环境不适用于 Minecraft 应用程序，因此通常不推荐使用它们。任何声称不这样做的人都应该保持怀疑态度，特别是如果他们没有任何数据支持他们的主张。
  * **服务器启动时间**和**闲置时 MSPT** 不是有用的指标，也不是更好的替代 GC 性能的指示。有大量有问题的声明将它们用作基准数据点。这是无能的表现，你不应该相信他们的说法。
* **预留足够的 RAM。** 无论您如何托管服务器，操作系统和 JVM 本身都需要 RAM 才能运行。请保留足够的 RAM 并按需降低 **Xmx**。

---

# **备份和恢复的最佳实践**

### 假如你喷屎了！你肯定不会想穿着一条破裤子被困在浴室里，对吗？~随身携带备用内裤很重要！~ 那么现在就来做个备份吧！

* **制定一个备份和恢复计划是至关重要的**
  * 意外的崩溃或不当的关机可能会导致世界的损坏或你的服务器的数据丢失。在本地和异地保留多个备份副本不仅是至关重要的，而且是运行一个持久的服务器的重要方面。
  * 定期测试您的备份副本。 未经测试的备份 = 没有备份。
  * 在可能的情况下，在操作系统层面进行备份（直接备份整个文件，避免使用 Minecraft 插件进行备份）。

### **在基于 Linux 的操作系统上**

#### 最简单的方法是手动停止服务器并创建一个 tarball。

```
tar -czvf ServerBackup_日期.tar.gz /[路径]/
```

例如，如果您想将备份文件保存到 /HOME/USER/MCbackup，您将这样输入指令。

**tar -czvf ServerBackup_May5.tar.gz /HOME/USER/MCbackup**

* 请务必定期将备份迁移到异地。 *同一硬盘驱动器中的备份属于重复而不是备份。*

#### 对于更高级的用户，您可以使用 rsync 设置 rsnapshot 以记录所有文件更改并自动化异地备份过程。

* 如果您对自动化备份过程感兴趣，请查看 [rsnapshot 的 Github 页面](https://github.com/rsnapshot/rsnapshot/blob/master/README.md)。

#### 此外，您可以使用 rsync.net 或 Backblaze 设置 Borg + Borgmatic

* [Borg 使用入门](https://borgbackup.readthedocs.io/en/stable/usage/general.html)
* [了解有关 Borgmatic 的更多信息](https://torsion.org/borgmatic)
* [rsync.net](https://rsync.net/)
* [Backblaze](https://www.backblaze.com/b2/cloud-storage.html)

### **在 Windows 操作系统上**

```
右键单击您的 Minecraft 根目录 > 发送到 > 压缩（zipped）文件夹
```

![image.png](https://whksoft.cn/usr/uploads/2022/04/1920931216.png)

* 请务必 tarball / zip 文件转移到外部站点作为镜像，以防[您的数据中心发生火灾](https://www.reuters.com/article/us-france-ovh-fire/millions-of-websites-offline-after-fire-at-french-cloud-services-firm-idUSKBN2B20NU)。
* 理论上，您可以在备份过程中使用 **save-off** 保持服务器运行。 请记住在备份任务完成后将其重新打开。

---

# **提升你生存质量的插件与工具**

这是我个人推荐的插件列表，它对每个服务器都有好处。

## **基本插件**

*免责声明：我不是由任何个人或团体赞助制作此列表的。 这份清单纯粹是出于对我与他们的亲身经历的热爱<3*

* [**Luckperm**](https://luckperms.net/)
  
  * 一个必备的权限插件，为您提供更精细的权限控制，而不会有将 OP 权限授予您的协管团队的风险。
* **[EssentialsX](https://essentialsx.net/) 系列插件**
  
  * EssentialsX 是一系列插件，可为您的服务器提供一些基本功能。
    （请只阅读并安装您需要的，而不是全部安装）
    ~~（有钱的自己买 CMI ，功能性相对完善，并且不知有ess功能，但是闭源，支持性不佳，不算是太推荐）~~
* **[EssentialsX-Discord](https://essentialsx.net/downloads.html)**
  
  * 一个轻量级的互联插件， ~可将您的 MC 小闹剧从游戏内聊天带到 Discord（国内谁用 DC 啊，又去用Minecraft_QQ了）~。
  * [安装指南](https://essentialsx.net/wiki/Discord-Tutorial.html)
* [**Chunky**](https://www.spigotmc.org/resources/chunky.81534)
  
  * 世界预生成插件。
* [**ChunkyBorder**](https://www.spigotmc.org/resources/chunkyborder.84278)
  
  * 管理世界边界的插件，比原版 worldborder 具有更多功能。
* **[PureTickets](https://www.spigotmc.org/resources/puretickets-ticket-system-with-discord-integration.71677)**
  
  * 用于管理服务器的反馈/技术支持的工单系统。
* **[EntityDetection](https://www.spigotmc.org/resources/entitydetection-tile-entity-support.20588)**
  
  * 一个简单的插件，用于在服务器上显示实体位置。
  * 定位有问题的实体和非法村民狂欢派对的有用工具。
* **[Vanish no packet](https://github.com/mbax/VanishNoPacket)**
  
  * 免费，开源的隐身插件。
* **[Openinv](https://dev.bukkit.org/projects/openinv)**
  
  * 允许您打开任何玩家的背包，即使他们当前不在线。
* **[MiniMOTD](https://www.spigotmc.org/resources/minimotd-server-list-motd-plugin-with-rgb-gradients.81254)**
  
  * 自定义 MOTD 和服务器图标。
* **[TabTPS](https://github.com/jpenilla/TabTPS)**
  
  * 用于在 tab 列表、bossbar 和 actionbar 中显示 TPS、MSPT 和其他信息，支持 RGB。
* **[AnnouncerPlus](https://github.com/jpenilla/AnnouncerPlus)**
  
  * 强大且完全可定制的公告插件。
* **[Wandering Trades](https://github.com/jpenilla/WanderingTrades)**
  
  * 添加自定义流浪商人战利品表的功能。
* **[AntiRaidFarm](https://www.spigotmc.org/resources/antiraidfarm-block-cheaty-infinite-raid-farms.83283)**
  
  * 一个简单的插件来禁用 Raid Farm （袭击农场）。
* **[Maintenance](https://www.spigotmc.org/resources/maintenance-bungee-and-spigot-support-1-8-1-17.40699)**
  
  * 启用/禁用维护模式。
* **[VanillaTweaks](https://github.com/MC-Machinations/VanillaTweaks)**
  
  * Vanillatweaks，但它是一个插件。
* **[TreeAssist](https://www.spigotmc.org/resources/treeassist.67436)**
  
  * 唯一不会让服务器宕机的砍树插件 ~~（UltimateTimber插件虽好，但是不适合大型生电服）~~。
* **[WorldEdit](https://dev.bukkit.org/projects/worldedit)**
  
  * 创 世 神
* **[WorldGuard](https://dev.bukkit.org/projects/worldguard)**
  
  * 世界保护用插件，防爆防熊各种防。
* **[Farm Control](https://github.com/froobynooby/FarmControl)**
  
  * 解决生物过剩的农场
* [**MobLimit**](https://github.com/Minebench/MobLimit)
  
  * 类似于上面提到的农场控制。
    （你只需要其中一个就行）
* **[Insights](https://www.spigotmc.org/resources/insights-super-configurable-region-limits-asynchronous-scans-1-17-x.56489)**
  
  * 一个反红石爆炸和方块限制器插件。
  * 定位潜在卡服机器的有用工具。
* **[Spark](https://spark.lucko.me/download)**
  
  * 一个分析器插件，可为您提供有关性能的更多见解。
    （除了 Timings 之外，它是解决服务器中可能出现的问题的完美辅助工具）
* **[BlueMap](https://www.spigotmc.org/resources/bluemap.83557)**
  
  * 提供服务器 3D 渲染的地图插件。 （警告，这个插件非常耗费资源）
  * 如果您想了解它的外观，可以在此处[查看 IOE 展示](https://map.eternity.community/)。
* **[squaremap](https://github.com/jpenilla/squaremap/releases)**
  
  * 一个轻量级的以原版为主题的 2D 地图，具有快速渲染速度和高度优化。
* **[ViaVersion](https://github.com/ViaVersion/ViaVersion)**
  
  * 在每次的 Minecraft 更新期间提供版本兼容的插件。
    您可以配置此插件以允许来自较新版本 Minecraft 的玩家加入服务器，同时等待稳定的 Paper 版本发布。
* **[Decent Holograms](https://www.spigotmc.org/resources/decent-holograms-1-8-1-18-papi-support-no-dependencies.96927)**
  
  * 一个不会影响服务器性能的全息插件。
* **[AntiBookBan](https://www.spigotmc.org/resources/antibookban.89720)**
  
  * 一个简单的插件，用于禁止书籍中的非 ASCII 字符，从而防止 bookban 漏洞。~~（国内用不上）~~
* [**Prism**](https://github.com/AddstarMC/Prism-Bukkit)
  
  * 具有回滚、恢复方块历史记录的管理插件。
* **[CoreProtect](https://github.com/PlayPro/CoreProtect/)**
  
  * 类似于 Prism，另一个管理插件。
    （两者选其一）
* **[LWCX](https://www.spigotmc.org/resources/lwc-extended.69551)**
  
  * 轻量级单方块保护插件。
* **[OreAnnouncer](https://www.spigotmc.org/resources/oreannouncer-collects-data-about-mined-blocks.33464)**
  
  * 作为 anti-xray 的替代品，此插件会在矿物被采掘时通知您。
* **[Quantum Random Teleport](https://www.spigotmc.org/resources/quantum-modern-wild-random-spawn-rtp-reimagined.86902/)**
  
  * 随机传送插件
* **[Random Teleport](https://www.spigotmc.org/resources/random-teleport-full-nether-support.76021/)**
  
  * 另一个随机传送插件
* **[ChestShop](https://www.spigotmc.org/resources/chestshop.51856)**
  
  * 简单的箱子商店插件
* **[Shop](https://www.spigotmc.org/resources/shop-a-simple-intuitive-shop-plugin.9628)**
  
  * 又一个箱子商店插件
    (任选其一)
* **[QuickShop-Hikari](https://github.com/Ghost-chu/QuickShop-Hikari)**
  
  * 又是一个箱子商店插件，只不过仅支持 1.18 ，功能比QuickShop-Reremake 多。

一个制作精良的插件可以为您的服务器带来额外的功能和改进； 另一方面，粗制滥造的插件可能会单枪匹马地降低性能，甚至对您的服务器造成永久性损坏。 **请在安装前查看插件背后的开发人员/组织的历史**，并在将它们投入生产之前对其进行适当的测试，从而进行尽职调查。

利用搜索功能，您可以轻松地在 Discord 上的 **#paper-help** 频道中收集有关插件的信息片段。

## **有用的工具**

* **[MCA Selector](https://github.com/Querz/mcaselector)**
  
  * 离线查看、删除和更改区块数据的工具。
  * 在使用此工具执行任何操作之前，请确保您有[正确的备份](https://eternity.community/index.php/paper-optimization/#Backup-and-Recovery-Best-Practice)。
* **[Amulet Editor](https://www.amuletmc.com/)**
  
  * 一个流行的 Minecraft 世界编辑器，允许对您的世界进行外部编辑。
* [**Web NBT Editor**](https://irath96.github.io/webNBT)
  
  * 查看和编辑 NBT 文件的简单工具。
* **[World size Calculator](https://onlinemo.de/world)**
  
  * 估计给定世界的文件大小的工具。
* **[Bedrock/JAVA world converter](https://chunker.app/)**
  
  * 在两个平台之间转换您的世界的实验性工具。

> 此外，还有一些旨在改善原版游戏玩家体验的客户端模组，强烈推荐给可能希望获得最佳游戏体验的玩家。
> 但是，它与这篇博文的主题不符，相反，[您可以在此处查看 **[Minecraft 客户端优化模组]**](https://eternity.community/index.php/minecraft-client-optimization)。

# **需要避免的一些事**

* **避免使用生物堆叠插件**
  
  * 生物堆叠是一个天生就有缺陷的想法。 生成怪物会消耗服务器资源，启用怪物堆叠后，服务器永远不会达到怪物上限，并且可能会陷入地狱级别的无限循环生成。
* 再加上似乎是零编码的堆叠插件，应该不惜一切代价避免堆积生物。
* **避免使用服务器优化插件与自动清理插件**
  
  * **修复性能问题不应该治标不治本，不得掩盖其问题，应该从根处理。**
  * 诸如 ClearLagg（清道夫/扫地大妈） 或 EntityTrackerFixer (ETF) 之类的插件会引入游戏机制上的不一致，为现有任务（例如清理地面物品和取消跟踪实体）创建不必要的工作，并且有时会对您的世界造成永久性损害（ETF 会从导致它们的实体中删除 AI 即使在插件被删除后也会有永久性的“脑损伤”）。
* **千万不要让玩家重新放置刷怪笼**
  
  * 玩家经常要求能够精准采集刷怪笼；但是，考虑到当足够多的刷怪笼聚集在一起时，它基本上是一个卡服机，这不是一个好主意。 为了服务器性能，不要让你的玩家重新放置刷怪笼。
* **不要使用具有重复/循环函数的数据包**
  
  * 具有重复函数的数据包会影响性能，应不惜一切代价避免。
  * 查找您希望拥有的数据包功能的插件进行替换。
* **不要从 BlackSpigot、MCM、陌生人或任何其他不明来源获取您的插件**
  
  * 你肯定不会购买从火车站里陌生人给你的苹果手机吧？ 同样的事情也适用于获取付费插件。
  * 从有信誉的网站获得你的插件，并进行尽职调查，以审计代码库或开发人员本身。
  * 轻描淡写，您的服务器可能会由于编码不良的插件而遭受巨大的性能损失，或者更糟糕的是被恶意软件通过内置后门入侵，更糟糕的是，您的服务器本身可能会被其他恶意活动接管。
* **请勿自动更新您的 paper.jar**
  
  * Paper 开发周期不包括适当的 release/beta 版本控制方案，因此有时漏洞百出的构建可能会意外流出。
  * 自动化构建提供了便利，但您不应盲目下载最新的 Paper.jar 以用于任何生产环境。
* ["著名"的 twitch 主播 Kenny](https://github.com/KennyTV) 编写的 **Minecraft 耻辱柱**
  
  * 适当地避免此列表中的所有内容，但不代表里面的所有内容不能用。
  * [https://kennytv.github.io/list-of-shame](https://kennytv.github.io/list-of-shame)

---

# **一些可供选择的服务商**

*免责声明：我没有恰任何个人或团体的烂钱然后制作此列表的。 此列表纯粹是出于对我个人经验和社区意见的热爱而制作的*

在托管 Minecraft 服务器时，有一下可用选项。 请阅读并理解每一项的限制。 以下提供了最强大的~~土豆~~的服务商。

* **专用服务器 (Baremetal)** – 一台完全属于您自己的专用机器。
  
  * 这是最昂贵的选择，需要您对 Linux 管理技能有基本的了解
  * 国外用户推荐使用 **[OVH](https://www.ovhcloud.com/en/bare-metal)** 或者 **[Hetzner](https://www.hetzner.com/)**，国内用户直接上 XX 云。
  * 非常适合同时拥有 50 名玩家以上的服务器
* **共享主机** – 一项托管服务，您可以在其中与其他 Minecraft 服务器共享资源，国内一般以 MultiCraft、翼龙、MCSM 面板服形式进行售卖。
  
  * 国外用户推荐使用 [Bloomhost](https://bloom.host/) 或者 [DedicatedMC](https://dedicatedmc.io/) ，国内用户请不要在某宝购买面板服，建议在找到服务商之前先看看他们在腐竹之间的口碑如何进行决定。
  * 依照你选择的服务器性能而定，一般来说可以容纳 10 - 50 名玩家不等。
  * 上面提到的两个主机被许多人广泛推荐，因为它们默认使用 [Aikar's Flag](https://eternity.community/index.php/paper-optimization/#JVM) 并允许您的 JVM 实例完整地使用 RAM。 这绝不是性能的保证，但它表明他们可能没有超卖他们的机器。
* **Oracle Cloud Free Tier** – 免费且速度非常快的云实例，非常有优势。
  
  * 由于最近人气飙升，此优惠可能不适用于您所在的地区。
    然而，试试运气总不会有错。
  * [仔细阅读这篇博文以获取更多信息](https://blogs.oracle.com/developers/how-to-set-up-and-run-a-really-powerful-free-minecraft-server-in-the-cloud)
  * 非常适合具有约 20 名玩家同时使用基于 ARM 的 A1 实例的服务器。
    （我个人拥有并测试过一个，我可以保证它的性能。它能够毫无问题地处理一个小型生存服务器）
  * 如果您确实选择了此托管选项，请注意，您的实例将/可能在试用开始后的第 60 天被禁用。 您可以删除现有且现已禁用的实例，然后使用**引导卷备份重新制作一个新实例**（假设您所在的区域仍有可用的免费次数）。
* **VPS** – 虚拟专用服务器
  
  * 这在表面上可能听起来很划算，但 VPS 通常没有上述共享主机的价值，因为硬件往往是过时的（毕竟洋垃圾），而且整个 VPS 行业更倾向于网络托管和其他不那么密集的 CPU 工作负载。
  * 理想情况是......额，机器的使用寿命高一点。如果你还是喜欢这个选择，那就找一个专门从事高端游戏服务器托管的主机，而不是一般用途的。
* **所谓符合“预算”的主机** – 一种更便宜的共享主机替代方案，性能要低得多。
  
  * 类似于上面的**共享主机**，但要便宜得多。他们通过在一台物理机器上配备尽可能多的服务器来获利（这称为超卖）
  * 这些类型的主机通常在推广上花费最多，并且在社交媒体上拥有广泛的团队来推广他们的服务。通常会提供巨大的折扣或优惠券来推动销售。
  * 他们经常对开服小白下手，这里有几个简单的方法可以在网络上识别他们。
    
    * 如果在主机介绍中没有标明 CPU 型号。
    * 如果他们以最大玩家数为基础宣传计划。
    * 如果他们将 RAM 作为性能提升进行追加销售。
    * 如果他们提供无限存储空间。
    * 如果他们全年都有折扣优惠。
    * 如果服务器的 JVM 参数中存在 **Xms=128M**
      ([您可以在 timings 报告中找到此信息](https://eternity.community/index.php/paper-optimization/#timings))
  * 一般情况下只适合5 - 25名玩家
    
    * 由于普遍的超卖行为，服务器可能在机器可能难以处理工作负载的高峰时段表现不佳。
    * 你的服务器也可能遭受突然的性能下降，有可能不是你自己的错，只是因为你的服务器位于一个过度拥挤的环境中，另一个 Minecraft 实例可能会使机器过载，简直是一条臭鱼坏了一锅粥。
* **只在暑假出售的主机 (Summer Host)** – 就像上面那一部分，但在各方面甚至更糟
  
  * "Summer Host" 一词来自于学校在放暑假期间出现的新服务商，这一时段对 Minecraft 服务器的需求正好达到顶峰。 一旦需求下降，大多数服务商将会装死或者跑路，明年再换个名字东山再起。
    请阅读这个非常搞笑但是也很准确的 Summer Host 页面了解更多信息。
  * 想要快速的在暑假期间捞一波钱吗？快来点击 [如何成为一家只在暑假期间运营的服务商](https://summerhost.pages.dev/) 了解更多滴信息吧！
  * 这种服务器只适合 5-10 名玩家。
  * 您应该不惜一切代价避免这种类型的主机，因为它们可能会在没有提前公告的情况下跑路，您宝贵的 Minecraft 世界可能会永远丢失。
* **免费不要钱滴主机** – 点名 Minehut 与 Aternos
  
  * **如果你不为产品氪金，那么你就成为了他们的产品**
  * 通过巧妙的营销手段，他们能够为每个人提供资源非常有限的免费服务器。
  * 他们从他们拥有的大量用户群中获利，并将启动服务器的风险外包给那些敢于为高级计划付费的人。
  * Minehut 和 Aternos 都对其服务施加了严格的限制，例如 JVM 参数、yml 配置、插件，并且不提供转移文件的能力。你是在购买他们的生态系统，你就被他们噶了韭菜。
  * 非常滴适合……如果你是小学生，家里父母不让你瞎花钱，然后你还想要服务器。
* **用自己的主机** – 在自己家开服务器
  
  * 如果您可以忽略将家宽的公网 IP （笑死，国内几乎都不给公网 ipv4 了）暴露给公众的潜在安全隐患以及被朋友 DDOS 攻击的风险，那么这是一个可行的选择。 但是，强烈建议不要这样做。
  * 如果你想要尝试的话，[请查看 Larry 撰写的 Syscraft 的有用指南](https://github.com/syscraft-mc/starter-server/blob/master/README.md)，了解如何开服。
  * 非常适合（严重依赖于您的硬件）
* **树莓派 (Raspberry pi)**
  
  * 对于那些喜欢瞎折腾的人来说，这是一个完美的解决方案。 事实上，Raspberry Pi 的功能不足以运行最新的 Minecraft 版本，而无需做出大量的游戏玩法妥协。
  * 非常适合有 1~5 名玩家的服务器。

# **域名与 SRV 记录**

> 如果您拥有一个域名，您可以设置一个服务记录 (SRV) 以拥有一个更漂亮且更容易记住的服务器地址。

您可以使用例如 **minecraft.yourdomain.com** 的域名，而不是将 **192.168.1.1:25555** 作为您服务器的公开地址

为此，您需要一个 **A 记录**和一个 **SRV 记录**。

```
在这种情况下 www.yourdomain.com 是您的域名，您希望创建 mc.yourdomain.com 作为您的服务器地址，而不是 192.168.1.1:25555
```

第 1 步：**创建一条 A 记录**，将主机名 **mc.yourdomain.com** 指向您服务器的**公网 IP 地址**

第 2 步：使用主机名 **_minecraft._tcp.mc.yourdomain.com** 创建一个 **SRV 记录**

将有设置**优先级**、**权重**、**端口**和**目标**的选项。

您需要将其设置为 **0, 0, 25555, mc.yourdomain.com**

* 如果您在默认端口 (25565) 上运行 Minecraft，则只需要 A 记录。
  （上面的示例是为共享主机上或在非默认端口上运行的服务器提供的）
* 请注意，DNS 更改需要时间来应用更改，并且生成的链接可能无法立即工作，耐心点。
* 某些 ISP DNS 不遵守 SRV 记录，因此如果您选择在非标准 25565 端口上运行，您的流程可能会有所不同。

---

# **后记**

> 哇，你做到了这一步，我希望你没有跳过任何东西。 本指南的最初想法是让刚踏上 Minecraft 服务器托管的初学者尽可能简单。 在写作过程中，我试图简化一些事情，如果有什么可以更好地解释或改进，请告诉我。 我希望保持本指南尽可能准确和最新，因此如果您发现任何错误，请通过 [Island of EterNity](https://discord.gg/eternity) 或 PaperMC Discord **[@EterNity#0001](https://discordapp.com/users/177150983258767360)** 告诉我，或者您可以发送电子邮件 **eternity&eternity.community(请将&换成@)**

---

# **相关链接**

* **GitHub** : [https://github.com/PaperMC](https://github.com/PaperMC)
* **Discord** : [https://discord.gg/PaperMC](https://discord.gg/PaperMC)
* **论坛** : [https://forums.papermc.io](https://forums.papermc.io/)
* **Wiki** : [https://paper.readthedocs.io](https://paper.readthedocs.io/)
* **JavaDocs** : [https://papermc.io/javadocs](https://papermc.io/javadocs)
* **捐赠:** [https://papermc.io/sponsors](https://papermc.io/sponsors)
