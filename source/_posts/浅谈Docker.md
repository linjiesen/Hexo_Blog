---
title: 浅谈Docker
date: 2019-04-05 16:03:15
tags: [Docker, 虚拟化]
categories: [云]
---
2013年发布至今， [Docker](https://www.docker.com/)一直广受瞩目，被认为可能会改变软件行业。今天就简单说说Docker.

# 一.Docker的优点

## 持续部署与测试

Docker在开发与运维的世界中具有极大的吸引力，因为它能保持跨环境的一致性。在开发与发布的生命周期中，不同的环境具有细微的不同，这些差异可能是由于不同安装包的版本和依赖关系引起的。然而，Docker可以通过确保从开发到产品发布整个过程环境的一致性来解决这个问题*Docker容器通过相关配置，保持容器内部所有的配置和依赖关系始终不变。最终，你可以在开发到产品发布的整个过程中使用相同的容器来确保没有任何差异或者人工干预。

使用Docker，你还可以确保开发者不需要配置完全相同的产品环境，他们可以在他们自己的系统上通过VirtualBox建立虚拟机来运行Docker容器。Docker的魅力在于它同样可以让你在亚马逊EC2实例上运行相同的容器。如果你需要在一个产品发布周期中完成一次升级，你可以很容易地将需要变更的东西放到Docker容器中，测试它们，并且使你已经存在的容器执行相同的变更。这种灵活性就是使用Docker的一个主要好处。和标准部署与集成过程一样，Docker可以让你构建、测试和发布镜像，这个镜像可以跨多个服务器进行部署。哪怕安装一个新的安全补丁，整个过程也是一样的。你可以安装补丁，然后测试它，并且将这个补丁发布到产品中。

## 多云平台

Docker最大的好处之一就是可移植性。在过去的几年里，所有主流的云计算提供商，包括亚马逊AWS和谷歌的GCP，都将Docker融入到他们的平台并增加了各自的支持。Docker容器能运行在亚马逊的EC2实例、谷歌的GCP实例、Rackspace服务器或者VirtualBox这些提供主机操作系统的平台上。举例来说，如果运行在亚马逊EC2实例上的Docker容器能够很容易地移植到其他几个平台上，比如说VirtualBox，并且达到类似的一致性和功能性，那这将允许你从基础设施层中抽象出来。除了AWS和GCP，Docker在其他不同的IaaS提供商也运行的非常好，例如微软的Azure、OpenStack和可以被具有不同配置的管理者所使用的Chef、Puppet、Ansible等。

## 环境标准化和版本控制

通过上面的讨论，Docker容器可以在不同的开发与产品发布生命周期中确保一致性，进而标准化你的环境。除此之外，Docker容器还可以像git仓库一样，可以让你提交变更到Docker镜像中并通过不同的版本来管理它们。设想如果你因为完成了一个组件的升级而导致你整个环境都损坏了，Docker可以让你轻松地回滚到这个镜像的前一个版本。这整个过程可以在几分钟内完成，如果和虚拟机的备份或者镜像创建流程对比，那Docker算相当快的，它可以让你快速地进行复制和实现冗余。此外，启动Docker就和运行一个进程一样快。

## 隔离性

Docker可以确保你的应用程序与资源是分隔开的。几个月前，Gartner发表了一篇报告，这份报告说明了运行Docker 容器进行资源隔离的效果和虚拟机（VM）管理程序一样的好，但是管理与控制方面还需要进行完善。

我们考虑这样一个场景，你在你的虚拟机中运行了很多应用程序，这些应用程序包括团队协作软件（例如Confluence）、问题追踪软件（例如JIRA）、集中身份管理系统（例如Crowd）等等。由于这些软件运行在不同的端口上，所以你必须使用Apache或者Nginx来做反向代理。到目前为止，一切都很正常，但是随着你的环境向前推进，你需要在你现有的环境中配置一个内容管理系统（例如Alfresco）。这时候有个问题发生了，这个软件需要一个不同版本的Apache Tomcat，为了满足这个需求，你只能将你现有的软件迁移到另一个版本的Tomcat上，或者找到适合你现有Tomcat的内容管理系统（Alfresco）版本。

对于上述场景，使用Docker就不用做这些事情了。Docker能够确保每个容器都拥有自己的资源，并且和其他容器是隔离的。你可以用不同的容器来运行使用不同堆栈的应用程序。除此之外，如果你想在服务器上直接删除一些应用程序是比较困难的，因为这样可能引发依赖关系冲突。而Docker可以帮你确保应用程序被完全清除，因为不同的应用程序运行在不同的容器上，如果你不在需要一款应用程序，那你可以简单地通过删除容器来删除这个应用程序，并且在你的宿主机操作系统上不会留下任何的临时文件或者配置文件。

除了上述好处，Docker还能确保每个应用程序只使用分配给它的资源（包括CPU、内存和磁盘空间）。一个特殊的软件将不会使用你全部的可用资源，要不然这将导致性能降低，甚至让其他应用程序完全停止工作。

## 安全性

如上所述，Gartner也承认Docker正在快速地发展。从安全角度来看，Docker确保运行在容器中的应用程序和其他容器中的应用程序是完全分隔与隔离的，在通信流量和管理上赋予你完全的控制权。Docker容器不能窥视运行在其他容器中的进程。从体系结构角度来看，每个容器只使用着自己的资源（从进程到网络堆栈）。

作为紧固安全的一种手段，Docker将宿主机操作系统上的敏感挂载点（例如/proc和/sys）作为只读挂载点，并且使用一种写时复制系统来确保容器不能读取其他容器的数据。Docker也限制了宿主机操作系统上的一些系统调用，并且和SELinux与AppArmor一起运行的很好。此外，在Docker Hub上可以使用的Docker镜像都通过数字签名来确保其可靠性。由于Docker容器是隔离的，并且资源是受限制的，所以即使你其中一个应用程序被黑，也不会影响运行在其它Docker容器上的应用程序。

## 环境配置

软件开发的第一件事就是配置开发环境,用户计算机的环境都不相同，你如何使自己开发的软件，能在别的机器跑起来？用户必须保证两件事：操作系统的设置，各种库和组件的安装。只有它们都正确，软件才能运行。Docker可以把软件和开发环境一并打包.

# 二.虚拟机
虚拟机（virtual machine）就是带环境安装的一种解决方案。它可以在一种操作系统里面运行另一种操作系统，比如在 Windows 系统里面运行 Linux 系统。应用程序对此毫无感知，因为虚拟机看上去跟真实系统一模一样，而对于底层系统来说，虚拟机就是一个普通文件，不需要了就删掉，对其他部分毫无影响。

虽然用户可以通过虚拟机还原软件的原始环境。但是，这个方案有几个缺点。

1. 资源占用多

虚拟机会独占一部分内存和硬盘空间。它运行的时候，其他程序就不能使用这些资源了。哪怕虚拟机里面的应用程序，真正使用的内存只有 1MB，虚拟机依然需要几百 MB 的内存才能运行。

2. 冗余步骤多

虚拟机是完整的操作系统，一些系统级别的操作步骤，往往无法跳过，比如用户登录。

3. 启动慢

    启动操作系统需要多久，启动虚拟机就需要多久。可能要等几分钟，应
用程序才能真正运行。

# 三.Linux 容器
由于虚拟机存在这些缺点，Linux 发展出了另一种虚拟化技术：Linux 容器（Linux Containers，缩写为 LXC）。

Linux 容器不是模拟一个完整的操作系统，而是对进程进行隔离。或者说，在正常进程的外面套了一个保护层。对于容器里面的进程来说，它接触到的各种资源都是虚拟的，从而实现与底层系统的隔离。

由于容器是进程级别的，相比虚拟机有很多优势。

1. 启动快

容器里面的应用，直接就是底层系统的一个进程，而不是虚拟机内部的进程。所以，启动容器相当于启动本机的一个进程，而不是启动一个操作系统，速度就快很多。

2. 资源占用少

容器只占用需要的资源，不占用那些没有用到的资源；虚拟机由于是完整的操作系统，不可避免要占用所有资源。另外，多个容器可以共享资源，虚拟机都是独享资源。

3. 体积小

容器只要包含用到的组件即可，而虚拟机是整个操作系统的打包，所以容器文件比虚拟机文件要小很多。

总之，容器有点像轻量级的虚拟机，能够提供虚拟化的环境，但是成本开销小得多。

# 四. Docker是什么?


> Docker is an open-source project that automates the deployment of applications inside software containers, by providing an additional layer of abstraction and automation of operating-system-level virtualization on Linux. Docker uses the resource isolation features of the Linux kernel such as cgroups and kernel namespaces, and a union-capable filesystem such as aufs and others to allow independent “containers” to run within a single Linux instance, avoiding the overhead of starting and maintaining virtual machines.Docker是一个开放源代码软件项目，让应用程序布署在软件容器下的工作可以自动化进行，借此在Linux操作系统上，提供一个额外的软件抽象层，以及操作系统层虚拟化的自动管理机制。Docker利用Linux核心中的资源分离机制，例如cgroups，以及Linux核心命名空间（name space），来建立独立的软件容器（containers）。这可以在单一Linux实体下运作，避免启动一个虚拟机器造成的额外负担。——摘自维基百科

# 五. 安装Docker
Docker包括两个版本,docker ce(Community Edition)社区版和
docker ee(Enterprise Edition)企业版.本篇所有内容只针对社区版.

Docker的安装方式请参考[官方文档](https://docs.docker.com/install/overview/)

安装完成执行`docker`命令会报 `permission denied` 没权限的错误

一些Windowsers会说用`root`用户啊......另一些有基本常识,知道不应该使用`root`的人可能会说,
那用`sudo docker`吧,这两种方法都是不对的,或者说不合适.

说使用`root`的人，应该回去好好学习一下 `Linux` 权限常识。一般 不应该直接使用 `root` 用户，直接使用 `root` 用户不仅仅是严重的违反了安全规范，而且也极容易造成操作事故。这不是 `Windows` 世界，`Linux/Unix` 世界是有严格的权限要求的，只应该使用最小的权限做事情。如果还不熟悉 `Linux` 权限机制，那就去学习一下，不要把 `Windows `的坏毛病带过来。

说使用 `sudo docker` 的人，思路是对的，因为理解了平时操作应该使用普通用户，只有在需要的时候，才 `sudo` 提升权限进行操作。但是问题就在这个需要二字上，事实上，不需要 `root` 权限就可以执行 `docker` 命令。

其实如果看过官方安装文档的话都会知道，只需要将操作 `docker` 的用户，加入 `docker` 组，那么该用户既拥有了操作 `docker` 的权限。
因此,只需要执行:

```
sudo usermod -aG docker $USER
```
把当前用户加入到`docker`组, 退出重新登录系统后,执行`docker info` 看一下,会发现可以不用`sudo docker`来执行`docker`命令了

如果需要添加别的用户，将其中的 `$USER `换成对应的用户名即可。

将用户添加到 `docker` 组，可以避免 root 权限误操作的问题，但是由于 dockerd 引擎是运行在 root 用户下的，而 docker 组成员有权限指挥 dockerd 引擎来做很多事情，因此，该用户实际上是拥有了 root 的权限的。因此不要误解了将当前用户加入 docker 组的初衷，这和赋予用户 sudo 权力是一样的，可不是说这个用户就没有 root 权限了。这样做，只
是不再需要使用 sudo 了，也降低了使用 sudo 时误操作的可能。

Docker是C-S架构,需要在本地先启动docker服务.
```
# service 命令的用法
$ sudo service docker start

# systemctl 命令的用法
$ sudo systemctl start docker
```

# 六.image文件

Docker 把应用程序及其依赖，打包在 image 文件里面。只有通过这个文件，才能生成 Docker 容器。image 文件可以看作是容器的模板。Docker 根据 image 文件生成容器的实例。同一个 image 文件，可以生成多个同时运行的容器实例。

image 是二进制文件。实际开发中，一个 image 文件往往通过继承另一个 image 文件，加上一些个性化设置而生成。

对Docker images的操作
```
# 列出本机的所有 image 文件
docker images
# 删除 image 文件
docker rmi xxx
```

# 七.Hello World!

docker 官方制作了Hello World的镜像.使用
```
docker run hello-world
```
docker会先检查本地有没有`hello-world`镜像,没有的话会从官方仓库中拉取.

##  docker pull 慢怎么解决

首先，要“感谢”伟大的墙。

然后，我们可以使用 Docker 镜像加速器来解决这个问题，加速器就是镜像、代理的概念。国内有不少机构提供了免费的加速器以方便大家使用，这里列出一些常用的加速器服务：

+ Docker 官方的中国镜像加速器：从2017年6月9日起，Docker 官方提供了在中国的加速器，以解决墙的问题。不用注册，直接使用加速器地址：https://registry.docker-cn.com 即可。
+ 中国科技大学的镜像加速器：中科大的加速器不用注册，直接使用地址 https://docker.mirrors.ustc.edu.cn/ 配置加速器即可。进一步的信息可以访问：http://mirrors.ustc.edu.cn/help/dockerhub.html?highlight=docker
+ 阿里云加速器：注册阿里云开发账户(免费的)后，访问这个链接就可以看到加速器地址： https://cr.console.aliyun.com/#/accelerator
+ DaoCloud 加速器：注册 DaoCloud 账户(支持微信登录)，然后访问： https://www.daocloud.io/mirror#accelerator-doc

## 访问官方文档很慢怎么办

再次感谢伟大的 **墙**
我们可以本地运行 Docker 官方文档的网站，以 docker 的方式：
```
$ docker run -d -p 80:4000 docs/docker.github.io
```
这样访问 Docker 宿主的 `80` 端口，如 http://localhost，就会看到官网文档了。

# 八. Docker常用命令

## 基础类

1. 查看Docker信息
```
# 查看docker版本
docker version
# 显示docker系统的信息
docker info
# 日志信息
docker logs
# 故障检查
docker status
```
2. 本地镜像

查看本地镜像
> docker images
```
(base)  alroy@Alan  ~  docker images
REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
nginx                        latest              27a188018e18        2 weeks ago         109MB
ubuntu                       latest              94e814e2efa8        7 weeks ago         88.9MB
hello-world                  latest              fce289e99eb9        4 months ago        1.84kB
zsnmwy/bilihelper            latest              b108cce8590c        6 months ago        96.3MB
zsnmwy/bilibili-live-tools   latest              e2b0e619d00a        9 months ago        112MB
```
删除本地镜像
> docker rmi hello-world 
```
(base)  alroy@Alan  ~  docker rmi hello-world
Error response from daemon: conflict: unable to remove repository reference "hello-world" (must force) - container 93b3b3af6661 is using its referenced image fce289e99eb9

```
正在运行的容器使用的镜像无法删除,想删除需先停止容器再删除镜像

3. 查看镜像详情
> docker inspect [ 镜像名 or 镜像 id ] 

4. 打包本地镜像, 使用压缩包来完成迁移

> docker save [ 镜像名 ] > [ 文件路径 ]

```
# 默认为文件流输出
docker save ubuntu > /home/alroy/ubuntu.img

# 或者使用 '-o' 选项指定输出文件路径
docker save -o ubuntu > /home/alroy/ubuntu.img
```

5. 导入镜像压缩包
> docker load < [ 文件路径 ]
```
(base)  ✘ alroy@Alan  ~  docker load -i  /home/alroy/ubuntu.img 
Loaded image: ubuntu:latest
```
6.  修改镜像tag
> docker tag [ 镜像名 or 镜像 id ] [ 新镜像名 ]:[ 新 tag ]

```
 docker tag hello-world:latest hello-world:test
```
7. 日志

> docker logs -f <容器名orID>

journalctl 日志工具使用

```
# 最后行数的日志
journalctl -n
# 详细信息
journalctl -f
# 本次启动后的所有日志
journalctl -b
# 查看启动记录
journalctl --list-boots
# 查看某次运行过程中的日志
sudo journalctl -b [启动顺序号，或者启动hash]
# 查看记录中指定单元 docker.service 的日志
journalctl -u docker.service
```

8. 查看容器信息

```
# 查看当前运行的容器
docker ps
# 查看全部容器
docker ps -a
# 查看全部容器的id和信息
docker ps -a -q
# 查看全部容器占用的空间
docker ps -as
# 查看一个正在运行容器进程，支持 ps 命令参数
docker top
# 查看容器的示例id
sudo docker inspect -f  '{{.Id}}' [id]
# 检查镜像或者容器的参数，默认返回 JSON 格式
docker inspect
```


9. 启动停止容器等操作
```
docker start|stop|restart [id]
# 暂停|恢复 某一容器的所有进程
docker pause|unpause [id]
# 杀死一个或多个指定容器进程
docker kill -s KILL [id]
# 停止全部运行的容器
docker stop `docker ps -q`
# 杀掉全部运行的容器
docker kill -s KILL `docker ps -q`
```
10. 交互式进入容器
```
docker exec -it {{containerName or containerID}} bash
docker exec -i {{containerName or containerID}} bash
docker exec -t {{containerName or containerID}} bash
docker exec -d {{containerName or containerID}} bash
```

11. run命令常用选项

选项 | 说明
:--|:--
-d | 后台运行容器, 并返回容器ID；不指定时, 启动后开始打印日志, Ctrl + C 退出命令同时会关闭容器
-i |以交互模式运行容器, 通常与 -t 同时使用；
-t |为容器重新分配一个伪输入终端, 通常与 -i 同时使用
--name "anyesu-container" |为容器指定一个别名, 不指定时随机生成
-h docker-anyesu |设置容器的主机名, 默认随机生成
--dns 8.8.8.8 |指定容器使用的 DNS 服务器, 默认和宿主机一致
-e docker_host=172.17.0.1 | 设置环境变量
--cpuset="0-2" or --cpuset="0,1,2" | 绑定容器到指定 CPU 运行
-m 100M | 设置容器使用内存最大值
--net bridge | 指定容器的网络连接类型, 支持 bridge / host / none / container 四种类型
--ip 172.18.0.13 | 为容器分配固定 ip ( 需要使用自定义网络 )
--expose 8081 --expose 8082 | 开放一个端口或一组端口, 会覆盖镜像设置中开放的端口
-p [宿主机端口]:[容器内端口] | 宿主机到容器的端口映射, 可指定宿主机的要监听的 ip, 默认为 0.0.0.0
-P | 注意是大写的, 宿主机随机指定一组可用的端口映射容器 expose 的所有端口
-v [宿主机目录路径]:[容器内目录路径] | 挂载宿主机的指定目录 ( 或文件 ) 到容器内的指定目录 ( 或文件 )
--add-host [主机名]:[ip] | 为容器 hosts 文件追加 host , 默认会在 hosts 文件最后追加内容：[主机名]:[容器ip]
--volumes-from [其他容器名] | 将其他容器的数据卷添加到此容器
--link [其他容器名]:[在该容器中的别名] | 添加链接到另一个容器, 在本容器 hosts 文件中加入关联容器的记录, 效果类似于 --add-host


剩个坑以后想起来再填(



