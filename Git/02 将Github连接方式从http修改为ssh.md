# 将Github连接方式从http修改为ssh
***需求来源：***

当频繁push修改后的代码到github仓库时，我每次都需要输入用户名和密码，感觉严重影响工作效率。



***解决办法：***

百度



***参考文档：***

https://blog.csdn.net/wozaixiaoximen/article/details/49487285



***自我实践：***

**实践步骤：**

1. 检查当前仓库的连接方式

2. 删除当前仓库的http连接方式

3. 修改当前仓库的连接方式为ssh

   ​

**上机操作：**

1. **检查当前仓库的连接方式**

​       git remote -v​

<img src="./picture/02/01 检查连接信息.png"/>



2. **删除仓库默认的http连接方式**

​      git remote rm origin

 <img src="./picture/02/02 删除连接信息.png"/>     



3. **设置仓库的连接方式为SSH**

   git remote add origin git@github.com:xuchaobcu/xuexi.git

   <img src="./picture/02/03 添加连接信息.png"/>

   ​




4.**验证实验结果：**

git remote -v

<img src="./picture/02/04 确认修改后的连接信息.png"/>

打完收工。