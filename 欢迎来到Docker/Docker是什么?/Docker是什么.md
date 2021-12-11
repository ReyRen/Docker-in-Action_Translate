​	如果你选择这本书，说明你已经听说过Docker了，它就是用来构建、安装部署和运行程序的开源项目。它是一个命令行工具程序，一个后台进程，甚至是一些列的远程服务用来能简化软件的安装移除等的工作。它完成这一切，就是用到了操作系统的技术`containers`。

## “Hello, World”

​	开始前，你先下载了[Docker](https://docs.docker.com/install/)。一旦你有网络并且下载好了Docker, 那么在你的命令行输入终端中输入:

```
docker run dockerinaction/hello_world
```

​	执行之后，Docker就开始引入眼帘。它将下载各种需要的组件并且最终输出“hello world”。如果你再次运行它，那么只会输出“hello world”了。接下来我们拆解这个命令不同的部分，和分析发生的事情。

​	第一，执行`docker run`是告诉Docker你想要触发下图的这个执行流程：在容器中安装和运行一个程序。

![](http://www.reyren.cn:88/Docker-In-Action/welcome-to-docker/what-is-docker/docker-run.png)

​	第二，指定你想在容器中运行的Docker程序。在这个例子中，这个程序就是`dockerinaction/hello_world`。这个叫做`image`名。到目前为止，你可以认为image name就是你要运行的程序的名字。image是文件和元数据的合集。元数据包含了特殊要执行的程序和其他相关的配置细节。

​	第一次执行这个命令的时候，Docker必须确认`dockerinaction/hello_world`是否已经被下载了，如果发现本地没有，那么就会去`Docker HUb`寻找，`Docker Hub`是Docker Inc提供的开源共有镜像仓库。`Docker Hub`回应给本机Docker，说我有！然后就开始执行现在。

​	一旦下载成功，Docker创建一个容器并且运行命令：

```
echo "hello world"
```

​	当输出完毕后，容器马上退出了，并且被标记为STOP状态。Restart将会让它重新启动。

​	当你再次运行上面的命令的时候，Docker将会再次检测image是否存在，发现这次存在了，然后马上再次启动一个新的容器，执行相应的应用程序。

![](http://www.reyren.cn:88/Docker-In-Action/welcome-to-docker/what-is-docker/docker-run-2.png)

​	这里需要强调的是，第二次执行使用的是和第一次相同的image， 但是创建的是不同的容器，所以通过这种方式执行100次，会创建出100个不同的容器。因此有时候，你需要进行适当的清理。

​	恭喜，你已经成为一个Docker使用者了，就是这么简单。但是这也需要了解你所运行的程序。比如说运行一个web程序在容器里，如果你不知道这个长期运行的程序所需要监听的TCP端口是80，那么你可能就不知道如何构建这个Docker命令。



## 容器

​	历史上，UNIX风格的操作系统使用术语Jail来描述一个修改过的运行时环境，它限制了Jail程序可以访问的资源范围。Jail的功能可以回溯到1979年，从那时起一直在发展。2005年，随着SUN公司的Solaris 10和Solaris Containers的发布，Container就称为此类运行时环境的首选属于。目标已经从限制文件系统范围扩展到在明确说明下将进程与所有资源隔离。

​	长期以来，使用容器一直是一个很好的实践。但是手动构建容器可能是一个非常具有挑战性的工作，并且很有可能因为一些配置的错误，从而陷入到一些系统性的安全中。Docker的出现就是为了解决这些问题的，只要使用Docker运行的软件，就都是在容器中了。Docker 使用现有的容器引擎来提供根据最佳实践构建的一致容器。

​	使用Docker，您可以不必紧紧跟随构建强大容器的技术发展，而让Docker帮你处理这些工作。随着Docker及其容器引擎的改进，你将获得最新最好的隔离功能。



## 容器不是虚拟化

