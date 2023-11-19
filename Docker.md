#### 什么是Docker

> - Docker 是一个开源的应用容器引擎
> - Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上
> - 容器是完全使用沙箱机制，相互隔离
> - 容器性能开销极低



#### docker架构

> 镜像(Image): Docker 镜像（Image），就相当于是 一个 root 文件系统。比如官方镜像 ubuntu:16.04 就包含了完整的一套 Ubuntu16.04 最小系统的 root 文件系统.
>
> 容器（Container）：镜像（Image）和容器（Contain er）的关系，就像是面向对象程序设计中的类和对象一 样，镜像是静态的定义，容器是镜像运行时的实体。容 器可以被创建、启动、停止、删除、暂停等.
>
> 仓库（Repository）：仓库可看成一个代码控制中心， 用来保存镜像.



#### docker 的基本命令

##### 镜像相关命令

1. 查看镜像

   ```sh
   # 查看本地所有的镜像
   docker images
   
   docker images -q # 查看所用镜像的id
   
   ###参数解释
   REPOSITORY：镜像名称
   TAG：镜像标签
   IMAGE ID：镜像ID
   CREATED：镜像的创建日期
   SIZE：镜像大小
   ```

2. 镜像搜索

   ```sh
   docker search 镜像名称
   ```

3. 镜像拉取

   ```sh
   # ()可选 如果不指定版本号,默认下载最新版本,可去docker hub查询镜像版本
   docker pull 镜像名称(:版本号)
   12
   ```

4. 镜像删除

   ```sh
   # 删除指定本地镜像
   docker rmi 镜像id 
   
   # 删除所有本地镜像
   docker rmi docker images -q
   ```

##### 容器相关命令

1. 查看容器相关

   ```sh
   docker ps # 查看正在运行的容器
   docker ps –a # 查看所有容器，包括正在运行和停止的容器
   ```

2. 创建并启动容器

   ```sh
   docker run 参数   启动
   
   #### 参数
   -i：保持容器运行。通常与 -t 同时使用。加入it这两个参数后，容器创建后自动进入容器中，退出容器后，容器自动关闭
   -t：为容器重新分配一个伪输入终端，通常与 -i 同时使用
   -d：以守护（后台）模式运行容器。创建一个容器在后台运行，需要使用docker exec 进入容器。退出后，容器不会关闭
   -it 创建的容器一般称为交互式容器，-id 创建的容器一般称为守护式容器
   --name：为创建的容器命名
   
   -v 本地目录:容器目录  把容器的某个目录挂载到宿主机的某个目录中去
   
   -p 本地端口:服务器端口
   ```

   2.1. 交互式容器启动

   > 以**交互式**方式创建并启动容器，启动完成后，直接进入当前容器。使用exit命令退出容器。需要注意的是以此种方式 启动容器，如果退出容器，则容器会进入**停止**状态.

   ```sh
   #创建并启动名称为 mycentos7 的交互式容器；
   # 下面指令中的镜像名称 centos:7 也可以使用镜像id 
   # 使用的shell是/bin/bash
   docker run -it --name=mysql57 mysql:5.7 /bin/bash
   ```

   2.2  守护式进程

   > 创建一个守护式容器；如果对于一个需要长期运行的容器来说，我们可以创建一个守护式容器.

   ```sh
   #创建并启动守护式容器  -id和di相等
   docker run -di --name=mycentos2 centos:7
   
   #登录进入容器命令为：docker exec -it container_name (或者 container_id) /bin/bash（exit退出 时，容器不会停止）
   docker exec -it mycentos2 /bin/bash
   ```

3. 进入容器

   ```sh
   # docker exec -it container_name (或者 container_id) /bin/bash
   #（exit退出 时，容器不会停止）
   docker exec 参数 
   # eg: 
   docker exec -it mycentos2 /bin/bash
   12345
   ```

4. 停止容器、删除容器、启动容器

   ```sh
   ## 停止
   docker stop 容器名称或者容器id
   #### 启动容器
   docker start 容器名称或者容器id
   
   ### 2.3.6 删除容器
   注意：如果容器是运行状态则删除失败，需要停止容器才能删除
   #删除指定容器
   docker rm 容器名称或者容器id
   
   # 删除所有容器：
   docker rm docker ps -a -q
   ```

5. 查看容器信息

   ```sh
   docker inspect 容器名称或者容器id  查看容器的基本信息
   容器之间在一个局域网内，linux宿主机器可以与容器进行通信；但是外部的物理机笔记本是不能与容器直接通信的，如果需要则需要通过宿主机器端口的代理.
   #  Docker容器的数据卷
   
   ## 3.1 数据卷概念
   - 数据卷是宿主机中的一个目录或文件
   - 当容器目录和数据卷目录绑定后，对方的修改会立即同步
   - 一个数据卷可以被多个容器同时挂载
   - 一个容器也可以被挂载多个数据卷![在这里插入图片描述](https://img-blog.csdnimg.cn/20200910101938570.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FCZXN0Um9va2ll,size_16,color_FFFFFF,t_70#pic_center)
   - - **作用:**
   - 容器数据持久化
   - 外部机器和容器间接通信
   - 容器之间数据交换
   ##  3.2 配置数据卷
   **创建启动容器时，使用 –v 参数 设置数据卷**
   docker run -id ... -v 宿主机目录(文件):容器内目录(文件) 
   ### 注意事项
   目录必须是绝对路径
   如果目录不存在，会自动创建
   一个容器可以挂载多个数据卷
   一个数据卷也可以被多个容器挂载
   两个容器可以挂载同一个容器
   ```

