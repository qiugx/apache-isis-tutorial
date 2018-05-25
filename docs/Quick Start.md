# 快速开始

本章节将从一个简单的 todoapp 应用开始，介绍如何快速开始来检出、编译、运行一个 Apache Isis 应用。

## todoapp 示例简介

todoapp 示例应用程序，它是一个相当完整的应用程序，用于跟踪代办事项，基于单个域类`ToDoItem`和存储库`ToDoItems`。

虽然该应用没有囊括所有的内容，但应用程序仍然展示了大量的 Apache Isis 的能力：
贡献的动作/集合/属性的使用，由`ToDoItemContributions`来演示; 
视图模型由`ToDoItemsByCategoryViewModel`和`ToDoItemsByDateRangeViewModel`来演示; 
仪表盘由`ToDoAppDashboard`演示; 
通过`DemoDomainEventSubscriptions`服务演示内部事件总线的使用。

领域模型如下：

```
[dom.ToDoItems]->[dom.ToDoItemRepository]
[dom.ToDoItemRepository]->[dom.ToDoItemRepositoryImpl]
[dom.ToDoItemRepositoryImpl]^-[dom.ToDoItemRepositoryImplUsingJdoQl]
[dom.ToDoItemRepositoryImpl]^-[dom.ToDoItemRepositoryImplUsingTypesafeQueries]
```

该应用程序还集成了许多 [Isis Addons](https://www.isisaddons.org/) 插件，
如[安全](https://github.com/isisaddons/isis-module-security)、
[命令分析](https://github.com/isisaddons/isis-module-command)、
[审计](https://github.com/isisaddons/isis-module-audit)、
[事件发布](https://github.com/isisaddons/isis-module-publishing)。 

虽然  Isis Addons 插件不是 Apache 软件基金会（简称“ASF”）的一部分，但它们都是在 Apache License 2.0 下授权的，并由 Apache Isis 提交者维护。


该 todoapp 应用托管在 https://github.com/isisaddons/isis-app-todoapp，你可以随意检出该源码进行学习。

## 配置环境

要想使用该应用实例，至少确保具备如下环境：

* JDK：Java 7+
* Maven：3.0.+
* Git

## 检出代码

```
git clone  https://github.com/isisaddons/isis-app-todoapp.git
```

## 构建应用

切换到应用程序的根目录，只需使用下面命令来构建：

```
cd isis-app-todoap
mvn clean install
```

## 运行应用

todoapp 应用程序会生成单个 WAR 文件，配置为运行 Wicket 查看器和 Restful Objects 查看器。
应用程序还将 JDO Objectstore 配置为使用内存中的 HSQLDB 连接。

构建应用程序后，您可以在 Maven 托管的 Jetty 实例中运行该 WAR：

```
cd webapp
mvn jetty:run -D jetty.port=9090
```

在上面，我们通过了一个属性来配置一个不同于默认端口（8080）的端口。看到如下输出，说明应用启动成功：

```
********************************************************************
*** WARNING: Wicket is running in DEVELOPMENT mode.              ***
***                               ^^^^^^^^^^^                    ***
*** Do NOT deploy to your live server(s) without changing this.  ***
*** See Application#getConfigurationType() for more information. ***
********************************************************************
十一月 29, 2016 11:06:40 下午 org.webjars.servlet.WebjarsServlet init
信息: WebjarsServlet cache enabled: true
十一月 29, 2016 11:06:40 下午 org.webjars.servlet.WebjarsServlet init
信息: WebjarsServlet initialization completed
[INFO] Started o.e.j.m.p.JettyWebAppContext@124dac75{/,[file:///D:/workspaceGit/isis-app-todoapp/webapp/src/main/webapp/
, jar:file:///D:/workspaceMaven/org/webjars/jquery-ui/1.11.4/jquery-ui-1.11.4.jar!/META-INF/resources, jar:file:///D:/wo
rkspaceMaven/org/webjars/bootstrap/3.3.6/bootstrap-3.3.6.jar!/META-INF/resources, jar:file:///D:/workspaceMaven/org/webj
ars/select2/3.5.2/select2-3.5.2.jar!/META-INF/resources, jar:file:///D:/workspaceMaven/org/webjars/Eonasdan-bootstrap-da
tetimepicker/4.15.35/Eonasdan-bootstrap-datetimepicker-4.15.35.jar!/META-INF/resources, jar:file:///D:/workspaceMaven/or
g/webjars/momentjs/2.10.3/momentjs-2.10.3.jar!/META-INF/resources, jar:file:///D:/workspaceMaven/org/webjars/font-awesom
e/4.4.0/font-awesome-4.4.0.jar!/META-INF/resources, jar:file:///D:/workspaceMaven/org/webjars/bower/summernote/0.7.0/sum
mernote-0.7.0.jar!/META-INF/resources, jar:file:///D:/workspaceMaven/org/webjars/jquery/1.11.1/jquery-1.11.1.jar!/META-I
NF/resources, jar:file:///D:/workspaceMaven/org/webjars/animate.css/3.2.5/animate.css-3.2.5.jar!/META-INF/resources, jar
:file:///D:/workspaceMaven/org/webjars/modernizr/2.8.3/modernizr-2.8.3.jar!/META-INF/resources, jar:file:///D:/workspace
Maven/org/webjars/swagger-ui/2.1.3/swagger-ui-2.1.3.jar!/META-INF/resources],AVAILABLE}{file:///D:/workspaceGit/isis-app
-todoapp/webapp/src/main/webapp/}
[INFO] Started ServerConnector@3ff83cc8{HTTP/1.1,[http/1.1]}{0.0.0.0:9090}
[INFO] Started @26173ms
[INFO] Started Jetty Server
```


您也可以通过部署到独立的 servlet 容器（如 [Tomcat](http://tomcat.apache.org/)）来运行该应用程序。

## 使用应用

该应用程序提供了一个欢迎页面，用于解释生成的类和文件，并提供详细的指导和下一步该做什么。

应用程序本身配置使用 shiro 了保证安全性，在 `WEB-INF/shiro.ini` 配置文件中进行配置的。
要登录，请使用默认的账号密码 todoapp-admin/pass。


## 应用结构

如上所述，生成的应用是用于跟踪待办事项的完整的应用。
它由以下模块组成（使用 JDOQL 或 DataNucleus 的类型安全查询作为查询数据库的方法的查询）：

* todoapp：父模块 (聚合器) 。
* todoapp-app：应用程序清单，用于引导应用程序和集成测试。
* todoapp-canonical：定义一个 XSD 和代码生成相应的 DTO 类，用于定制的 REST 内容协商。
* todoapp-dom：领域对象模型，由`ToDoItem`和`ToDoItems`领域服务组成。还定义了`ToDoItemRepository`存储库类。
* todoapp-fixture：用于在演示或单元测试时初始化系统的领域对象固件。
* todoapp-integtests：从 UI 到数据库执行的端到端集成测试。
* todoapp-webapp：使用 Wicket 查看器或 RestfulObjects 查看器作为 webapp运行（使用 `web.xml`）。




