1. 查看端口占用

   `lsof -i:端口号`

2. 杀死进程

   `kill 进程号`

3. 查看进程信息

   `ps -ef|grep 进程名`

4. 更改文件权限 change mode

   `chmod xxx 文件名`

   eg: `chmod 777 conf.d`

5. 更改文件所属用户 change owner

   `chown 用户名 文件/文件夹` 不会向子目录传递

   eg：`chown www-data /home/ubuntu/www`

   `chown -R 用户名 文件夹  ` 会修改所有子目录文件