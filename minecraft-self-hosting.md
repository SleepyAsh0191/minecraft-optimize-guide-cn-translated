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

11. 前往 https://startmc.sh
12. 在文件名下，输入 server.jar （取决于你服务器核心的文件名）
13. 在脚本类型下拉菜单中选择 Basic (Windows)
14. 在 RAM 大小下输入一个合适的数值（在本例中为 2000M）
   （请注意，如果您还想在托管它的同一台 PC 上玩 Minecraft，您需要考虑系统 RAM 使用情况以及 Minecraft 客户端以及您运行的任何其他内容。最好通过打开任务管理器进行检查并查看您的可用 RAM 总量）

![](https://s1.ax1x.com/2022/04/28/LOK3kQ.jpg) > ![](https://s1.ax1x.com/2022/04/28/LOKlTg.jpg)

15. 填写完上述所有选项后，向下滚动到页面末尾 > 单击**下载** > 如果 Google Chrome 向您发出警告，请选择**保留**。

[![LOKbct.jpg](https://s1.ax1x.com/2022/04/28/LOKbct.jpg)](https://imgtu.com/i/LOKbct)

16. 将 startmc 文件移动到您在上一步中创建的 Minecraft 根目录中。

# **下载 Paper**

17. 前往 https://papermc.io/downloads
18. 选择要下载的最新版本。 （您应该始终使用最新版本的 Paper，因为它包含最新的优化和错误修复）

[![LOMm4J.jpg](https://s1.ax1x.com/2022/04/28/LOMm4J.jpg)](https://imgtu.com/i/LOMm4J)

19. 如果弹出警告，请选择**保留**。
20. 下载完成后，将 Paper-1.18.1-xxx.jar 文件移动到您之前创建的同一个 Minecraft 根目录中。
21. **将 `Paper` 文件重命名为 `server`**（如果您启用了文件扩展名，它将是 `paper.jar` > `server.jar`）（这样您每次更新 Paper 服务器时都不必更改您的 startmc 文件）

# **启动 Paper 服务器**

22. 是时候启动您的 Paper 服务器了！ 让我们做最后的检查吧！
如果按照上述所有步骤操作，您的 **Minecraft 根目录**应如下所示……

[![LOl9O0.jpg](https://s1.ax1x.com/2022/04/28/LOl9O0.jpg)](https://imgtu.com/i/LOl9O0)

（如果看起来不是这样，请回到第一步，看看你错过了什么……）

23. **双击 `startmc` 启动服务器！**

[![LOlFTU.jpg](https://s1.ax1x.com/2022/04/28/LOlFTU.jpg)](https://imgtu.com/i/LOlFTU)

24. 应该会出现一个命令提示符窗口，一旦完成 patching，您应该会看到 `Done (1.165s)！ `或类似的东西，然后您的服务器已成功启动！

# **加入本地服务器**

25. 启动 Minecraft 客户端 > 多人游戏 > 添加服务器并输入 `localhost` 作为服务器地址并加入！

[![LOlMm6.jpg](https://s1.ax1x.com/2022/04/28/LOlMm6.jpg)](https://imgtu.com/i/LOlMm6)

# **后记**

希望您发现本指南有助于启动您的服务器，并将任何其他问题重定向到官方 PaperMC Discord 上的 `#paper-help` 频道。 如果您需要为家庭网络之外的朋友设置端口转发以进行连接，那么真正有趣的部分就开始了，但这是我今天不想开的一个天坑。 你最好的选择是在百度上搜索 “你的路由器型号+如何端口映射” 作为关键字。 祝你好运！

如果您在路由器管理页面中显示的 WAN IP 为 10 开头，那么请向您所在地区的运营商申请公网 IP，鉴于国内大多数家用宽带不再提供公网 IPv4 服务，请自行搜索 FRP （内网穿透）相关信息（无广）。

如果您有任何改进建议，请通过 Discord [**@EterNity**](https://discordapp.com/users/177150983258767360) 与我联系，或发送电子邮件至 eternity#eternity.community 给我发送电子邮件，再次感谢您！
