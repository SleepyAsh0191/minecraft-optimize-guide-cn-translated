# Paper 酱的 Minecraft 服务器自开服教程

---

# **介绍/背景**

你好呀！您点进这里是可能是因为经济原因买不起，或者因为您的父母拒绝为您购买服务器，并且您仍然想使用您的个人计算机作为主机与您的朋友一起玩，但您实际上不知道自己在做什么，也不知道从哪里开始。幸运的是，本指南是为对 Minecraft 开服一无所知的小白设计的！

---

最后更新：2022 年 1 月 21 日

---

# **必备条件**

**为了帮助指导您完成整个过程，请确保您满足下面列出的所有要求。**

* 拥有基本的阅读能力，会英语，并且能够毫无困难地遵循给出的指示。
* 当前帐户在计算机上具有管理员权限。

---

# **说明**

## **请按照 100% 的说明进行操作。重要的是，请不要跳过任何步骤！**

## **安装 JAVA**

1. 前往 [https://adoptium.net](https://adoptium.net/)
2. 确保选中 **Temurin 17 (LTS)** 然后单击 **Latest Release**。
3. 下载完成后，双击文件启动安装程序。

![Step 1](https://s1.ax1x.com/2022/04/13/LMKKUg.png) | ![Step 2](https://s1.ax1x.com/2022/04/13/LMKM5Q.png)
---|---

4. 按照说明进行操作，并在询问时点击 **Next**。
5. 在自定义设置上，选择 **Set JAVA_HOME variable > Will be installed on local hard drive**。

![Step 4](https://s1.ax1x.com/2022/04/13/LMvJ5F.png) | ![Step 5](https://s1.ax1x.com/2022/04/13/LMvGUU.jpg)
---|---

6. 完成安装并关闭窗口。
7. 通过打开**开始菜单**验证 JAVA 是否已成功安装，然后输入 **“cmd”**

![](https://s1.ax1x.com/2022/04/13/LMvvq0.png) | ![](https://s1.ax1x.com/2022/04/13/LMxEs1.png)
---|---

8. 单击以打开**命令提示符/PowerShell**，然后输入 **“java -version”**
它应该返回以下输出或类似的指示 1.17.x 已安装。

![](https://static01.imgkr.com/temp/f7d8bb32e9fd4facaa608f816424da58.jpg)

# **创建你的 Minecraft 服务器文件夹**

9. **找到合适的位置来创建您的 Minecraft 根目录。**  
  这是生成和存储所有服务器文件的地方。
  （如果您启用了 OneDrive 同步，请不要将您的 Minecraft 文件夹放在同步区域以避免可能出现的问题......
  OneDrive 同步的默认文件夹是桌面、文档和图片）  
  右键单击任意位置 > 新建文件夹
  
![](https://s1.ax1x.com/2022/04/17/LU2PSA.jpg) > ![](https://s1.ax1x.com/2022/04/17/LU29Wd.jpg)  

10. 在 Minecraft 文件夹中，**右键**单击任意位置 > **新建** > **文本文档**
将文件命名为 **eula** > **打开 eula.txt 文件** > 输入 **eula=true** > **保存退出**

![](https://s1.ax1x.com/2022/04/17/LU2InP.jpg) > ![](https://s1.ax1x.com/2022/04/17/LU24Xt.jpg) > ![](https://s1.ax1x.com/2022/04/17/LU2h6I.jpg)

[(这意味着您已阅读并同意 Mojang 最终用户许可协议)](https://account.mojang.com/documents/minecraft_eula)

# **创建 startmc.bat**

1. 前往 https://startmc.sh
2. 在文件名下，输入 server.jar （取决于你服务器核心的文件名）
3. 在脚本类型下拉菜单中选择 Basic (Windows)
4. 在 RAM 大小下输入一个合适的数值（在本例中为 2000M）
5. （请注意，如果您还想在托管它的同一台 PC 上玩 Minecraft，您需要考虑系统 RAM 使用情况以及 Minecraft 客户端以及您运行的任何其他内容。最好通过打开任务管理器进行检查并查看您的可用 RAM 总量）