	如果你选择这本书，说明你已经听说过Docker了，它就是用来构建、安装部署和运行程序的开源项目。它是一个命令行工具程序，一个后台进程，甚至是一些列的远程服务用来能简化软件的安装移除等的工作。它完成这一切，就是用到了操作系统的技术`containers`。

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

​	在这个云原生的时代，人们倾向于将虚拟机视为部署单元，其中部署单个进程意味着创建一个完整的网络附加的虚拟机。虚拟机提供虚拟硬件。它们需要很长时间来创造并需要大量的资源开销，因为除了使用的软件之外，它还承载了整个操作系统的运行。虚拟机一旦全部都启动并且进程运行起来，就会有很好的效果。但是这个启动的过程就是比较耗时的，这种情况就不能够满足对时间要求较苛刻的部署场景。

​	不同于虚拟机，Docker容器没有使用任何的硬件虚拟化。在Docker中运行的程序直接与宿主机内核接口交互。许多程序可以隔绝运行而无需运行冗余操作系统或忍受完整启动程序所带来的延迟。这是一个重要的区别。Docker不是一个硬件虚拟化技术。的确，它帮助你使用已经存在于操作系统内核的容器技术。

​	虚拟机提供硬件的抽象，所以可以运行完整的全新的操作系统。容器只是操作系统的一个功能。所以Docker容器是可以运行在任何一个虚拟机上的。Mac和Windows的Docker用户都是运行Docker是在虚拟机中的。

## 容器中的程序是隔离的

​	容器和隔绝的特性已经存在了十几年。Docker使用Linux的namespace和CGroups技术，这两个技术是从2007年进入linux的东西。Docker本身是不提供容器技术的，但是它主要将这个技术以更加简单的使用方式反馈给用户。为了理解在操作系统上的容器是什么样子，我们先建立一个基准。下图展示了一个运行在简化了的计算机架构下的基础案例。

![ ](http://www.reyren.cn:88/Docker-In-Action/welcome-to-docker/what-is-docker/basline-what-is-container.png)

​	注意这里的CLI是运行在所谓的用户内存空间的，就像其他在操作系统上运行的应用程序一样。一般情况下，运行在用户空间的程序是没法修

改内核空间的东西的。广义讲，操作系统就是用户程序和硬件之间的接口。

![](http://www.reyren.cn:88/Docker-In-Action/welcome-to-docker/what-is-docker/Docker-run-three-containers.png)

​	从图1.4中可以看出，Docker的运行就是在用户空间的两个程序。第一个是容器引擎。如果安装恰当的化，这个程序会是一直运行的。第二个是Docker CLI。这个是用户交互的docker程序。

​	图1.4也展示了三个容器。每一个容器都以Docker引擎的子进程运行，并且用容器进行包裹。分配的进程只运行在用户空间的自己的内存子系统中。在容器中运行的程序也只能访问容器域内的内存和资源。

​	Docker的构建使用了10个系统主要的特性，在Part I中会通过Docker命令来解释这些特性：

- PID namespace---进程识别号和能力
- UTS namespace---主机和域名
- MNT namespace---文件系统访问和结构
- IPC namespace---共享内存间进程交流
- NET namespace---网络访问和结构
- USR namespace---用户名和识别号
- chroot syscall---控制根文件系统位置
- cgroupfs---资源保护
- CAP drop---操作系统特性限制
- Security modules---强制访问控制

​	Docker使用这些操作系统特性在运行时创建了容器，但是却使用了另外一些列的技术来打包容器。

## 容器运输

​	你可想象docker容器就是物理环境的集装箱。在这个盒子中运行一个应用和它的所有依赖（除了操作系统的内核）。就像货轮、火车等运送集装箱一样，docker也是可以运行、拷贝和分发容器。Docker完成了传统意义上的容器隐喻包括一种打包和分发软件的方式。担任运输容器角色的组件就是image。

​	通俗的讲，一个Docker image是一捆可以在容器内可以被运行的文件快照。你可以通过以用一个镜像创建多个容器。但是，虽然使用的是同一个镜像，但是这些容器之间并不会共享不同容器对镜像内文件的修改。image是在Docker生态中可移动的。

​	Docker提供了一些列的基础设施组件来简化Docker image的分发。这些组件是registries和indexes。
