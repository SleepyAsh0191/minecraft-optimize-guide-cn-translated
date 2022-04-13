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
  This is where all the server file will be generated and stored.  
  (If you have OneDrive Sync enabled, DO NOT put your Minecraft folder in the synced area to avoid possible issues…  
  Default folder for OneDrive Sync is Desktop, Documents, and Pictures)  
  Right click anywhere > Create New Folder