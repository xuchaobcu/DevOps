# 基于SSH协议的Github访问设置说明
***需求来源：***

当频繁push修改后的代码到github仓库时，我每次都需要输入用户名和密码，感觉严重影响工作效率。



***解决办法：***

百度



***参考文档：***

https://blog.csdn.net/u012373815/article/details/53575362



***自我实践：***

**实践步骤：**

1. 设置本地Git客户端的user name 和email

2. 生成本地ssh密钥对

3. 在GIthub上设置ssh生成的 publick key

   ​

**上机操作：**

1. 设置本地Git客户端的user name 和email

​       git config --global user.name "github注册账号"

​       git config --global user.email "github注册邮箱"

​       ![01设置邮箱和账号](.\picture\01设置邮箱和账号.png)

