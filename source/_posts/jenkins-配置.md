---
title: jenkins 配置
date: 2021-01-31 23:07:10
tags:
---

Jenkins 是一个可扩展的持续集成引擎。接下来，我将通过两篇文章来全面介绍jenkins的基本概念，安装、配置、自动构建、监控、部署，以及在生产环境的高级应用。

## Jenkins 介绍

主要用途：

1、持续、自动地构建/测试软件项目。

2、监控一些定时执行的任务。

Jenkins特性：

1、易于安装-本文提供四种配置方式。

2、易于配置-所有配置都是通过其提供的web界面实现。

3、集成RSS/E-mail通过RSS发布构建结果或当构建完成时通过e-mail通知。

4、生成JUnit/TestNG测试报告。

5、分布式构建支持Jenkins能够让多台计算机一起构建/测试。

6、文件识别:Jenkins能够跟踪哪次构建生成哪些jar，哪次构建使用哪个版本的jar等。

7、插件支持:支持扩展插件，你可以开发适合自己团队使用的工具。

8、Jenkins一切配置都可以在web界面上完成。有些配置如MAVEN_HOME和Email，只需要配置一次，所有的项目就都能用。当然也可以通过修改XML进行配置。

9、支持Maven的模块(Module)，Jenkins对Maven做了优化，因此它能自动识别Module，每个Module可以配置成一个job。相当灵活。

10、测试报告聚合，所有模块的测试报告都被聚合在一起，结果一目了然，使用其他CI，这几乎是件不可能完成的任务。

11、构件指纹(artifact fingerprint)，每次build的结果构件都被很好的自动管理，无需任何配置就可以方便的浏览下载。

## Jenkins安装
1.下载安装
2.配置

```
sudo vi /etc/sysconfig/jenkins

```

JENKINS_USER="root" ## 原值 "jenkins" 必须修改，否则权限不足

JENKINS_PORT="8080" ## 原值 "8080" 可以不修改

Jenkins安装目录: /usr/lib/jenkins

Jenkins工作目录: /var/lib/jenkins(对应于环境变量 JENKINS_HOME)

构建项目源码目录：/var/lib/jenkins/workspace

日志默认路径：/var/log/jenkins/jenkins.log

启动

sudo systemctl enable jenkins

sudo systemctl restart jenkins

或

service jenkins start

基本配置

3、登录

登录地址：http://localhost:8080/    

初始密码获取：

sudo cat /var/lib/jenkins/secrets/initialAdminPassword

密码修改：

登录主页-用户-选择用户（admin）-设置-输入新密码

4、Jenkins插件安装

Jenkins 插件管理器允许您安装新的插件，和更新您Jenkins服务器上的插件。管理者将连接到联机资料库，检索可用的和已更新的插件。如果您的Jenkins服务器 无法直接连接到外部资源，您可以从Jenkins网站上下载。

在已运行的Jenkins主页中，点击左侧的系统管理—>管理插件->可选插件
安装插件：Git plugin，ssh plugin

5、JDK、Maven、Git配置

jenkins 构建，依赖JDK、Maven、Git 插件，因此项目构建前需要对插件做配置，配置流程如下：

进入 系统管理->全局工具配置：

Maven别名：用户自定义，便于标识就可以。

MAVEN_HOME：这个是本机MAVEN的安装路径。

git安装完毕后，此处git默认配置完毕.

JDK别名：用户自定义，便于标识就可以。

JDK_HOME：这个是本机JDK的安装路径。见上文第二部分 JDK安装。（错误的路径会有红字提示你的）

自动安装：不推荐这个选项

## 源码构建、打包、部署、运行

（基于springboot，maven，git，ssh）

1、 新建项目

新建-> 输入一个任务名->构建一个maven项目。

源码管理：
Repository URL：

git@github.com:wzjgn/xinwei-example.git

或

https://github.com/wzjgn/xinwei-example.git

如果Repository URL 地址有误，此处会报错。

Add ，添加源码访问凭证。

本案例通过ssh key 访问git源码服务器。因此，在此处添加ssh私钥

构建触发器：

选择“Build whenever a SNAPSHOT dependency is built” ：依赖于快照的构建，当git有修改时就构建项目。

Build periodically ：此选项仅仅通知Jenkins按指定的频率对项目进行构建，而不管SCM是否有变化。如果想在这个Job中运行一些测试用例的话，它就很有帮助。

Poll SCM ：这是CI 系统中常见的选项。当您选择此选项，您可以指定一个定时作业表达式来定义Jenkins每隔多久检查一下您源代码仓库的变化。如果发现变化，就执行一次构建。例如，表达式中填写0,15,30,45 * * * *将使Jenkins每隔15分钟就检查一次您源码仓库的变化。

构建：

默认Jenkins在workspace目录下面找到pom.xml文件。

Post Steps：

配置构建成功后的动作，添加shell。

脚本作用：

1、关闭原应用程序进程

2、解压新生成的tar包

3、wrapper启动重构后的应用

#### 构建

手动触发项目构建流程：

构建状态:下图中分级符号概述了一个Job新近一次构建会产生的四种可能的状态：

Successful:完成构建，且被认为是稳定的。

Unstable:完成构建，但被认为不稳定。

Failed:构建失败。

Disabled:构建已禁用。

构建稳定性: 当一个Job中构建已完成并生成了一个未发布的目标构件，如果您准备评估此次构建的稳定性，Jenkins会基于一些后处理器任务为构建发布一个稳健指数 (从0-100 )，这些任务一般以插件的方式实现。它们可能包括单元测试(JUnit)、覆盖率(Cobertura )和静态代码分析(FindBugs)。分数越高，表明构建越稳定。下图中分级符号概述了稳定性的评分范围。任何构建作业的状态(总分100)低于80分就是不稳定的。

双击上图 项目名称，进入该项目的控制台：

在控制台点击“立即构建”，BuildHistory 列表第一条记录展示项目当前的构建进度条。点击 “项目构建进度条”，进入监控页面。查看构建过程，日志。

构建完毕，访问地址

http://localhost/