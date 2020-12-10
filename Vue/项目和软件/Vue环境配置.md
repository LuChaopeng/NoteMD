## Vue环境配置

1. 安装NVM（Node版本管理器）

   Windows下在https://github.com/coreybutler/nvm-windows/releases下载并安装；

   在环境变量中，添加NVM_HOME，路径为nvm的路径；Path里添加%NVM_HOME%和%NVM_SYMLINK%

2. 安装node

   使用nvm list available查看所有远程node列表，使用nvm install 15.3.0 安装15.3.0版本（安装完后重启命令行）

   使用nvm use 15.3.0 使用15.3.0版本的node

   关于NPM命令：npm随会node一起被安装

3. 卸载NVM

   windows下通过控制面板卸载

4. 使用cnpm代替npm命令

   npm install -g cnpm --registry=https://registry.npm.taobao.org