# Mac下iterm2终端安装rz、sz 命令方法



### Mac下iterm2终端安装rz、sz 命令方法



> 近来，在平时使用过程中需要向服务器上传文件操作，在Windows下有Xshell可以使用rz、sz命令用来上传和下载文件，但由于我使用的是Mac OSX，所以在iterm2终端上安装了rz、sz命令。



1. 然后需要下载 iterm2-send-zmodem.sh 和 iterm2-recv-zmodem.sh 保存到Mac的 /usr/local/bin/ 路径下

2. 在OSX上安装lrzsz：

   ```shell
   brew install lrzsz
   ```

3. 进入目录添加权限

   ```shell
   cd /usr/local/bin
   
   chmod +x iterm2-send-zmodem.sh
   
   chmod +x iterm2-recv-zmodem.sh
   ```

4. 设置iterm2的触发器（triggers）
   执行如下以下步骤：command+“,” 组合键打开item2的“Preferences”面板->Profiles选项卡->Advanced->Triggers（点击Edit即可）

   > Regular expression: \*\*B0100
   >     Action: Run Silent Coprocess
   >     Parameters: /usr/local/bin/iterm2-send-zmodem.sh
   > Regular expression: \*\*B00000000000000
   >     Action: Run Silent Coprocess
   >     Parameters: /usr/local/bin/iterm2-recv-zmodem.sh 



## 最终截图

![image-20210316105329231](/Users/huxuekuo/Library/Application Support/typora-user-images/image-20210316105329231.png)