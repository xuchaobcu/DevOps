# Centos 7 以上版本安装 Docker
***需求来源：***

学习docker基本使用。



***参考文档：***

https://blog.csdn.net/wangfei0904306/article/details/62046753



***自我实践：***

**实践步骤：**

1. 准备虚拟机，推荐使用Centos 7以上版本，默认root账户安装

2. 安装docker

3. 设置mirror

4. 开放管理端口映射

5. 启动docker

6. 测试docker

   

**上机操作：**

1. **准备虚拟机、使用Centos 7.X 版本**

​       lsb_release -a

<img src="./picture/01/01 检查当前操作系统版本信息.png"/>



2. **安装Docker**

-  更新虚拟机Centos 7.X的yum源

​       yum -y update

<img src="./picture/01/02 更新本地yum源.png"/>     

- 设置Docker CE资源库

   sudo yum install -y yum-utils 

  sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

<img src="./picture/01/03 设置Docker CE资源库.png"/>

sudo yum makecache fast 

<img src="./picture/01/04 设置Docker CE资源库2.png"/>

(这步如果出现Timeout, 则执行如下语句将镜像转为清华镜像)

sudo sed -i 's+download-stage.docker.com+mirrors.tuna.tsinghua.edu.cn/docker-ce+' /etc/yum.repos.d/docker-ce.repo  

 sudo sed -i 's+download.docker.com+mirrors.tuna.tsinghua.edu.cn/docker-ce+' /etc/yum.repos.d/docker-ce.repo  

- 安装docker

  sudo yum -y install docker-ce 

  <img src="./picture/01/05 安装Docker.png"/>

- 启动docker

  sudo systemctl start docker 

  <img src="./picture/01/06 启动Docker.png"/>

3. **设置mirror**

    https://lug.ustc.edu.cn/wiki/mirrors/help/docker

       新版的 Docker 使用 /etc/docker/daemon.json 来配置 Daemon
       在该配置文件中加入（没有该文件的话，请先创建一个）：
       {
         "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
       }
       
       如果docker不能pull，设置其它镜像参考：http://www.datastart.cn/tech/2016/09/28/docker-mirror.html
   <img src="./picture/01/07 设置mirror.png"/>

4. **开放管理端口映射：**

管理端口在 /lib/systemd/system/docker.service 文件中
将其中第11行的 ExecStart=/usr/bin/dockerd 替换为：
**ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock -H tcp://0.0.0.0:7654**
（此处默认2375为主管理端口，unix:///var/run/docker.sock用于本地管理，7654是备用的端口）

<img src="./picture/01/08 开放管理映射端口.png"/>

将管理地址写入 /etc/profile

echo 'export DOCKER_HOST=tcp://0.0.0.0:2375' >> /etc/profile

使profile生效

source /etc/profile



5. **启动Docker：**

systemctl daemon-reload && service docker restart 

<img src="./picture/01/09 重新加载并重启docker.png"/>



6. **测试Docker：**

   sudo docker run hello-world

   <img src="./picture/01/10 测试docker.png"/>

   

大功告成。