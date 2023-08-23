# Delivery_Platform
第一个可部署web学习项目，基于springboot
#### 方法1：
项目打包后，直接通过 nohup java -jar /usr/local/reggie_take_out/target/reggie_take_out-1.0-SNAPSHOT.jar &> /usr/local/reggie_take_out/target/reggie_take_out.log & 运行即可。
#### 方法2：
在服务器上搭建极狐Gitlab，将Java代码提交。  
在jenkins上配置：   
1. Source Code Management: 拉去代码的git仓库地址；
2. Build Triggers: 定时触发逻辑，例如dailyCI（H(1-10) 0 * * *）；
3. Pre Step: 将服务器上之前的jar包清理掉；
4. Build: 填写构建项目的根xml路径，例如pom.xml；
5. Post Steps: 将jar包从Jenkins服务器，传到项目服务端上，并通过方法1中命令运行。
   
**该方法好处：**
修改代码并上传git服务器后，可以手动触发构建，及时看到项目效果。
#### 方法3：
在方法2的基础上，可以将项目放到docker内：  
1. 区别1：Pre Step的清理还需要再加上docker stop, docker rm, docker rmi；
2. 区别2：Post Steps需要将jar包放在docker内的对应目录中，再通过dockerfile构建镜像（docker build, docker run）。
#### 方法4：
和方法3类似，也是用docker，但利用了docker的外挂目录：
1. Pre Step的清理只清理外挂目录，docker stop, 但不清理docker；
2. Post Steps不构建docker，只启动docker，docker start，jar包的启动逻辑写在docker内部。
  
**该方法好处：**
不需要总是清理、构建docker容器。
