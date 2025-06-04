### `nvm`安装步骤及使用问题

**:tipping_hand_woman:注意：安装`nvm`前需要删除本地原有的`node`**

1. **安装`nvm`版本1.1.1** 

   [nvm安装地址]: https://nvm.uihtm.com/download.html

   <img src="D:\PrivateFiles\Note\复习笔记\img\image-20250304113011423.png" alt="image-20250304113011423" style="zoom: 67%;" />

   `nvm`默认安装地址为：

   `C:\Users\'用户名'\AppData\Roaming\nvm`

   `C:\Program Files\nodejs`

2. **`nvm`安装node步骤**

   :tipping_hand_man:注意全程使用管理员权限开启`cmd`

   * `nvm -v`

   ![image-20250304113409261](D:\PrivateFiles\Note\复习笔记\img\image-20250304113409261.png)

   * 安装多版本node

     `nvm install 16.20.2`

     `nvm install 20.10.0`

   * 查看node版本

     `nvm list`

   * 切换node版本

     `nvm use 16.2.2`

3. `nvm use `具体某一个node版本后，查看`node -v` 下载配置`pnpm`

   :tipping_hand_man:注意全程使用管理员权限开启`cmd`

   ```cmd
   //查看node版本
   node -v
   npm -v
   下载pnpm
   npm install -g pnpm@8.15.9
   /**查看pnpm版本**/
   pnpm -v
   ```

**遇到的问题：`nvm`安装`node`后，`node`不识别**

解决办法：

1. 找到`nvm`的安装地址，在按钮地址文件夹目录下手动建立一个`nodejs`文件夹

```cmd
C:\Users\han>where nvm
C:\Users\han\AppData\Roaming\nvm\nvm.exe
```

<img src="D:\PrivateFiles\Note\复习笔记\img\nvm-File.png" alt="nvm安装目录下建立nodejs文件夹" style="zoom:50%;" />

2. 修改`nvm`用户环境变量

   将`NVM_SYMLINK`地址改为`C:\Users\yonghuming\AppData\Roaming\nvm\nodejs`

   ![](D:\PrivateFiles\Note\复习笔记\img\环境变量.png)

3. 删除原本下载的node版本重新下载node

   ```cmd
   nvm ls
   nvm uninstall -g 原本node版本
   nvm install -g node目标版本
   node -v
   ```


#### 问题：安装yarn global add @vue/cli 成功后在全局无法识别vue命令

解决：

1. 获取 where yarn bin 将yarn的运行文件地址添加到系统环境变量中
2. 更改当前cmd的文件的用户权限为全控制
3. 在window系统cmd打开对应文件夹地址
4. 重新删除安装包 yarn global remove vue/cli