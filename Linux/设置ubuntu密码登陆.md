1. 切换root权限：`sudo -i`

2. 修改配置文件：`vim ~/etc/ssh/sshd_config`

   将一下两个字段前面的**`#`**删掉，再将后面的单词改为**`yes`**

   **`PermitRootLogin`**

   **`PasswordAuthentication`**

   <img src="/Users/zhengyuxuan/Knowledge/Linux/img/修改配置文件.png" alt="image-20240717201803202" style="zoom:50%;float: left" />

3. 初始密码/修改密码：**`passwd`**按步骤修改即可

4. 重启ssh服务：**`systemctl restart ssh`**