#### 应用部署

##### mysql的部署

```sh
docker search mysql  #### 搜索镜像信息

docker pull mysql:5.7 #### 下载镜像信息

#### 启动mysql系统
docker run -id --name=mysql57  -p 3307:3306 -v $PWD/conf:/etc/mysql/conf.d  -v $PWD/logs:/logs   -v $PWD/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7  

#### 参数说明：
- **-p 3307:3306**：将容器的 3306 端口映射到宿主机的 3307 端口。
- **-v $PWD/conf:/etc/mysql/conf.d**：将主机当前目录下的 conf/my.cnf 挂载到容器的 /etc/mysql/my.cnf (配置目录)
- **-v $PWD/logs:/logs**：将主机当前目录下的 logs 目录挂载到容器的 /logs.(日志目录)
- **-v $PWD/data:/var/lib/mysql** ：将主机当前目录下的data目录挂载到容器的 /var/lib/mysql .(数据目录)
- **-e MYSQL_ROOT_PASSWORD=root：**初始化 root 用户的密码

4 进入容器，操作mysql

docker exec –it c_mysql bash
```

##### nginx部署，Vue项目上线

```sh
### vue2项目打包的过程
npm run build  #### vue.config.js中配置相对路径

#### 生成一个dist目录
```



```sh
docker search nginx 
docker pull nginx

##### 开始骚操作
docker run -id --name=vue2_nginx -p 8088:80 -v $PWD/dist:/usr/share/nginx/html/dist -v $PWD/conf.d:/etc/nginx/conf.d nginx

-p  8088:80 ## 宿主机8088端口访问 localhost:8088 访问即可

-v $PWD/dist:/usr/share/nginx/html/dist  ### 打包后文件映射关系

$PWD/conf.d:/etc/nginx/conf.d  ### 配置文件映射关系
```



###### 打包后404的效果

```js
 # 此处的 @router 实际上是引用下面的转发，否则在 Vue 路由刷新时可能会抛出 404
      try_files $uri $uri/ /index.html;
```

###### 跨域的问题

vue.config.js中配置代理,上线后跨域失效

```json
devServer: {
        host: '127.0.0.1',
        port: 8080,
        open: true,// vue项目启动时自动打开浏览器
        proxy: {
            '/api': { // '/api'是代理标识，用于告诉node，url前面是/api的就是使用代理的
                target: "http://127.0.0.1:3000", //目标地址，一般是指后台服务器地址
                changeOrigin: true, //是否跨域
                pathRewrite: { // pathRewrite 的作用是把实际Request Url中的'/api'用""代替
                    '^/api': "" 
                }
            }
        }
    }
```

借助于nginx解决上线后APi接口代理

```json
server{
        # 需要被监听的端口号，前提是此端口号没有被占用，否则在重启 Nginx 时会报错
    listen       80;
    # 服务名称，无所谓
    server_name  localhost;

    # 上述端口指向的根目录
    root /usr/share/nginx/html/dist;
    # 项目根目录中指向项目首页

    charset utf-8;
    client_max_body_size 20m;
    client_body_buffer_size 128k;

    # 根请求会指向的页面
    location / {
      # 请求指向的首页
      index index.html;
      # 此处的 @router 实际上是引用下面的转发，否则在 Vue 路由刷新时可能会抛出 404
      try_files $uri $uri/ /index.html;
    }
    location /api {
      proxy_pass http://192.168.3.176:3000;
      rewrite  ^/api/(.*)$ /$1 break;
    }

    # location /bigData/image {
    #     alias /home/lighthouse/wwwroot/intelligent-sanitation-system/dist/bigData/image/;
    #     allow all;
    #     autoindex on;
    #     charset utf-8;
    # }

    # 由于路由的资源不一定是真实的路径，无法找到具体文件
    # 所以需要将请求重写到 index.html 中，然后交给真正的 Vue 路由处理请求资源
}
```



##### 部署`express`

> ###### 大致步骤
>
> 1. 先创建好`express`项目
> 2. 在项目中创建`Dockerfile`文件
> 3. 构建镜像
> 4. 创建并运行容器

###### 创建`Dockerfile`文件

```shell
# Dockerfile文件内容
# 使用 Node.js 官方镜像作为基础镜像
FROM node:14

# 设置工作目录
WORKDIR /app

# 复制项目文件到容器中
COPY package*.json ./
RUN npm install

# 复制项目文件到容器中
COPY . .

# 暴露应用的端口
EXPOSE 3000

# 启动 Express 项目
CMD ["npm", "start"]
```

##### 构建镜像

```shell
# 在项目目录下运行, (.)点表示当前目录，
docker build -t 镜像名称 .
```

##### 创建容器并运行

```shell
docker run -p 3000:3000 -d 镜像名称
```



