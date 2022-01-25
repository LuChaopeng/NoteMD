1. 使用File watcher自动编译less报错

   > cmd.exe /D /C call C:\Users\ming\AppData\Roaming\npm\lessc.cmd style.less style.css
   > '"node"' �����ڲ����ⲿ���Ҳ���ǿ����еĳ���
   > ���������ļ���
   >
   > 进程已结束，退出代码为 9009

   看到一篇文章但并未解决。

   > #### 配置环境 
   >
   > 1、安装webstrom
   >
   > 2、安装nodeJS[http://nodejs.cn/download/](https://link.jianshu.com?t=http://nodejs.cn/download/)选择windows系统（.msi）下载完成后直接安装
   >
   > win+r打开cmd命令行，输入node -v 显示node版本表示安装成功。
   >
   > 3、安装less
   >
   >    ①win+r打开cmd命令行工具，输入npm install -g less 安装less，
   >
   >    ②可能错误：出现多个npm err、node err
   >
   > ​        解决办法：尝试输入npm config set strict-ssl false，问题解决再恢复：npm config set strict-ssl true
   >
   > ​    ③安装完成后，注意提示路径，找到完成后提示的路径，复制路径:"users\Administrator\AppData\Roaming\npm\lessc.cmd"
   >
   > 4、配置webstorm
   >
   > file>settings>Tools>file Watchers>
   >
   > 找到右边的绿色的+，选择Less文件，弹出设置菜单
   >
   > 配置Less编译菜单，设置Program地址：安装less第三步复制的路径users\Administrator\AppData\Roaming\npm\lessc.cmd ，其余选项不用更改
   >
   > 做完配置后，在webstorm所在项目中创建Less文件
   >
   > 可能错误，编辑器控制台报错，提示node+乱码，可能node没有安装成功，或者安装成功后系统配置没有完全，可以尝试重启电脑若还报错重装nodeJS
   >
   > 如果没有错误，则可以看到Less文件创建后提示有下级文件夹，生成同名css文件。每次更改less文件，css文件实时编译，同步更改。
   >
   > 然后就可以快乐的使用Less了   ~~O(∩_∩)O~~
   >
   > 
   >
   > 作者：Anfly
   > 链接：https://www.jianshu.com/p/2dd82f523489
   > 来源：简书
   > 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。