# ---docker学习笔记---



----



# docker 是什么？

Docker是基于Go语言实现的云开源项目。

Docker的主要目标是“Build，Ship and Run Any App,Anywhere”，也就是通过对应用组件的封装、分发、部署、运行等生命周期的管理，使用户的APP（可以是一个WEB应用或数据库应用等等）及其运行环境能够做到“一次镜像，处处运行”。

Linux容器技术的出现就解决了这样一个问题，而 Docker 就是在它的基础上发展过来的。将应用打成镜像，通过镜像成为运行在Docker容器上面的实例，而 Docker容器在任何操作系统上都是一致的，这就实现了跨平台、跨服务器。只需要一次配置好环境，换到别的机器上就可以一键部署好，大大简化了操作。





# 容器和虚拟机比较

## 虚拟机

虚拟机（virtual machine）就是带环境安装的一种解决方案。

它可以在一种操作系统里面运行另一种操作系统，比如在Windows10系统里面运行Linux系统CentOS7。应用程序对此毫无感知，因为虚拟机看上去跟真实系统一模一样，而对于底层系统来说，虚拟机就是一个普通文件，不需要了就删掉，对其他部分毫无影响。这类虚拟机完美的运行了另一套系统，能够使应用程序，操作系统和硬件三者之间的逻辑不变。 

虚拟机的缺点：

* 资源占用多
* 冗余步骤多
* 启动慢



## 容器

Linux容器(Linux Containers，缩写为 LXC)

Linux容器是与系统其他部分隔离开的一系列进程，从另一个镜像运行，并由该镜像提供支持进程所需的全部文件。容器提供的镜像包含了应用的所有依赖项，因而在从开发到测试再到生产的整个过程中，它都具有可移植性和一致性。

Linux 容器不是模拟一个完整的操作系统而是对进程进行隔离。有了容器，就可以将软件运行所需的所有资源打包到一个隔离的容器中。容器与虚拟机不同，不需要捆绑一整套操作系统，只需要软件工作所需的库资源和设置。系统因此而变得高效轻量并保证部署在任何环境中的软件都能始终如一地运行。

启动速度快，占用体积小



## 对比

* 传统虚拟机技术是虚拟出一套硬件后，在其上运行一个完整操作系统，在该系统上再运行所需应用进程；

* 容器内的应用进程直接运行于宿主的内核，容器内没有自己的内核且也没有进行硬件虚拟。因此容器要比传统虚拟机更为轻便。

* 每个容器之间互相隔离，每个容器有自己的文件系统 ，容器之间进程不会相互影响，能区分计算资源。





# docker的特点

一次构建，随处运行

## 更快的应用交付和部署

传统的应用开发完成后，需要提供一堆安装程序和配置说明文档，安装部署后需根据配置文档进行繁杂的配置才能正常运行。Docker化之后只需要交付少量容器镜像文件，在正式生产环境加载镜像并运行即可，应用安装配置在镜像里已经内置好，大大节省部署配置和测试验证时间。

## 更便捷的升级和扩容和缩容

随着微服务架构和Docker的发展，大量的应用会通过微服务方式架构，应用的开发构建将变成搭乐高积木一样，每个Docker容器将变成一块“积木”，应用的升级将变得非常容易。当现有的容器不足以支撑业务处理时，可通过镜像运行新的容器进行快速扩容，使应用系统的扩容从原先的天级变成分钟级甚至秒级。

## 更简单的系统运维

应用容器化运行后，生产环境运行的应用可与开发、测试环境的应用高度一致，容器会将应用程序相关的环境和状态完全封装起来，不会因为底层基础架构和操作系统的不一致性给应用带来影响，产生新的BUG。当出现程序异常时，也可以通过测试环境的相同容器进行快速定位和修复。

## 更高效的计算资源利用

Docker是内核级虚拟化，其不像传统的虚拟化技术一样需要额外的Hypervisor支持，所以在一台物理机上可以运行很多个容器实例，可大大提升物理服务器的CPU和内存的利用率。



# docker下载

docker官网：http://www.docker.com



设置镜像：

yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo



# docker的组成

## 镜像

Docker 镜像（Image）就是一个**只读**的模板。镜像可以用来创建 Docker 容器，一个镜像可以创建很多容器。

它也相当于是一个root文件系统。比如官方镜像 centos:7 就包含了完整的一套 centos:7 最小系统的 root 文件系统。

相当于容器的“源代码”，docker镜像文件类似于Java的类模板，而docker容器实例类似于java中new出来的实例对象。



## 容器

1. 从面向对象角度

Docker 利用容器（Container）独立运行的一个或一组应用，应用程序或服务运行在容器里面，容器就类似于一个虚拟化的运行环境，容器是用镜像创建的运行实例。就像是Java中的类和实例对象一样，镜像是静态的定义，容器是镜像运行时的实体。容器为镜像提供了一个标准的和隔离的运行环境，它可以被启动、开始、停止、删除。每个容器都是相互隔离的、保证安全的平台

 

2.  从镜像容器角度

可以把容器看做是一个简易版的 Linux 环境（包括root用户权限、进程空间、用户空间和网络空间等）和运行在其中的应用程序。



## 仓库

仓库（Repository）是集中存放镜像文件的场所。

类似于

Maven仓库，存放各种jar包的地方；

github仓库，存放各种git项目的地方；

Docker公司提供的官方registry被称为Docker Hub，存放各种镜像模板的地方。

仓库分为公开仓库（Public）和私有仓库（Private）两种形式。

最大的公开仓库是 Docker Hub(https://hub.docker.com/)，

存放了数量庞大的镜像供用户下载。国内的公开仓库包括阿里云 、网易云等





# docker常用命令

## 镜像命令

### docker images

列出本地主机上的镜像

```sh
PS C:\Users\mao\Desktop> docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    feb5d9fea6a5   8 months ago   13.3kB
PS C:\Users\mao\Desktop>
```



* REPOSITORY：表示镜像的仓库源

* TAG：镜像的标签版本号

* IMAGE ID：镜像ID

* CREATED：镜像创建时间

* SIZE：镜像大小

 同一仓库源可以有多个 TAG版本，代表这个仓库源的不同个版本，我们使用 REPOSITORY:TAG 来定义不同的镜像。如果你不指定一个镜像的版本标签，例如你只使用 ubuntu，docker 将默认使用 ubuntu:latest 镜像



参数：

-a：列出本地所有的镜像（含历史映像层）

-q ：只显示镜像ID。

```sh
PS C:\Users\mao\Desktop> docker images -q
feb5d9fea6a5
PS C:\Users\mao\Desktop>
```



### docker search 镜像名

```sh
PS C:\Users\mao\Desktop> docker search redis
NAME                                               DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
redis                                              Redis is an open source key-value store that…   10986     [OK]
bitnami/redis                                      Bitnami Redis Docker Image                      220                  [OK]
bitnami/redis-sentinel                             Bitnami Docker Image for Redis Sentinel         37                   [OK]
bitnami/redis-cluster                                                                              32
rapidfort/redis-cluster                            RapidFort optimized, hardened image for Redi…   13
circleci/redis                                     CircleCI images for Redis                       13                   [OK]
rapidfort/redis                                    RapidFort optimized, hardened image for Redi…   13
ubuntu/redis                                       Redis, an open source key-value store. Long-…   10
bitnami/redis-exporter                                                                             7
clearlinux/redis                                   Redis key-value data structure server with t…   4
google/guestbook-python-redis                      A simple guestbook example written in Python…   4
ibmcom/ibm-cloud-databases-redis-operator-bundle   Bundle image for the IBM Operator for Redis     1
bitnami/redis-sentinel-exporter                                                                    1
ibmcom/ibm-cloud-databases-redis-operator          Container image for the IBM Operator for Red…   1
ibmcom/redis-ha                                                                                    1
ibmcom/ibm-cloud-databases-redis-catalog           Catalog image for the IBM Operator for Redis    1
cimg/redis                                                                                         0
blackflysolutions/redis                            Redis container for Drupal and CiviCRM          0
ibmcom/redisearch-ppc64le                                                                          0
drud/redis                                         redis                                           0                    [OK]
ibmcom/redis-ppc64le                                                                               0
rancher/redislabs-ssl                                                                              0
vmware/redis-photon                                                                                0
newrelic/k8s-nri-redis                             New Relic Infrastructure Redis Integration (…   0
astronomerinc/ap-redis                             Redis service for Airflow's celery executor …   0
PS C:\Users\mao\Desktop>
```



* NAME： 镜像名字
* DESCRIPTION：镜像说明
* STARS：点赞数量
* OFFICIAL：是否为官方
* AUTOMATED：是否是自动构建的



参数：

--limit : 只列出N个镜像，默认25个

```sh
PS C:\Users\mao\Desktop> docker search --limit 5 redis
NAME                     DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
redis                    Redis is an open source key-value store that…   10986     [OK]
bitnami/redis            Bitnami Redis Docker Image                      220                  [OK]
bitnami/redis-sentinel   Bitnami Docker Image for Redis Sentinel         37                   [OK]
circleci/redis           CircleCI images for Redis                       13                   [OK]
bitnami/redis-exporter                                                   7
PS C:\Users\mao\Desktop> docker search --limit 2 redis
NAME            DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
redis           Redis is an open source key-value store that…   10986     [OK]
bitnami/redis   Bitnami Redis Docker Image                      220                  [OK]
PS C:\Users\mao\Desktop>
```





### docker pull 镜像名

下载某个镜像

或者 docker pull 镜像名:TAG

如果没有指定TAG，则下载最新版，相当于docker pull 镜像名:latest

```sh
PS C:\Users\mao\Desktop> docker pull redis
Using default tag: latest
latest: Pulling from library/redis
42c077c10790: Pull complete
a300d83d65f9: Pull complete
ebdc3afaab5c: Pull complete
31eec7f8651c: Pull complete
9c6a6b89d274: Pull complete
5c8099a4b45c: Pull complete
Digest: sha256:1b90dbfe6943c72a7469c134cad3f02eb810f016049a0e19ad78be07040cdb0c
Status: Downloaded newer image for redis:latest
docker.io/library/redis:latest
PS C:\Users\mao\Desktop>
```



```sh
PS C:\Users\mao\Desktop> docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
redis         latest    53aa81e8adfa   3 days ago     117MB
hello-world   latest    feb5d9fea6a5   8 months ago   13.3kB
PS C:\Users\mao\Desktop>
```



### docker system df 

查看镜像/容器/数据卷所占的空间

```sh
PS C:\Users\mao\Desktop> docker system df
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          2         1         116.8MB   116.8MB (99%)
Containers      4         0         0B        0B
Local Volumes   0         0         0B        0B
Build Cache     0         0         0B        0B
PS C:\Users\mao\Desktop>
```



### docker rmi 镜像ID

删除镜像

* 删除一个：docker rmi -f 镜像ID
* 删除多个：docker rmi -f 镜像名1:TAG 镜像名2:TAG ...
* 删除全部：docker rmi -f $(docker images -qa)



```sh
PS C:\Users\mao\Desktop> docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
redis         latest    53aa81e8adfa   3 days ago     117MB
hello-world   latest    feb5d9fea6a5   8 months ago   13.3kB
PS C:\Users\mao\Desktop> docker rmi -f feb5d9fea6a5
Untagged: hello-world:latest
Untagged: hello-world@sha256:10d7d58d5ebd2a652f4d93fdd86da8f265f5318c6a73cc5b6a9798ff6d2b2e67
Deleted: sha256:feb5d9fea6a5e9606aa995e879d862b825965ba48de054caab5ef356dc6b3412
PS C:\Users\mao\Desktop> docker images
REPOSITORY   TAG       IMAGE ID       CREATED      SIZE
redis        latest    53aa81e8adfa   3 days ago   117MB
PS C:\Users\mao\Desktop>
```





## 容器命令

### 新建和启动容器

命令：docker run [options] image [COMMAND] [ARG...]

 OPTIONS说明：

--name="容器新名字"    为容器指定一个名称

-d: 后台运行容器并返回容器ID，也即启动守护式容器(后台运行)；

-i：以交互模式运行容器，通常与 -t 同时使用；

-t：为容器重新分配一个伪输入终端，通常与 -i 同时使用；

​        也即启动交互式容器(前台有伪终端，等待交互)；

-P: 随机端口映射，大写P

-p: 指定端口映射，小写p

```
PS C:\Users\mao\Desktop> docker run -it ubuntu
root@0f625a3b6f69:/#
root@0f625a3b6f69:/#
root@0f625a3b6f69:/#
root@0f625a3b6f69:/# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@0f625a3b6f69:/#
```

后台运行：

```sh
PS C:\Users\mao\Desktop> docker run -d ubuntu
5952c102876398890fd1e4cf5130b13922229a170f603cef4fd9b9b619576874
PS C:\Users\mao\Desktop>
```



### 列出当前所有正在运行的容器

命令：docker ps

```sh
PS C:\Users\mao\Desktop> docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED          STATUS          PORTS     NAMES
057d044acdcb   ubuntu    "bash"    15 seconds ago   Up 15 seconds             musing_ride
PS C:\Users\mao\Desktop>
```



参数：

-a :列出当前所有正在运行的容器+历史上运行过的

```sh
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED             STATUS                         PORTS     NAMES
057d044acdcb   ubuntu         "bash"     2 minutes ago       Up 2 minutes                             musing_ride
5952c1028763   ubuntu         "bash"     3 minutes ago       Exited (0) 3 minutes ago                 silly_noyce
0f625a3b6f69   ubuntu         "bash"     5 minutes ago       Exited (130) 3 minutes ago               silly_ellis
f62123ed8d68   feb5d9fea6a5   "/hello"   About an hour ago   Exited (0) About an hour ago             hungry_hamilton
09603a7cc2bb   feb5d9fea6a5   "/hello"   2 hours ago         Exited (0) 2 hours ago                   keen_rosalind
58f57bb9913f   feb5d9fea6a5   "/hello"   2 hours ago         Exited (0) 2 hours ago                   determined_ishizaka
057165d214ed   feb5d9fea6a5   "/hello"   4 weeks ago         Exited (0) 4 weeks ago                   goofy_hertz
PS C:\Users\mao\Desktop>
```



-l :显示最近创建的容器。

```sh
PS C:\Users\mao\Desktop> docker ps -l
CONTAINER ID   IMAGE     COMMAND   CREATED         STATUS         PORTS     NAMES
057d044acdcb   ubuntu    "bash"    2 minutes ago   Up 2 minutes             musing_ride
PS C:\Users\mao\Desktop>
```



-n：显示最近n个创建的容器。

```sh
PS C:\Users\mao\Desktop> docker ps -n 2
CONTAINER ID   IMAGE     COMMAND   CREATED         STATUS                     PORTS     NAMES
057d044acdcb   ubuntu    "bash"    3 minutes ago   Up 3 minutes                         musing_ride
5952c1028763   ubuntu    "bash"    4 minutes ago   Exited (0) 4 minutes ago             silly_noyce
PS C:\Users\mao\Desktop> docker ps -n 4
CONTAINER ID   IMAGE          COMMAND    CREATED             STATUS                         PORTS     NAMES
057d044acdcb   ubuntu         "bash"     3 minutes ago       Up 3 minutes                             musing_ride
5952c1028763   ubuntu         "bash"     4 minutes ago       Exited (0) 4 minutes ago                 silly_noyce
0f625a3b6f69   ubuntu         "bash"     6 minutes ago       Exited (130) 5 minutes ago               silly_ellis
f62123ed8d68   feb5d9fea6a5   "/hello"   About an hour ago   Exited (0) About an hour ago             hungry_hamilton
PS C:\Users\mao\Desktop>
```



-q :静默模式，只显示容器编号。

```sh
PS C:\Users\mao\Desktop> docker ps -q
057d044acdcb
PS C:\Users\mao\Desktop>
```





### 退出容器

方式一：

run进去容器，exit退出，容器停止

```sh
PS C:\Users\mao\Desktop> docker run -it ubuntu
root@cb7c2e3d6b9f:/# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@cb7c2e3d6b9f:/# exit
exit
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED              STATUS                          PORTS     NAMES
cb7c2e3d6b9f   ubuntu         "bash"     38 seconds ago       Exited (0) 13 seconds ago                 zealous_kirch
a6b5661d143d   ubuntu         "bash"     About a minute ago   Exited (0) About a minute ago             admiring_colden
057d044acdcb   ubuntu         "bash"     8 minutes ago        Exited (127) 2 minutes ago                musing_ride
5952c1028763   ubuntu         "bash"     9 minutes ago        Exited (0) 9 minutes ago                  silly_noyce
0f625a3b6f69   ubuntu         "bash"     11 minutes ago       Exited (130) 10 minutes ago               silly_ellis
f62123ed8d68   feb5d9fea6a5   "/hello"   About an hour ago    Exited (0) About an hour ago              hungry_hamilton
09603a7cc2bb   feb5d9fea6a5   "/hello"   2 hours ago          Exited (0) 2 hours ago                    keen_rosalind
58f57bb9913f   feb5d9fea6a5   "/hello"   2 hours ago          Exited (0) 2 hours ago                    determined_ishizaka
057165d214ed   feb5d9fea6a5   "/hello"   4 weeks ago          Exited (0) 4 weeks ago                    goofy_hertz
PS C:\Users\mao\Desktop>
```



方式二：

进去容器，ctrl+p+q退出，容器不停止

```sh
PS C:\Users\mao\Desktop> docker run -it ubuntu
root@f8d05f9740a4:/#
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED              STATUS                          PORTS     NAMES
f8d05f9740a4   ubuntu         "bash"     22 seconds ago       Up 22 seconds                             sad_elion
cb7c2e3d6b9f   ubuntu         "bash"     About a minute ago   Exited (0) About a minute ago             zealous_kirch
a6b5661d143d   ubuntu         "bash"     2 minutes ago        Exited (0) 2 minutes ago                  admiring_colden
057d044acdcb   ubuntu         "bash"     9 minutes ago        Exited (127) 4 minutes ago                musing_ride
5952c1028763   ubuntu         "bash"     10 minutes ago       Exited (0) 10 minutes ago                 silly_noyce
0f625a3b6f69   ubuntu         "bash"     12 minutes ago       Exited (130) 11 minutes ago               silly_ellis
f62123ed8d68   feb5d9fea6a5   "/hello"   About an hour ago    Exited (0) About an hour ago              hungry_hamilton
09603a7cc2bb   feb5d9fea6a5   "/hello"   2 hours ago          Exited (0) 2 hours ago                    keen_rosalind
58f57bb9913f   feb5d9fea6a5   "/hello"   2 hours ago          Exited (0) 2 hours ago                    determined_ishizaka
057165d214ed   feb5d9fea6a5   "/hello"   4 weeks ago          Exited (0) 4 weeks ago                    goofy_hertz
PS C:\Users\mao\Desktop>
```



### 启动已停止运行的容器

命令：docker start 容器id或者容器名

```sh
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED             STATUS                         PORTS     NAMES
43c96b66638b   ubuntu         "bash"     2 minutes ago       Up 2 minutes                             stupefied_sinoussi
5065bd9f0811   ubuntu         "bash"     2 minutes ago       Up 2 minutes                             sleepy_mahavira
2e6e0a4e3a1d   ubuntu         "bash"     2 minutes ago       Up 2 minutes                             modest_visvesvaraya
f8d05f9740a4   ubuntu         "bash"     3 minutes ago       Up 3 minutes                             sad_elion
cb7c2e3d6b9f   ubuntu         "bash"     5 minutes ago       Exited (0) 4 minutes ago                 zealous_kirch
a6b5661d143d   ubuntu         "bash"     5 minutes ago       Exited (0) 4 seconds ago                 admiring_colden
057d044acdcb   ubuntu         "bash"     12 minutes ago      Exited (127) 7 minutes ago               musing_ride
5952c1028763   ubuntu         "bash"     14 minutes ago      Exited (0) 14 minutes ago                silly_noyce
0f625a3b6f69   ubuntu         "bash"     15 minutes ago      Exited (130) 14 minutes ago              silly_ellis
f62123ed8d68   feb5d9fea6a5   "/hello"   About an hour ago   Exited (0) About an hour ago             hungry_hamilton
09603a7cc2bb   feb5d9fea6a5   "/hello"   2 hours ago         Exited (0) 2 hours ago                   keen_rosalind
58f57bb9913f   feb5d9fea6a5   "/hello"   2 hours ago         Exited (0) 2 hours ago                   determined_ishizaka
057165d214ed   feb5d9fea6a5   "/hello"   4 weeks ago         Exited (0) 4 weeks ago                   goofy_hertz
PS C:\Users\mao\Desktop> docker start 057d044acdcb
057d044acdcb
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED             STATUS                         PORTS     NAMES
43c96b66638b   ubuntu         "bash"     3 minutes ago       Up 3 minutes                             stupefied_sinoussi
5065bd9f0811   ubuntu         "bash"     3 minutes ago       Up 3 minutes                             sleepy_mahavira
2e6e0a4e3a1d   ubuntu         "bash"     3 minutes ago       Up 3 minutes                             modest_visvesvaraya
f8d05f9740a4   ubuntu         "bash"     4 minutes ago       Up 4 minutes                             sad_elion
cb7c2e3d6b9f   ubuntu         "bash"     5 minutes ago       Exited (0) 5 minutes ago                 zealous_kirch
a6b5661d143d   ubuntu         "bash"     6 minutes ago       Exited (0) 45 seconds ago                admiring_colden
057d044acdcb   ubuntu         "bash"     13 minutes ago      Up 3 seconds                             musing_ride
5952c1028763   ubuntu         "bash"     14 minutes ago      Exited (0) 14 minutes ago                silly_noyce
0f625a3b6f69   ubuntu         "bash"     16 minutes ago      Exited (130) 15 minutes ago              silly_ellis
f62123ed8d68   feb5d9fea6a5   "/hello"   About an hour ago   Exited (0) About an hour ago             hungry_hamilton
09603a7cc2bb   feb5d9fea6a5   "/hello"   2 hours ago         Exited (0) 2 hours ago                   keen_rosalind
58f57bb9913f   feb5d9fea6a5   "/hello"   2 hours ago         Exited (0) 2 hours ago                   determined_ishizaka
057165d214ed   feb5d9fea6a5   "/hello"   4 weeks ago         Exited (0) 4 weeks ago                   goofy_hertz
PS C:\Users\mao\Desktop>
```



```sh
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED             STATUS                          PORTS     NAMES
43c96b66638b   ubuntu         "bash"     3 minutes ago       Up 3 minutes                              stupefied_sinoussi
5065bd9f0811   ubuntu         "bash"     4 minutes ago       Up 4 minutes                              sleepy_mahavira
2e6e0a4e3a1d   ubuntu         "bash"     4 minutes ago       Up 4 minutes                              modest_visvesvaraya
f8d05f9740a4   ubuntu         "bash"     5 minutes ago       Up 5 minutes                              sad_elion
cb7c2e3d6b9f   ubuntu         "bash"     6 minutes ago       Exited (0) 6 minutes ago                  zealous_kirch
a6b5661d143d   ubuntu         "bash"     7 minutes ago       Exited (0) About a minute ago             admiring_colden
057d044acdcb   ubuntu         "bash"     14 minutes ago      Up 53 seconds                             musing_ride
5952c1028763   ubuntu         "bash"     15 minutes ago      Exited (0) 15 minutes ago                 silly_noyce
0f625a3b6f69   ubuntu         "bash"     17 minutes ago      Exited (130) 16 minutes ago               silly_ellis
f62123ed8d68   feb5d9fea6a5   "/hello"   About an hour ago   Exited (0) About an hour ago              hungry_hamilton
09603a7cc2bb   feb5d9fea6a5   "/hello"   2 hours ago         Exited (0) 2 hours ago                    keen_rosalind
58f57bb9913f   feb5d9fea6a5   "/hello"   2 hours ago         Exited (0) 2 hours ago                    determined_ishizaka
057165d214ed   feb5d9fea6a5   "/hello"   4 weeks ago         Exited (0) 4 weeks ago                    goofy_hertz
PS C:\Users\mao\Desktop> docker start silly_ellis
silly_ellis
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED          STATUS                      PORTS     NAMES
43c96b66638b   ubuntu         "bash"     4 minutes ago    Up 4 minutes                          stupefied_sinoussi
5065bd9f0811   ubuntu         "bash"     4 minutes ago    Up 4 minutes                          sleepy_mahavira
2e6e0a4e3a1d   ubuntu         "bash"     4 minutes ago    Up 4 minutes                          modest_visvesvaraya
f8d05f9740a4   ubuntu         "bash"     5 minutes ago    Up 5 minutes                          sad_elion
cb7c2e3d6b9f   ubuntu         "bash"     7 minutes ago    Exited (0) 6 minutes ago              zealous_kirch
a6b5661d143d   ubuntu         "bash"     7 minutes ago    Exited (0) 2 minutes ago              admiring_colden
057d044acdcb   ubuntu         "bash"     14 minutes ago   Up About a minute                     musing_ride
5952c1028763   ubuntu         "bash"     16 minutes ago   Exited (0) 16 minutes ago             silly_noyce
0f625a3b6f69   ubuntu         "bash"     18 minutes ago   Up 4 seconds                          silly_ellis
f62123ed8d68   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                hungry_hamilton
09603a7cc2bb   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                keen_rosalind
58f57bb9913f   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                determined_ishizaka
057165d214ed   feb5d9fea6a5   "/hello"   4 weeks ago      Exited (0) 4 weeks ago                goofy_hertz
PS C:\Users\mao\Desktop>
```





### 重启容器

命令：docker restart 容器id或者容器名

```sh
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED          STATUS                      PORTS     NAMES
43c96b66638b   ubuntu         "bash"     5 minutes ago    Up 5 minutes                          stupefied_sinoussi
5065bd9f0811   ubuntu         "bash"     5 minutes ago    Up 5 minutes                          sleepy_mahavira
2e6e0a4e3a1d   ubuntu         "bash"     6 minutes ago    Up 6 minutes                          modest_visvesvaraya
f8d05f9740a4   ubuntu         "bash"     7 minutes ago    Up 7 minutes                          sad_elion
cb7c2e3d6b9f   ubuntu         "bash"     8 minutes ago    Exited (0) 8 minutes ago              zealous_kirch
a6b5661d143d   ubuntu         "bash"     8 minutes ago    Exited (0) 3 minutes ago              admiring_colden
057d044acdcb   ubuntu         "bash"     16 minutes ago   Up 2 minutes                          musing_ride
5952c1028763   ubuntu         "bash"     17 minutes ago   Exited (0) 17 minutes ago             silly_noyce
0f625a3b6f69   ubuntu         "bash"     19 minutes ago   Up About a minute                     silly_ellis
f62123ed8d68   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                hungry_hamilton
09603a7cc2bb   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                keen_rosalind
58f57bb9913f   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                determined_ishizaka
057165d214ed   feb5d9fea6a5   "/hello"   4 weeks ago      Exited (0) 4 weeks ago                goofy_hertz
PS C:\Users\mao\Desktop> docker restart 43c96b66638b
43c96b66638b
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED          STATUS                      PORTS     NAMES
43c96b66638b   ubuntu         "bash"     6 minutes ago    Up 5 seconds                          stupefied_sinoussi
5065bd9f0811   ubuntu         "bash"     6 minutes ago    Up 6 minutes                          sleepy_mahavira
2e6e0a4e3a1d   ubuntu         "bash"     6 minutes ago    Up 6 minutes                          modest_visvesvaraya
f8d05f9740a4   ubuntu         "bash"     7 minutes ago    Up 7 minutes                          sad_elion
cb7c2e3d6b9f   ubuntu         "bash"     9 minutes ago    Exited (0) 8 minutes ago              zealous_kirch
a6b5661d143d   ubuntu         "bash"     9 minutes ago    Exited (0) 4 minutes ago              admiring_colden
057d044acdcb   ubuntu         "bash"     17 minutes ago   Up 3 minutes                          musing_ride
5952c1028763   ubuntu         "bash"     18 minutes ago   Exited (0) 18 minutes ago             silly_noyce
0f625a3b6f69   ubuntu         "bash"     20 minutes ago   Up 2 minutes                          silly_ellis
f62123ed8d68   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                hungry_hamilton
09603a7cc2bb   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                keen_rosalind
58f57bb9913f   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                determined_ishizaka
057165d214ed   feb5d9fea6a5   "/hello"   4 weeks ago      Exited (0) 4 weeks ago                goofy_hertz
PS C:\Users\mao\Desktop>
```



```sh
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED          STATUS                      PORTS     NAMES
43c96b66638b   ubuntu         "bash"     6 minutes ago    Up 5 seconds                          stupefied_sinoussi
5065bd9f0811   ubuntu         "bash"     6 minutes ago    Up 6 minutes                          sleepy_mahavira
2e6e0a4e3a1d   ubuntu         "bash"     6 minutes ago    Up 6 minutes                          modest_visvesvaraya
f8d05f9740a4   ubuntu         "bash"     7 minutes ago    Up 7 minutes                          sad_elion
cb7c2e3d6b9f   ubuntu         "bash"     9 minutes ago    Exited (0) 8 minutes ago              zealous_kirch
a6b5661d143d   ubuntu         "bash"     9 minutes ago    Exited (0) 4 minutes ago              admiring_colden
057d044acdcb   ubuntu         "bash"     17 minutes ago   Up 3 minutes                          musing_ride
5952c1028763   ubuntu         "bash"     18 minutes ago   Exited (0) 18 minutes ago             silly_noyce
0f625a3b6f69   ubuntu         "bash"     20 minutes ago   Up 2 minutes                          silly_ellis
f62123ed8d68   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                hungry_hamilton
09603a7cc2bb   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                keen_rosalind
58f57bb9913f   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                determined_ishizaka
057165d214ed   feb5d9fea6a5   "/hello"   4 weeks ago      Exited (0) 4 weeks ago                goofy_hertz
PS C:\Users\mao\Desktop> docker restart sleepy_mahavira
sleepy_mahavira
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED          STATUS                      PORTS     NAMES
43c96b66638b   ubuntu         "bash"     7 minutes ago    Up About a minute                     stupefied_sinoussi
5065bd9f0811   ubuntu         "bash"     7 minutes ago    Up 3 seconds                          sleepy_mahavira
2e6e0a4e3a1d   ubuntu         "bash"     8 minutes ago    Up 8 minutes                          modest_visvesvaraya
f8d05f9740a4   ubuntu         "bash"     8 minutes ago    Up 8 minutes                          sad_elion
cb7c2e3d6b9f   ubuntu         "bash"     10 minutes ago   Exited (0) 9 minutes ago              zealous_kirch
a6b5661d143d   ubuntu         "bash"     10 minutes ago   Exited (0) 5 minutes ago              admiring_colden
057d044acdcb   ubuntu         "bash"     18 minutes ago   Up 4 minutes                          musing_ride
5952c1028763   ubuntu         "bash"     19 minutes ago   Exited (0) 19 minutes ago             silly_noyce
0f625a3b6f69   ubuntu         "bash"     21 minutes ago   Up 3 minutes                          silly_ellis
f62123ed8d68   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                hungry_hamilton
09603a7cc2bb   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                keen_rosalind
58f57bb9913f   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                determined_ishizaka
057165d214ed   feb5d9fea6a5   "/hello"   4 weeks ago      Exited (0) 4 weeks ago                goofy_hertz
PS C:\Users\mao\Desktop>
```





### 停止容器

命令：docker stop 容器id或者容器名



```sh
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED          STATUS                      PORTS     NAMES
43c96b66638b   ubuntu         "bash"     8 minutes ago    Up 2 minutes                          stupefied_sinoussi
5065bd9f0811   ubuntu         "bash"     8 minutes ago    Up About a minute                     sleepy_mahavira
2e6e0a4e3a1d   ubuntu         "bash"     9 minutes ago    Up 9 minutes                          modest_visvesvaraya
f8d05f9740a4   ubuntu         "bash"     9 minutes ago    Up 9 minutes                          sad_elion
cb7c2e3d6b9f   ubuntu         "bash"     11 minutes ago   Exited (0) 11 minutes ago             zealous_kirch
a6b5661d143d   ubuntu         "bash"     11 minutes ago   Exited (0) 6 minutes ago              admiring_colden
057d044acdcb   ubuntu         "bash"     19 minutes ago   Up 5 minutes                          musing_ride
5952c1028763   ubuntu         "bash"     20 minutes ago   Exited (0) 20 minutes ago             silly_noyce
0f625a3b6f69   ubuntu         "bash"     22 minutes ago   Up 4 minutes                          silly_ellis
f62123ed8d68   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                hungry_hamilton
09603a7cc2bb   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                keen_rosalind
58f57bb9913f   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                determined_ishizaka
057165d214ed   feb5d9fea6a5   "/hello"   4 weeks ago      Exited (0) 4 weeks ago                goofy_hertz
PS C:\Users\mao\Desktop> docker stop 43c96b66638b
43c96b66638b
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED          STATUS                       PORTS     NAMES
43c96b66638b   ubuntu         "bash"     9 minutes ago    Exited (137) 3 seconds ago             stupefied_sinoussi
5065bd9f0811   ubuntu         "bash"     9 minutes ago    Up About a minute                      sleepy_mahavira
2e6e0a4e3a1d   ubuntu         "bash"     9 minutes ago    Up 9 minutes                           modest_visvesvaraya
f8d05f9740a4   ubuntu         "bash"     10 minutes ago   Up 10 minutes                          sad_elion
cb7c2e3d6b9f   ubuntu         "bash"     11 minutes ago   Exited (0) 11 minutes ago              zealous_kirch
a6b5661d143d   ubuntu         "bash"     12 minutes ago   Exited (0) 6 minutes ago               admiring_colden
057d044acdcb   ubuntu         "bash"     19 minutes ago   Up 6 minutes                           musing_ride
5952c1028763   ubuntu         "bash"     20 minutes ago   Exited (0) 20 minutes ago              silly_noyce
0f625a3b6f69   ubuntu         "bash"     22 minutes ago   Up 4 minutes                           silly_ellis
f62123ed8d68   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                 hungry_hamilton
09603a7cc2bb   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                 keen_rosalind
58f57bb9913f   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                 determined_ishizaka
057165d214ed   feb5d9fea6a5   "/hello"   4 weeks ago      Exited (0) 4 weeks ago                 goofy_hertz
PS C:\Users\mao\Desktop>
```



```sh
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED          STATUS                        PORTS     NAMES
43c96b66638b   ubuntu         "bash"     9 minutes ago    Exited (137) 26 seconds ago             stupefied_sinoussi
5065bd9f0811   ubuntu         "bash"     9 minutes ago    Up About a minute                       sleepy_mahavira
2e6e0a4e3a1d   ubuntu         "bash"     9 minutes ago    Up 9 minutes                            modest_visvesvaraya
f8d05f9740a4   ubuntu         "bash"     10 minutes ago   Up 10 minutes                           sad_elion
cb7c2e3d6b9f   ubuntu         "bash"     12 minutes ago   Exited (0) 11 minutes ago               zealous_kirch
a6b5661d143d   ubuntu         "bash"     12 minutes ago   Exited (0) 7 minutes ago                admiring_colden
057d044acdcb   ubuntu         "bash"     20 minutes ago   Up 6 minutes                            musing_ride
5952c1028763   ubuntu         "bash"     21 minutes ago   Exited (0) 21 minutes ago               silly_noyce
0f625a3b6f69   ubuntu         "bash"     23 minutes ago   Up 5 minutes                            silly_ellis
f62123ed8d68   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                  hungry_hamilton
09603a7cc2bb   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                  keen_rosalind
58f57bb9913f   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                  determined_ishizaka
057165d214ed   feb5d9fea6a5   "/hello"   4 weeks ago      Exited (0) 4 weeks ago                  goofy_hertz
PS C:\Users\mao\Desktop> docker stop sad_elion
sad_elion
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED          STATUS                        PORTS     NAMES
43c96b66638b   ubuntu         "bash"     10 minutes ago   Exited (137) 55 seconds ago             stupefied_sinoussi
5065bd9f0811   ubuntu         "bash"     10 minutes ago   Up 2 minutes                            sleepy_mahavira
2e6e0a4e3a1d   ubuntu         "bash"     10 minutes ago   Up 10 minutes                           modest_visvesvaraya
f8d05f9740a4   ubuntu         "bash"     11 minutes ago   Exited (137) 2 seconds ago              sad_elion
cb7c2e3d6b9f   ubuntu         "bash"     12 minutes ago   Exited (0) 12 minutes ago               zealous_kirch
a6b5661d143d   ubuntu         "bash"     13 minutes ago   Exited (0) 7 minutes ago                admiring_colden
057d044acdcb   ubuntu         "bash"     20 minutes ago   Up 7 minutes                            musing_ride
5952c1028763   ubuntu         "bash"     21 minutes ago   Exited (0) 21 minutes ago               silly_noyce
0f625a3b6f69   ubuntu         "bash"     23 minutes ago   Up 5 minutes                            silly_ellis
f62123ed8d68   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                  hungry_hamilton
09603a7cc2bb   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                  keen_rosalind
58f57bb9913f   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                  determined_ishizaka
057165d214ed   feb5d9fea6a5   "/hello"   4 weeks ago      Exited (0) 4 weeks ago                  goofy_hertz
PS C:\Users\mao\Desktop>
```



### 强制停止容器

命令：docker kill 容器id或者容器名

```sh
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED          STATUS                            PORTS     NAMES
43c96b66638b   ubuntu         "bash"     11 minutes ago   Exited (137) 2 minutes ago                  stupefied_sinoussi
5065bd9f0811   ubuntu         "bash"     11 minutes ago   Up 3 minutes                                sleepy_mahavira
2e6e0a4e3a1d   ubuntu         "bash"     11 minutes ago   Up 11 minutes                               modest_visvesvaraya
f8d05f9740a4   ubuntu         "bash"     12 minutes ago   Exited (137) About a minute ago             sad_elion
cb7c2e3d6b9f   ubuntu         "bash"     14 minutes ago   Exited (0) 13 minutes ago                   zealous_kirch
a6b5661d143d   ubuntu         "bash"     14 minutes ago   Exited (0) 9 minutes ago                    admiring_colden
057d044acdcb   ubuntu         "bash"     22 minutes ago   Up 8 minutes                                musing_ride
5952c1028763   ubuntu         "bash"     23 minutes ago   Exited (0) 23 minutes ago                   silly_noyce
0f625a3b6f69   ubuntu         "bash"     25 minutes ago   Up 7 minutes                                silly_ellis
f62123ed8d68   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                      hungry_hamilton
09603a7cc2bb   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                      keen_rosalind
58f57bb9913f   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                      determined_ishizaka
057165d214ed   feb5d9fea6a5   "/hello"   4 weeks ago      Exited (0) 4 weeks ago                      goofy_hertz
PS C:\Users\mao\Desktop> docker kill 5065bd9f0811
5065bd9f0811
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED          STATUS                            PORTS     NAMES
43c96b66638b   ubuntu         "bash"     11 minutes ago   Exited (137) 2 minutes ago                  stupefied_sinoussi
5065bd9f0811   ubuntu         "bash"     12 minutes ago   Exited (137) 4 seconds ago                  sleepy_mahavira
2e6e0a4e3a1d   ubuntu         "bash"     12 minutes ago   Up 12 minutes                               modest_visvesvaraya
f8d05f9740a4   ubuntu         "bash"     13 minutes ago   Exited (137) About a minute ago             sad_elion
cb7c2e3d6b9f   ubuntu         "bash"     14 minutes ago   Exited (0) 14 minutes ago                   zealous_kirch
a6b5661d143d   ubuntu         "bash"     15 minutes ago   Exited (0) 9 minutes ago                    admiring_colden
057d044acdcb   ubuntu         "bash"     22 minutes ago   Up 9 minutes                                musing_ride
5952c1028763   ubuntu         "bash"     23 minutes ago   Exited (0) 23 minutes ago                   silly_noyce
0f625a3b6f69   ubuntu         "bash"     25 minutes ago   Up 7 minutes                                silly_ellis
f62123ed8d68   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                      hungry_hamilton
09603a7cc2bb   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                      keen_rosalind
58f57bb9913f   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                      determined_ishizaka
057165d214ed   feb5d9fea6a5   "/hello"   4 weeks ago      Exited (0) 4 weeks ago                      goofy_hertz
PS C:\Users\mao\Desktop>
```



```sh
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED          STATUS                        PORTS     NAMES
43c96b66638b   ubuntu         "bash"     12 minutes ago   Exited (137) 3 minutes ago              stupefied_sinoussi
5065bd9f0811   ubuntu         "bash"     12 minutes ago   Exited (137) 23 seconds ago             sleepy_mahavira
2e6e0a4e3a1d   ubuntu         "bash"     12 minutes ago   Up 12 minutes                           modest_visvesvaraya
f8d05f9740a4   ubuntu         "bash"     13 minutes ago   Exited (137) 2 minutes ago              sad_elion
cb7c2e3d6b9f   ubuntu         "bash"     14 minutes ago   Exited (0) 14 minutes ago               zealous_kirch
a6b5661d143d   ubuntu         "bash"     15 minutes ago   Exited (0) 10 minutes ago               admiring_colden
057d044acdcb   ubuntu         "bash"     22 minutes ago   Up 9 minutes                            musing_ride
5952c1028763   ubuntu         "bash"     24 minutes ago   Exited (0) 24 minutes ago               silly_noyce
0f625a3b6f69   ubuntu         "bash"     25 minutes ago   Up 7 minutes                            silly_ellis
f62123ed8d68   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                  hungry_hamilton
09603a7cc2bb   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                  keen_rosalind
58f57bb9913f   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                  determined_ishizaka
057165d214ed   feb5d9fea6a5   "/hello"   4 weeks ago      Exited (0) 4 weeks ago                  goofy_hertz
PS C:\Users\mao\Desktop> docker kill modest_visvesvaraya
modest_visvesvaraya
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED          STATUS                            PORTS     NAMES
43c96b66638b   ubuntu         "bash"     12 minutes ago   Exited (137) 3 minutes ago                  stupefied_sinoussi
5065bd9f0811   ubuntu         "bash"     13 minutes ago   Exited (137) About a minute ago             sleepy_mahavira
2e6e0a4e3a1d   ubuntu         "bash"     13 minutes ago   Exited (137) 2 seconds ago                  modest_visvesvaraya
f8d05f9740a4   ubuntu         "bash"     14 minutes ago   Exited (137) 2 minutes ago                  sad_elion
cb7c2e3d6b9f   ubuntu         "bash"     15 minutes ago   Exited (0) 15 minutes ago                   zealous_kirch
a6b5661d143d   ubuntu         "bash"     16 minutes ago   Exited (0) 10 minutes ago                   admiring_colden
057d044acdcb   ubuntu         "bash"     23 minutes ago   Up 9 minutes                                musing_ride
5952c1028763   ubuntu         "bash"     24 minutes ago   Exited (0) 24 minutes ago                   silly_noyce
0f625a3b6f69   ubuntu         "bash"     26 minutes ago   Up 8 minutes                                silly_ellis
f62123ed8d68   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                      hungry_hamilton
09603a7cc2bb   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                      keen_rosalind
58f57bb9913f   feb5d9fea6a5   "/hello"   2 hours ago      Exited (0) 2 hours ago                      determined_ishizaka
057165d214ed   feb5d9fea6a5   "/hello"   4 weeks ago      Exited (0) 4 weeks ago                      goofy_hertz
PS C:\Users\mao\Desktop>
```





### 删除已停止的容器

命令：docker rm 容器id

```sh
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED       STATUS                    PORTS     NAMES
5065bd9f0811   ubuntu         "bash"     4 days ago    Exited (137) 4 days ago             sleepy_mahavira
2e6e0a4e3a1d   ubuntu         "bash"     4 days ago    Exited (137) 4 days ago             modest_visvesvaraya
f8d05f9740a4   ubuntu         "bash"     4 days ago    Exited (137) 4 days ago             sad_elion
cb7c2e3d6b9f   ubuntu         "bash"     4 days ago    Exited (0) 4 days ago               zealous_kirch
a6b5661d143d   ubuntu         "bash"     4 days ago    Exited (0) 4 days ago               admiring_colden
057d044acdcb   ubuntu         "bash"     4 days ago    Exited (255) 4 days ago             musing_ride
5952c1028763   ubuntu         "bash"     4 days ago    Exited (0) 4 days ago               silly_noyce
0f625a3b6f69   ubuntu         "bash"     4 days ago    Exited (255) 4 days ago             silly_ellis
f62123ed8d68   feb5d9fea6a5   "/hello"   4 days ago    Exited (0) 4 days ago               hungry_hamilton
09603a7cc2bb   feb5d9fea6a5   "/hello"   4 days ago    Exited (0) 4 days ago               keen_rosalind
58f57bb9913f   feb5d9fea6a5   "/hello"   4 days ago    Exited (0) 4 days ago               determined_ishizaka
057165d214ed   feb5d9fea6a5   "/hello"   4 weeks ago   Exited (0) 4 weeks ago              goofy_hertz
PS C:\Users\mao\Desktop> docker rm cb7c2e3d6b9f
cb7c2e3d6b9f
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED       STATUS                    PORTS     NAMES
5065bd9f0811   ubuntu         "bash"     4 days ago    Exited (137) 4 days ago             sleepy_mahavira
2e6e0a4e3a1d   ubuntu         "bash"     4 days ago    Exited (137) 4 days ago             modest_visvesvaraya
f8d05f9740a4   ubuntu         "bash"     4 days ago    Exited (137) 4 days ago             sad_elion
a6b5661d143d   ubuntu         "bash"     4 days ago    Exited (0) 4 days ago               admiring_colden
057d044acdcb   ubuntu         "bash"     4 days ago    Exited (255) 4 days ago             musing_ride
5952c1028763   ubuntu         "bash"     4 days ago    Exited (0) 4 days ago               silly_noyce
0f625a3b6f69   ubuntu         "bash"     4 days ago    Exited (255) 4 days ago             silly_ellis
f62123ed8d68   feb5d9fea6a5   "/hello"   4 days ago    Exited (0) 4 days ago               hungry_hamilton
09603a7cc2bb   feb5d9fea6a5   "/hello"   4 days ago    Exited (0) 4 days ago               keen_rosalind
58f57bb9913f   feb5d9fea6a5   "/hello"   4 days ago    Exited (0) 4 days ago               determined_ishizaka
057165d214ed   feb5d9fea6a5   "/hello"   4 weeks ago   Exited (0) 4 weeks ago              goofy_hertz
PS C:\Users\mao\Desktop> docker rm f62123ed8d68
f62123ed8d68
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED       STATUS                    PORTS     NAMES
5065bd9f0811   ubuntu         "bash"     4 days ago    Exited (137) 4 days ago             sleepy_mahavira
2e6e0a4e3a1d   ubuntu         "bash"     4 days ago    Exited (137) 4 days ago             modest_visvesvaraya
f8d05f9740a4   ubuntu         "bash"     4 days ago    Exited (137) 4 days ago             sad_elion
a6b5661d143d   ubuntu         "bash"     4 days ago    Exited (0) 4 days ago               admiring_colden
057d044acdcb   ubuntu         "bash"     4 days ago    Exited (255) 4 days ago             musing_ride
5952c1028763   ubuntu         "bash"     4 days ago    Exited (0) 4 days ago               silly_noyce
0f625a3b6f69   ubuntu         "bash"     4 days ago    Exited (255) 4 days ago             silly_ellis
09603a7cc2bb   feb5d9fea6a5   "/hello"   4 days ago    Exited (0) 4 days ago               keen_rosalind
58f57bb9913f   feb5d9fea6a5   "/hello"   4 days ago    Exited (0) 4 days ago               determined_ishizaka
057165d214ed   feb5d9fea6a5   "/hello"   4 weeks ago   Exited (0) 4 weeks ago              goofy_hertz
PS C:\Users\mao\Desktop>
```





### 启动守护式容器

命令：docker run -d 容器名



\#使用镜像centos:latest以后台模式启动一个容器

docker run -d centos

问题：然后docker ps -a 进行查看, 会发现容器已经退出

很重要的要说明的一点: Docker容器后台运行,就必须有一个前台进程.

容器运行的命令如果不是那些一直挂起的命令（比如运行top，tail），就是会自动退出的。

这个是docker的机制问题,比如你的web容器,我们以nginx为例，正常情况下,

我们配置启动服务只需要启动响应的service即可。例如service nginx start

但是,这样做,nginx为后台进程模式运行,就导致docker前台没有运行的应用,

这样的容器后台启动后,会立即自杀因为他觉得他没事可做了.

所以，最佳的解决方案是,将你要运行的程序以前台进程的形式运行，

常见就是命令行模式，表示我还有交互操作，别中断



```sh
PS C:\Users\mao\Desktop> docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
redis        latest    53aa81e8adfa   7 days ago    117MB
ubuntu       latest    d2e4e1f51132   5 weeks ago   77.8MB
PS C:\Users\mao\Desktop> docker run -p6389:6379 -d redis
a7275991a5f8ca607a27bb46c920e9cd0efbda2eeb5e1f08323a621e1a005d11
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                  PORTS                    NAMES
a7275991a5f8   redis     "docker-entrypoint.s…"   10 seconds ago   Up 9 seconds            0.0.0.0:6389->6379/tcp   nostalgic_zhukovsky
5952c1028763   ubuntu    "bash"                   4 days ago       Exited (0) 4 days ago                            silly_noyce
PS C:\Users\mao\Desktop> redis-cli -p 6389
127.0.0.1:6389> ping
PONG
127.0.0.1:6389> exit
PS C:\Users\mao\Desktop>
```





### 查看容器日志

命令：docker logs 容器ID

```sh
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS                          PORTS     NAMES
a7275991a5f8   redis     "docker-entrypoint.s…"   6 minutes ago   Exited (0) About a minute ago             nostalgic_zhukovsky
5952c1028763   ubuntu    "bash"                   4 days ago      Exited (0) 4 days ago                     silly_noyce
PS C:\Users\mao\Desktop> docker logs a7275991a5f8
1:C 05 Jun 2022 06:14:56.388 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 05 Jun 2022 06:14:56.389 # Redis version=7.0.0, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 05 Jun 2022 06:14:56.389 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
1:M 05 Jun 2022 06:14:56.389 * monotonic clock: POSIX clock_gettime
1:M 05 Jun 2022 06:14:56.390 * Running mode=standalone, port=6379.
1:M 05 Jun 2022 06:14:56.390 # Server initialized
1:M 05 Jun 2022 06:14:56.390 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
1:M 05 Jun 2022 06:14:56.391 * The AOF directory appendonlydir doesn't exist
1:M 05 Jun 2022 06:14:56.391 * Ready to accept connections
1:signal-handler (1654409781) Received SIGTERM scheduling shutdown...
1:M 05 Jun 2022 06:16:21.632 # User requested shutdown...
1:M 05 Jun 2022 06:16:21.632 * Saving the final RDB snapshot before exiting.
1:M 05 Jun 2022 06:16:21.634 * DB saved on disk
1:M 05 Jun 2022 06:16:21.634 # Redis is now ready to exit, bye bye...
1:C 05 Jun 2022 06:16:46.592 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 05 Jun 2022 06:16:46.593 # Redis version=7.0.0, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 05 Jun 2022 06:16:46.593 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
1:M 05 Jun 2022 06:16:46.593 * monotonic clock: POSIX clock_gettime
1:M 05 Jun 2022 06:16:46.593 * Running mode=standalone, port=6379.
1:M 05 Jun 2022 06:16:46.593 # Server initialized
1:M 05 Jun 2022 06:16:46.593 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
1:M 05 Jun 2022 06:16:46.594 * The AOF directory appendonlydir doesn't exist
1:M 05 Jun 2022 06:16:46.594 * Loading RDB produced by version 7.0.0
1:M 05 Jun 2022 06:16:46.594 * RDB age 25 seconds
1:M 05 Jun 2022 06:16:46.594 * RDB memory usage when created 0.87 Mb
1:M 05 Jun 2022 06:16:46.594 * Done loading RDB, keys loaded: 0, keys expired: 0.
1:M 05 Jun 2022 06:16:46.594 * DB loaded from disk: 0.000 seconds
1:M 05 Jun 2022 06:16:46.594 * Ready to accept connections
1:signal-handler (1654409818) Received SIGTERM scheduling shutdown...
1:M 05 Jun 2022 06:16:58.633 # User requested shutdown...
1:M 05 Jun 2022 06:16:58.633 * Saving the final RDB snapshot before exiting.
1:M 05 Jun 2022 06:16:58.635 * DB saved on disk
1:M 05 Jun 2022 06:16:58.635 # Redis is now ready to exit, bye bye...
1:C 05 Jun 2022 06:17:16.302 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 05 Jun 2022 06:17:16.302 # Redis version=7.0.0, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 05 Jun 2022 06:17:16.302 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
1:M 05 Jun 2022 06:17:16.303 * monotonic clock: POSIX clock_gettime
1:M 05 Jun 2022 06:17:16.303 * Running mode=standalone, port=6379.
1:M 05 Jun 2022 06:17:16.303 # Server initialized
1:M 05 Jun 2022 06:17:16.303 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
1:M 05 Jun 2022 06:17:16.304 * The AOF directory appendonlydir doesn't exist
1:M 05 Jun 2022 06:17:16.304 * Loading RDB produced by version 7.0.0
1:M 05 Jun 2022 06:17:16.304 * RDB age 18 seconds
1:M 05 Jun 2022 06:17:16.304 * RDB memory usage when created 0.82 Mb
1:M 05 Jun 2022 06:17:16.304 * Done loading RDB, keys loaded: 0, keys expired: 0.
1:M 05 Jun 2022 06:17:16.304 * DB loaded from disk: 0.000 seconds
1:M 05 Jun 2022 06:17:16.304 * Ready to accept connections
1:signal-handler (1654409962) Received SIGTERM scheduling shutdown...
1:M 05 Jun 2022 06:19:22.919 # User requested shutdown...
1:M 05 Jun 2022 06:19:22.920 * Saving the final RDB snapshot before exiting.
1:M 05 Jun 2022 06:19:22.922 * DB saved on disk
1:M 05 Jun 2022 06:19:22.922 # Redis is now ready to exit, bye bye...
1:C 05 Jun 2022 06:20:03.271 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 05 Jun 2022 06:20:03.271 # Redis version=7.0.0, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 05 Jun 2022 06:20:03.271 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
1:M 05 Jun 2022 06:20:03.271 * monotonic clock: POSIX clock_gettime
1:M 05 Jun 2022 06:20:03.272 * Running mode=standalone, port=6379.
1:M 05 Jun 2022 06:20:03.272 # Server initialized
1:M 05 Jun 2022 06:20:03.272 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
1:M 05 Jun 2022 06:20:03.272 * The AOF directory appendonlydir doesn't exist
1:M 05 Jun 2022 06:20:03.272 * Loading RDB produced by version 7.0.0
1:M 05 Jun 2022 06:20:03.272 * RDB age 41 seconds
1:M 05 Jun 2022 06:20:03.272 * RDB memory usage when created 0.87 Mb
1:M 05 Jun 2022 06:20:03.272 * Done loading RDB, keys loaded: 0, keys expired: 0.
1:M 05 Jun 2022 06:20:03.272 * DB loaded from disk: 0.000 seconds
1:M 05 Jun 2022 06:20:03.272 * Ready to accept connections
1:signal-handler (1654410018) Received SIGTERM scheduling shutdown...
1:M 05 Jun 2022 06:20:18.916 # User requested shutdown...
1:M 05 Jun 2022 06:20:18.916 * Saving the final RDB snapshot before exiting.
1:M 05 Jun 2022 06:20:18.918 * DB saved on disk
1:M 05 Jun 2022 06:20:18.918 # Redis is now ready to exit, bye bye...
PS C:\Users\mao\Desktop>
```



### 查看容器内运行的进程

命令：docker top 容器ID

```sh
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS                     PORTS     NAMES
a7275991a5f8   redis     "docker-entrypoint.s…"   8 minutes ago   Exited (0) 3 minutes ago             nostalgic_zhukovsky
5952c1028763   ubuntu    "bash"                   4 days ago      Exited (0) 4 days ago                silly_noyce
PS C:\Users\mao\Desktop> docker top a7275991a5f8
Error response from daemon: Container a7275991a5f8ca607a27bb46c920e9cd0efbda2eeb5e1f08323a621e1a005d11 is not running
PS C:\Users\mao\Desktop> docker start a7275991a5f8
a7275991a5f8
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS                  PORTS                    NAMES
a7275991a5f8   redis     "docker-entrypoint.s…"   9 minutes ago   Up 4 seconds            0.0.0.0:6389->6379/tcp   nostalgic_zhukovsky
5952c1028763   ubuntu    "bash"                   4 days ago      Exited (0) 4 days ago                            silly_noyce
PS C:\Users\mao\Desktop> docker top a7275991a5f8
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
999                 1664                1644                0                   06:23               ?                   00:00:00            redis-server *:6379
PS C:\Users\mao\Desktop>
```





### 查看容器内部细节

命令：docker inspect 容器ID

```sh
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                  PORTS                    NAMES
a7275991a5f8   redis     "docker-entrypoint.s…"   10 minutes ago   Up About a minute       0.0.0.0:6389->6379/tcp   nostalgic_zhukovsky
5952c1028763   ubuntu    "bash"                   4 days ago       Exited (0) 4 days ago                            silly_noyce
PS C:\Users\mao\Desktop> docker inspect a7275991a5f8
[
    {
        "Id": "a7275991a5f8ca607a27bb46c920e9cd0efbda2eeb5e1f08323a621e1a005d11",
        "Created": "2022-06-05T06:14:55.905008518Z",
        "Path": "docker-entrypoint.sh",
        "Args": [
            "redis-server"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 1664,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2022-06-05T06:23:57.563259974Z",
            "FinishedAt": "2022-06-05T06:20:18.920939498Z"
        },
        "Image": "sha256:53aa81e8adfa939348cd4c846c0ab682b16dc7641714e36bfc57b764f0b947dc",
        "ResolvConfPath": "/var/lib/docker/containers/a7275991a5f8ca607a27bb46c920e9cd0efbda2eeb5e1f08323a621e1a005d11/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/a7275991a5f8ca607a27bb46c920e9cd0efbda2eeb5e1f08323a621e1a005d11/hostname",
        "HostsPath": "/var/lib/docker/containers/a7275991a5f8ca607a27bb46c920e9cd0efbda2eeb5e1f08323a621e1a005d11/hosts",
        "LogPath": "/var/lib/docker/containers/a7275991a5f8ca607a27bb46c920e9cd0efbda2eeb5e1f08323a621e1a005d11/a7275991a5f8ca607a27bb46c920e9cd0efbda2eeb5e1f08323a621e1a005d11-json.log",
        "Name": "/nostalgic_zhukovsky",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {
                "6379/tcp": [
                    {
                        "HostIp": "",
                        "HostPort": "6389"
                    }
                ]
            },
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                50,
                160
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/13ef8274b81a6c8f26ac06895ac304c739946c2fec8e1afb91b9c9cc3ab65f0f-init/diff:/var/lib/docker/overlay2/554d073f62c69fe5ddf8714516024f5ebbd351d52b678ba42d8d23033fa78c0b/diff:/var/lib/docker/overlay2/6eae66b49bdbb91a82f0c570a68e46060541bb449cb23fb280c5e8ed33160c6c/diff:/var/lib/docker/overlay2/c12ba5c77a448628b25da9a453c4b0a6b7f7495e54695e8c9691deaaf57a3772/diff:/var/lib/docker/overlay2/4fc5482dfe8a9747a641e624dc32314b7721176f8b32bfc231d864fd3b2f0729/diff:/var/lib/docker/overlay2/91211f126b80c5a64623113c4b5fba230fbdf27a90780597225b255d72f589f6/diff:/var/lib/docker/overlay2/aa45b050ad99ffd7c424550d337ecfa68105d5c946443933f257ab55ec5e86d0/diff",
                "MergedDir": "/var/lib/docker/overlay2/13ef8274b81a6c8f26ac06895ac304c739946c2fec8e1afb91b9c9cc3ab65f0f/merged",
                "UpperDir": "/var/lib/docker/overlay2/13ef8274b81a6c8f26ac06895ac304c739946c2fec8e1afb91b9c9cc3ab65f0f/diff",
                "WorkDir": "/var/lib/docker/overlay2/13ef8274b81a6c8f26ac06895ac304c739946c2fec8e1afb91b9c9cc3ab65f0f/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [
            {
                "Type": "volume",
                "Name": "d3f51821d9173e29f4d17fbb2c34f080819ba7af04926d45735efab95258d7c3",
                "Source": "/var/lib/docker/volumes/d3f51821d9173e29f4d17fbb2c34f080819ba7af04926d45735efab95258d7c3/_data",
                "Destination": "/data",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],
        "Config": {
            "Hostname": "a7275991a5f8",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "6379/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.14",
                "REDIS_VERSION=7.0.0",
                "REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-7.0.0.tar.gz",
                "REDIS_DOWNLOAD_SHA=284d8bd1fd85d6a55a05ee4e7c31c31977ad56cbf344ed83790beeb148baa720"
            ],
            "Cmd": [
                "redis-server"
            ],
            "Image": "redis",
            "Volumes": {
                "/data": {}
            },
            "WorkingDir": "/data",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {}
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "83e118569bb5bfc82b4373c053814d3c6d980be3e7489805c9da06b5a23e1fda",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {
                "6379/tcp": [
                    {
                        "HostIp": "0.0.0.0",
                        "HostPort": "6389"
                    }
                ]
            },
            "SandboxKey": "/var/run/docker/netns/83e118569bb5",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "07cd994deaa2d32bf5d28c81cb5b157fe081da4648e914534a9c1626b15363a3",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "f98ca8a13ff8439242393c0d1a517feb40fa821c712b68669df607373e2a5f7e",
                    "EndpointID": "07cd994deaa2d32bf5d28c81cb5b157fe081da4648e914534a9c1626b15363a3",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
PS C:\Users\mao\Desktop>
```





### 进入正在运行的容器并以命令行交互

命令：docker exec -it 容器ID bashShell

```sh
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                  PORTS                    NAMES
a7275991a5f8   redis     "docker-entrypoint.s…"   13 minutes ago   Up 4 minutes            0.0.0.0:6389->6379/tcp   nostalgic_zhukovsky
5952c1028763   ubuntu    "bash"                   4 days ago       Exited (0) 4 days ago                            silly_noyce
PS C:\Users\mao\Desktop> docker exec -it a7275991a5f8 /bin/bash
root@a7275991a5f8:/data# ls
dump.rdb
root@a7275991a5f8:/data# cd ./../
root@a7275991a5f8:/# ls
bin  boot  data  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@a7275991a5f8:/# exit
exit
PS C:\Users\mao\Desktop>

```



重新进入：

docker attach 容器ID



区别：

attach 直接进入容器启动命令的终端，不会启动新的进程，exit退出，会导致容器的停止。

exec 是在容器中打开新的终端，并且可以启动新的进程，exit退出，不会导致容器的停止。





### 从容器内拷贝文件到主机上

docker cp 容器ID:容器内路径 目的主机路径



export ：导出容器的内容留作为一个tar归档文件[对应import命令]

import ：从tar包中的内容创建一个新的文件系统再导入为镜像[对应export]



```sh
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                  PORTS                    NAMES
a7275991a5f8   redis     "docker-entrypoint.s…"   26 minutes ago   Up 7 minutes            0.0.0.0:6389->6379/tcp   nostalgic_zhukovsky
5952c1028763   ubuntu    "bash"                   4 days ago       Exited (0) 4 days ago                            silly_noyce
PS C:\Users\mao\Desktop> docker export a7275991a5f8 a.tar
"docker export" requires exactly 1 argument.
See 'docker export --help'.

Usage:  docker export [OPTIONS] CONTAINER

Export a container's filesystem as a tar archive
PS C:\Users\mao\Desktop> docker export a7275991a5f8 > a.tar
PS C:\Users\mao\Desktop> ls


    目录: C:\Users\mao\Desktop


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----          2022/6/5     14:43      204765792 a.tar
```







# Docker 镜像

## 是什么？

是一种轻量级、可执行的独立软件包，它包含运行某个软件所需的所有内容，我们把应用程序和配置依赖打包好形成一个可交付的运行环境(包括代码、运行时需要的库、环境变量和配置文件等)，这个打包好的运行环境就是image镜像文件。

只有通过这个镜像文件才能生成Docker容器实例(类似Java中new出来一个对象)。



## 联合文件系统

UnionFS（联合文件系统）：Union文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下(unite several directories into a single virtual filesystem)。Union 文件系统是 Docker 镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录



## Docker镜像加载原理

docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。

bootfs(boot file system)主要包含bootloader和kernel, bootloader主要是引导加载kernel, Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层是引导文件系统bootfs。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。

rootfs (root file system) ，在bootfs之上。包含的就是典型 Linux 系统中的 /dev, /proc, /bin, /etc 等标准目录和文件。rootfs就是各种不同的操作系统发行版，比如Ubuntu，Centos等等。 

对于一个精简的OS，rootfs可以很小，只需要包括最基本的命令、工具和程序库就可以了，因为底层直接用Host的kernel，自己只需要提供 rootfs 就行了。由此可见对于不同的linux发行版, bootfs基本是一致的, rootfs会有差别, 因此不同的发行版可以公用bootfs。





镜像分层最大的一个好处就是共享资源，方便复制迁移，就是为了复用。

比如说有多个镜像都从相同的 base 镜像构建而来，那么 Docker Host 只需在磁盘上保存一份 base 镜像；

同时内存中也只需加载一份 base 镜像，就可以为所有容器服务了。而且镜像的每一层都可以被共享。



Docker镜像层都是只读的，容器层是可写的

当容器启动时，一个新的可写层被加载到镜像的顶部。

这一层通常被称作“容器层”，“容器层”之下的都叫“镜像层”。



当容器启动时，一个新的可写层被加载到镜像的顶部。这一层通常被称作“容器层”，“容器层”之下的都叫“镜像层”。

所有对容器的改动 - 无论添加、删除、还是修改文件都只会发生在容器层中。只有容器层是可写的，容器层下面的所有镜像层都是只读的。





## docker commit

docker commit提交容器副本使之成为一个新的镜像

命令：docker commit -m="提交的描述信息" -a="作者" 容器ID 要创建的目标镜像名:[标签名]



```sh
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                  PORTS                    NAMES
a7275991a5f8   redis     "docker-entrypoint.s…"   39 minutes ago   Up 19 minutes           0.0.0.0:6389->6379/tcp   nostalgic_zhukovsky
5952c1028763   ubuntu    "bash"                   4 days ago       Exited (0) 4 days ago                            silly_noyce
PS C:\Users\mao\Desktop> docker stop nostalgic_zhukovsky
nostalgic_zhukovsky
PS C:\Users\mao\Desktop> docker commit -m="abc" -a="mao" a7275991a5f8 myredis:1.0
sha256:be89dd503a5f6160a49385b8c78ce72cb354403fa5e6e45e23f2270018092fb7
PS C:\Users\mao\Desktop> docker images
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
myredis      1.0       be89dd503a5f   9 seconds ago   117MB
redis        latest    53aa81e8adfa   7 days ago      117MB
ubuntu       latest    d2e4e1f51132   5 weeks ago     77.8MB
PS C:\Users\mao\Desktop> docker run -d myredis
Unable to find image 'myredis:latest' locally
docker: Error response from daemon: pull access denied for myredis, repository does not exist or may require 'docker login': denied: requested access to the resource is denied.
See 'docker run --help'.
PS C:\Users\mao\Desktop> docker run -d myredis:1.0
88cb42a3ce4452e42735008a5460d0c0ead2499c7ed86569d84c9f6effd86870
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS                     PORTS      NAMES
88cb42a3ce44   myredis:1.0   "docker-entrypoint.s…"   11 seconds ago   Up 10 seconds              6379/tcp   vigorous_solomon
a7275991a5f8   redis         "docker-entrypoint.s…"   41 minutes ago   Exited (0) 2 minutes ago              nostalgic_zhukovsky
5952c1028763   ubuntu        "bash"                   4 days ago       Exited (0) 4 days ago                 silly_noyce
PS C:\Users\mao\Desktop>
```







# Docker 容器数据卷

命令：docker run -it --privileged=true -v 宿主机绝对路径目录:/容器内目录  镜像名



 --privileged=true：

Docker挂载主机目录访问如果出现cannot open directory .: Permission denied

使用--privileged=true可以解决此问题



```sh
docker run -it  --privileged=true  -p6380:6379 --name redis1 -v H:/Docker/redis:/redis redis
```



读写：

命令： docker run -it --privileged=true -v /宿主机绝对路径目录:/容器内目录:rw   镜像名

只读：

命令： docker run -it --privileged=true -v /宿主机绝对路径目录:/容器内目录:ro   镜像名







# 常用安装

## 安装Tomcat

### 1. 搜索镜像

命令：docker search tomcat

```sh
PS C:\Users\mao\Desktop> docker search Tomcat
NAME                                           DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
tomcat                                         Apache Tomcat is an open source implementati…   3341      [OK]
tomee                                          Apache TomEE is an all-Apache Java EE certif…   97        [OK]
bitnami/tomcat                                 Bitnami Tomcat Docker Image                     45                   [OK]
arm32v7/tomcat                                 Apache Tomcat is an open source implementati…   11
rightctrl/tomcat                               CentOS , Oracle Java, tomcat application ssl…   7                    [OK]
arm64v8/tomcat                                 Apache Tomcat is an open source implementati…   7
jelastic/tomcat                                An image of the Tomcat Java application serv…   4
amd64/tomcat                                   Apache Tomcat is an open source implementati…   4
tomcat2111/pisignage-server                    PiSignage Server                                3                    [OK]
oobsri/tomcat8                                 Testing CI Jobs with different names.           2
cfje/tomcat-resource                           Tomcat Concourse Resource                       2
appsvc/tomcat                                                                                  1
chenyufeng/tomcat-centos                       tomcat基于centos6的镜像                              1                    [OK]
ppc64le/tomcat                                 Apache Tomcat is an open source implementati…   1
tomcatling/jupyterhub_aws                                                                      1
tomcat2111/papercut-mf                         PaperCut MF Application Server                  0
softwareplant/tomcat                           Tomcat images for jira-cloud testing            0                    [OK]
tomcatengineering/pg_backup_rotated            Clone of martianrock/pg_backup_rotated but w…   0
misolims/miso-base                             MySQL 5.7 Database and Tomcat 8 Server neede…   0
s390x/tomcat                                   Apache Tomcat is an open source implementati…   0
tomcat2111/bitbucket-pipelines-elasticsearch   Elasticsearch for Bitbucket's Pipelines         0
semoss/docker-tomcat                           Tomcat, Java, Maven, and Git on top of debian   0                    [OK]
tomcat2111/phpredisadmin                       This is a Docker image for phpredisadmin        0                    [OK]
secoresearch/tomcat-varnish                    Tomcat and Varnish 5.0                          0                    [OK]
tomcat0823/auto1                                                                               0
PS C:\Users\mao\Desktop>
```



### 2. 下载

命令：docker pull tomcat

```sh
PS C:\Users\mao\Desktop> docker pull tomcat
Using default tag: latest
latest: Pulling from library/tomcat
e756f3fdd6a3: Pull complete
bf168a674899: Pull complete
e604223835cc: Pull complete
6d5c91c4cd86: Pull complete
5e20d165240e: Pull complete
1334d60df9a8: Pull complete
16c2728dcd90: Pull complete
05288798d23d: Pull complete
c022dc2b2581: Pull complete
d86ac2f896ee: Pull complete
Digest: sha256:b4e84cff017ff5202cb760ccb1373dd950158f926d6afb04bd5e9f7337291501
Status: Downloaded newer image for tomcat:latest
docker.io/library/tomcat:latest
PS C:\Users\mao\Desktop>
```



### 3. 查看是否拉取成功

docker images

```sh
PS C:\Users\mao\Desktop> docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
tomcat       latest    c795915cb678   2 weeks ago   680MB
redis        latest    53aa81e8adfa   2 weeks ago   117MB
mysql        latest    65b636d5542b   2 weeks ago   524MB
ubuntu       latest    d2e4e1f51132   6 weeks ago   77.8MB
PS C:\Users\mao\Desktop>
```



### 4. 运行

 命令：docker run -d --name tomcat1 -p8080:8080 tomcat

命令：docker run -d --name tomcat1 -p8080:8080 --privileged=true  -v H:/Docker/tomcat/:/usr/local/tomcat/webapps  tomcat 



```sh
PS C:\Users\mao\Desktop> docker run -d --name tomcat1 -p8080:8080 --privileged=true  -v H:/Docker/tomcat/:/usr/local/tomcat/webapps  tomcat
3ca156e4541d4828906f34700fd690ea9bce0e543117ed27724cea70cf9f5780
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                      PORTS                    NAMES
3ca156e4541d   tomcat    "catalina.sh run"        15 seconds ago   Up 14 seconds               0.0.0.0:8080->8080/tcp   tomcat1
3948921a6099   redis     "docker-entrypoint.s…"   52 minutes ago   Exited (0) 11 minutes ago                            redis1
PS C:\Users\mao\Desktop>
```



### 5. 测试

访问8080端口





## 安装mysql

### 1. 搜索和下载

命令：docker pull mysql



### 2. 查看是否拉取成功

docker images



```sh
PS C:\Users\mao\Desktop> docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
tomcat       latest    c795915cb678   2 weeks ago   680MB
redis        latest    53aa81e8adfa   2 weeks ago   117MB
mysql        latest    65b636d5542b   2 weeks ago   524MB
ubuntu       latest    d2e4e1f51132   6 weeks ago   77.8MB
PS C:\Users\mao\Desktop>
```



### 3. 运行

命令：docker run -p 3307:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql

命令：

```sh
docker run -d -p 3307:3306 --privileged=true -v H:/Docker/mysql/log/:/var/log/mysql -v H:/Docker/mysql/data:/var/lib/mysql -v H:/Docker/mysql/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456  --name mysql1 mysql
```



```sh
PS C:\Users\mao\Desktop> docker run -d -p 3307:3306 --privileged=true -v H:/Docker/mysql/log/:/var/log/mysql -v H:/Docker/mysql/data:/var/lib/mysql -v H:/Docker/mysql/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456  --name mysql1 mysql
2d379d342bb6879bef7d9e359cb532f6146c108b26a72fcc2e7085301331d200
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED             STATUS                       PORTS                               NAMES
2d379d342bb6   mysql     "docker-entrypoint.s…"   48 seconds ago      Up 47 seconds                33060/tcp, 0.0.0.0:3307->3306/tcp   mysql1
3ca156e4541d   tomcat    "catalina.sh run"        22 minutes ago      Exited (143) 8 minutes ago                                       tomcat1
3948921a6099   redis     "docker-entrypoint.s…"   About an hour ago   Exited (0) 34 minutes ago                                        redis1
PS C:\Users\mao\Desktop>
```



### 4. 在H:\Docker\mysql\conf下创建my.conf

写入一下内容

```sh
default_character_set=utf8
collation_server = utf8_general_ci
character_set_server = utf8
```



### 5. 测试

```sh
PS C:\Users\mao\Desktop> mysql -P 3307 -u root -p
Enter password: ******
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 8.0.29 MySQL Community Server - GPL

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)

mysql>
```



### 6. 验证编码

```sh
mysql> SHOW VARIABLES LIKE 'character%'
    -> ;
+--------------------------+--------------------------------+
| Variable_name            | Value                          |
+--------------------------+--------------------------------+
| character_set_client     | gbk                            |
| character_set_connection | gbk                            |
| character_set_database   | utf8mb4                        |
| character_set_filesystem | binary                         |
| character_set_results    | gbk                            |
| character_set_server     | utf8mb4                        |
| character_set_system     | utf8mb3                        |
| character_sets_dir       | /usr/share/mysql-8.0/charsets/ |
+--------------------------+--------------------------------+
8 rows in set (0.01 sec)
```



### 7. 数据测试

```sh
mysql> create database db1
    -> ;
Query OK, 1 row affected (0.12 sec)

mysql> use db1
Database changed
mysql> create table test(id int,name char(6));
Query OK, 0 rows affected (0.37 sec)

mysql> show tables;
+---------------+
| Tables_in_db1 |
+---------------+
| test          |
+---------------+
1 row in set (0.00 sec)

mysql> insert into test values(1,"1234");
Query OK, 1 row affected (0.09 sec)

mysql> insert into test values(1,"你好");
Query OK, 1 row affected (0.09 sec)

mysql> select * from test;
+------+------+
| id   | name |
+------+------+
|    1 | 1234 |
|    1 | 你好 |
+------+------+
2 rows in set (0.00 sec)

mysql>
```







## 安装redis

### 1. 搜索和下载

命令：docker pull redis



### 2. 查看是否拉取成功

docker images



```sh
PS C:\Users\mao\Desktop> docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
tomcat       latest    c795915cb678   2 weeks ago   680MB
redis        latest    53aa81e8adfa   2 weeks ago   117MB
mysql        latest    65b636d5542b   2 weeks ago   524MB
ubuntu       latest    d2e4e1f51132   6 weeks ago   77.8MB
PS C:\Users\mao\Desktop>
```



### 3. 运行

命令：

```sh
docker run -p 6380:6379 --name redis1 --privileged=true -v H:/Docker/redis/conf:/etc/redis/ -v H:/Docker/redis/data/:/data -d redis redis-server /etc/redis/redis.conf
```



```sh
PS C:\Users\mao\Desktop> docker run -p 6380:6379 --name redis1 --privileged=true -v H:/Docker/redis/conf:/etc/redis/ -v H:/Docker/redis/data/:/data -d redis redis-server /etc/redis/redis.conf
8a80769441281ea5f5872c3656eea04c7728b026baae1ea7a13d540fc4daecc3
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                      PORTS                    NAMES
8a8076944128   redis     "docker-entrypoint.s…"   31 seconds ago   Exited (1) 28 seconds ago                            redis1
81902b3cfdc4   mysql     "docker-entrypoint.s…"   11 hours ago     Exited (0) 11 hours ago                              mysql3
24ee3e986397   mysql     "docker-entrypoint.s…"   11 hours ago     Exited (0) 11 hours ago                              mysql2
2d379d342bb6   mysql     "docker-entrypoint.s…"   36 hours ago     Exited (0) 12 hours ago                              mysql1
3ca156e4541d   tomcat    "catalina.sh run"        37 hours ago     Up About an hour            0.0.0.0:8080->8080/tcp   tomcat1
PS C:\Users\mao\Desktop>
```



```sh
PS C:\Users\mao\Desktop> docker logs 8a8076944128
1:C 15 Jun 2022 03:06:54.166 # Fatal error, can't open config file '/etc/redis/redis.conf': No such file or directory
PS C:\Users\mao\Desktop>
```



### 4. 创建配置文件

在H:\Docker\redis\conf目录下创建redis.conf文件

文件内容可以从其他redis服务器里复制并粘贴



```sh
PS H:\Docker\redis\conf> ls


    目录: H:\Docker\redis\conf


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         2022/6/15     11:04          66028 redis.conf


PS H:\Docker\redis\conf>
```

```sh
# pwd
/data
# cd ..
# cd etc
# cd redis
# ls
redis.conf
#
```



### 5. 重启redis服务器

命令：

```sh
docker restart redis1
```



```sh
PS C:\Users\mao\Desktop> docker restart redis1
redis1
PS C:\Users\mao\Desktop>
```



### 6. 查看日志

命令：

```sh
docker logs redis1
```



```sh
1:C 15 Jun 2022 03:14:13.190 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 15 Jun 2022 03:14:13.190 # Redis version=7.0.0, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 15 Jun 2022 03:14:13.190 # Configuration loaded
1:M 15 Jun 2022 03:14:13.191 * monotonic clock: POSIX clock_gettime
                _._
           _.-``__ ''-._
      _.-``    `.  `_.  ''-._           Redis 7.0.0 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 1
  `-._    `-._  `-./  _.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |           https://redis.io
  `-._    `-._`-.__.-'_.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |
  `-._    `-._`-.__.-'_.-'    _.-'
      `-._    `-.__.-'    _.-'
          `-._        _.-'
              `-.__.-'

1:M 15 Jun 2022 03:14:13.191 # Server initialized
1:M 15 Jun 2022 03:14:13.191 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
1:M 15 Jun 2022 03:14:13.192 * The AOF directory appendonlydir doesn't exist
1:M 15 Jun 2022 03:14:13.193 * Loading RDB produced by version 7.0.0
1:M 15 Jun 2022 03:14:13.193 * RDB age 1 seconds
1:M 15 Jun 2022 03:14:13.193 * RDB memory usage when created 0.82 Mb
1:M 15 Jun 2022 03:14:13.193 * Done loading RDB, keys loaded: 0, keys expired: 0.
1:M 15 Jun 2022 03:14:13.193 * DB loaded from disk: 0.002 seconds
1:M 15 Jun 2022 03:14:13.193 * Ready to accept connections
PS C:\Users\mao\Desktop>
```



配置文件成功加载



### 7. 连接服务器

外部使用命令

```sh
redis-cli -p 6380
```



```sh
C:\Users\mao>redis-cli -p 6380
127.0.0.1:6380> ping
PONG
127.0.0.1:6380>
```



或者

```sh
docker exec -it redis1 /bin/bash
redis-cli
```



```sh
PS C:\Users\mao\Desktop> docker exec -it redis1 /bin/bash
root@8a8076944128:/data# redis-cli
127.0.0.1:6379> ping
PONG
127.0.0.1:6379>
```



### 8. 测试

数据测试

```sh
127.0.0.1:6379> set a 1
OK
127.0.0.1:6379> keys *
1) "a"
127.0.0.1:6379> get a
"1"
127.0.0.1:6379>

```

配置文件测试

当前为16个库

```sh
127.0.0.1:6379> select 15
OK
127.0.0.1:6379[15]> select 16
(error) ERR DB index is out of range
127.0.0.1:6379[15]>
```



更改库的大小，把库的数量更改为2



重启服务器



再次访问

```sh
127.0.0.1:6380> select 1
OK
127.0.0.1:6380[1]> select 2
(error) ERR DB index is out of range
127.0.0.1:6380[1]>
```



配置文件有效







# DockerFile

## 是什么？

Dockerfile是用来构建Docker镜像的文本文件，是由一条条构建镜像所需的指令和参数构成的脚本。

Dockerfile，需要定义一个Dockerfile，Dockerfile定义了进程需要的一切东西。Dockerfile涉及的内容包括执行代码或者是文件、环境变量、依赖包、运行时环境、动态链接库、操作系统的发行版、服务进程和内核进程(当应用进程需要和系统服务和内核进程打交道，这时需要考虑如何设计namespace的权限控制)等等;



## 构建过程

* 每条保留字指令都必须为大写字母且后面要跟随至少一个参数
* 指令按照从上到下，顺序执行
* #表示注释
* 每条指令都会创建一个新的镜像层并对镜像进行提交



1. docker从基础镜像运行一个容器
2. 执行一条指令并对容器作出修改
3. 执行类似docker commit的操作提交一个新的镜像层
4. docker再基于刚提交的镜像运行一个新容器
5. 执行dockerfile中的下一条指令直到所有指令都执行完成





## 保留字

### FROM

基础镜像，当前新镜像是基于哪个镜像的，指定一个已经存在的镜像作为模板，第一条必须是from



### MAINTAINER

maintainer，镜像维护者的姓名和邮箱地址



### RUN

容器构建时需要运行的命令

一共有两种模式：

* shell模式：RUN 命令
* exec模式：RUN["可执行文件","参数1","参数2"......]

RUN在docker build时运行



### EXPOSE

当前容器对外暴露出的端口



### WORKDIR

指定在创建容器后，终端默认登陆的进来工作目录，一个落脚点



### USER

指定该镜像以什么样的用户去执行，如果都不指定，默认是root



### ENV

用来在构建镜像过程中设置环境变量

```SH
ENV 变量名 变量值
```

比如：

```sh
ENV JAVA_HOME /usr/test
```



### ADD

将宿主机目录下的文件拷贝进镜像且会自动处理URL和解压tar压缩包



### COPY

类似ADD，拷贝文件和目录到镜像中，但是不解压

```sh
COPY src dest
```



### VOLUME

容器数据卷，用于数据保存和持久化工作



### CMD

指定容器启动后的要干的事情

一共有两种模式：

* shell模式：RUN 命令
* exec模式：RUN["可执行文件","参数1","参数2"......]

Dockerfile 中可以有多个 CMD 指令，但只有最后一个生效，CMD 会被 docker run 之后的参数替换

CMD是在docker run 时运行。



### ENTRYPOINT

类似于 CMD 指令，但是ENTRYPOINT不会被docker run后面的命令覆盖，而且这些命令行参数会被当作参数送给 ENTRYPOINT 指令指定的程序

ENTRYPOINT可以和CMD一起用，一般是变参才会使用 CMD ，这里的 CMD 等于是在给 ENTRYPOINT 传参。

当指定了ENTRYPOINT后，CMD的含义就发生了变化，不再是直接运行其命令而是将CMD的内容作为参数传递给ENTRYPOINT指令，他两个组合会变成 ENTRYPOINT CMD



如果 Dockerfile 中如果存在多个 ENTRYPOINT 指令，仅最后一个生效。



### 示例

```dockerfile
#
# NOTE: THIS DOCKERFILE IS GENERATED VIA "apply-templates.sh"
#
# PLEASE DO NOT EDIT IT DIRECTLY.
#

FROM amazoncorretto:17

ENV CATALINA_HOME /usr/local/tomcat
ENV PATH $CATALINA_HOME/bin:$PATH
RUN mkdir -p "$CATALINA_HOME"
WORKDIR $CATALINA_HOME

# let "Tomcat Native" live somewhere isolated
ENV TOMCAT_NATIVE_LIBDIR $CATALINA_HOME/native-jni-lib
ENV LD_LIBRARY_PATH ${LD_LIBRARY_PATH:+$LD_LIBRARY_PATH:}$TOMCAT_NATIVE_LIBDIR

# see https://www.apache.org/dist/tomcat/tomcat-10/KEYS
# see also "versions.sh" (https://github.com/docker-library/tomcat/blob/master/versions.sh)
ENV GPG_KEYS A9C5DF4D22E99998D9875A5110C01C5A2F6059E7

ENV TOMCAT_MAJOR 10
ENV TOMCAT_VERSION 10.0.22
ENV TOMCAT_SHA512 fe46db8794f066882b30e7a94bd8d3dbcf29e8e8ffaf67c1355846755745a7c9eafd124819283f218bcf410921a485b44b57b56fd6251fb99d67d95f3dd36826

RUN set -eux; \
	\
# http://yum.baseurl.org/wiki/YumDB.html
	if ! command -v yumdb > /dev/null; then \
		yum install -y yum-utils; \
		yumdb set reason dep yum-utils; \
	fi; \
# a helper function to "yum install" things, but only if they aren't installed (and to set their "reason" to "dep" so "yum autoremove" can purge them for us)
	_yum_install_temporary() { ( set -eu +x; \
		local pkg todo=''; \
		for pkg; do \
			if ! rpm --query "$pkg" > /dev/null 2>&1; then \
				todo="$todo $pkg"; \
			fi; \
		done; \
		if [ -n "$todo" ]; then \
			set -x; \
			yum install -y $todo; \
			yumdb set reason dep $todo; \
		fi; \
	) }; \
	_yum_install_temporary gzip tar; \
	\
	ddist() { \
		local f="$1"; shift; \
		local distFile="$1"; shift; \
		local mvnFile="${1:-}"; \
		local success=; \
		local distUrl=; \
		for distUrl in \
# https://issues.apache.org/jira/browse/INFRA-8753?focusedCommentId=14735394#comment-14735394
			"https://www.apache.org/dyn/closer.cgi?action=download&filename=$distFile" \
# if the version is outdated (or we're grabbing the .asc file), we might have to pull from the dist/archive :/
			"https://downloads.apache.org/$distFile" \
			"https://www-us.apache.org/dist/$distFile" \
			"https://www.apache.org/dist/$distFile" \
			"https://archive.apache.org/dist/$distFile" \
# if all else fails, let's try Maven (https://www.mail-archive.com/users@tomcat.apache.org/msg134940.html; https://mvnrepository.com/artifact/org.apache.tomcat/tomcat; https://repo1.maven.org/maven2/org/apache/tomcat/tomcat/)
			${mvnFile:+"https://repo1.maven.org/maven2/org/apache/tomcat/tomcat/$mvnFile"} \
		; do \
			if curl -fL -o "$f" "$distUrl" && [ -s "$f" ]; then \
				success=1; \
				break; \
			fi; \
		done; \
		[ -n "$success" ]; \
	}; \
	\
	ddist 'tomcat.tar.gz' "tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz" "$TOMCAT_VERSION/tomcat-$TOMCAT_VERSION.tar.gz"; \
	echo "$TOMCAT_SHA512 *tomcat.tar.gz" | sha512sum --strict --check -; \
	ddist 'tomcat.tar.gz.asc' "tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz.asc" "$TOMCAT_VERSION/tomcat-$TOMCAT_VERSION.tar.gz.asc"; \
	export GNUPGHOME="$(mktemp -d)"; \
	for key in $GPG_KEYS; do \
		gpg --batch --keyserver keyserver.ubuntu.com --recv-keys "$key"; \
	done; \
	gpg --batch --verify tomcat.tar.gz.asc tomcat.tar.gz; \
	tar -xf tomcat.tar.gz --strip-components=1; \
	rm bin/*.bat; \
	rm tomcat.tar.gz*; \
	command -v gpgconf && gpgconf --kill all || :; \
	rm -rf "$GNUPGHOME"; \
	\
# https://tomcat.apache.org/tomcat-9.0-doc/security-howto.html#Default_web_applications
	mv webapps webapps.dist; \
	mkdir webapps; \
# we don't delete them completely because they're frankly a pain to get back for users who do want them, and they're generally tiny (~7MB)
	\
	nativeBuildDir="$(mktemp -d)"; \
	tar -xf bin/tomcat-native.tar.gz -C "$nativeBuildDir" --strip-components=1; \
	_yum_install_temporary \
		apr-devel \
		gcc \
		make \
		openssl11-devel \
	; \
	( \
		export CATALINA_HOME="$PWD"; \
		cd "$nativeBuildDir/native"; \
		aprConfig="$(command -v apr-1-config)"; \
		./configure \
			--libdir="$TOMCAT_NATIVE_LIBDIR" \
			--prefix="$CATALINA_HOME" \
			--with-apr="$aprConfig" \
			--with-java-home="$JAVA_HOME" \
			--with-ssl=yes \
		; \
		nproc="$(nproc)"; \
		make -j "$nproc"; \
		make install; \
	); \
	rm -rf "$nativeBuildDir"; \
	rm bin/tomcat-native.tar.gz; \
	\
# mark any explicit dependencies as manually installed
	find "$TOMCAT_NATIVE_LIBDIR" -type f -executable -exec ldd '{}' ';' \
		| awk '/=>/ && $(NF-1) != "=>" { print $(NF-1) }' \
		| xargs -rt readlink -e \
		| sort -u \
		| xargs -rt rpm --query --whatprovides \
		| sort -u \
		| tee "$TOMCAT_NATIVE_LIBDIR/.dependencies.txt" \
		| xargs -r yumdb set reason user \
	; \
	\
# clean up anything added temporarily and not later marked as necessary
	yum autoremove -y; \
	yum clean all; \
	rm -rf /var/cache/yum; \
	\
# sh removes env vars it doesn't support (ones with periods)
# https://github.com/docker-library/tomcat/issues/77
	find ./bin/ -name '*.sh' -exec sed -ri 's|^#!/bin/sh$|#!/usr/bin/env bash|' '{}' +; \
	\
# fix permissions (especially for running as non-root)
# https://github.com/docker-library/tomcat/issues/35
	chmod -R +rX .; \
	chmod 777 logs temp work; \
	\
# smoke test
	catalina.sh version

# verify Tomcat Native is working properly
RUN set -eux; \
	nativeLines="$(catalina.sh configtest 2>&1)"; \
	nativeLines="$(echo "$nativeLines" | grep 'Apache Tomcat Native')"; \
	nativeLines="$(echo "$nativeLines" | sort -u)"; \
	if ! echo "$nativeLines" | grep -E 'INFO: Loaded( APR based)? Apache Tomcat Native library' >&2; then \
		echo >&2 "$nativeLines"; \
		exit 1; \
	fi

EXPOSE 8080
CMD ["catalina.sh", "run"]
```







## 自定义镜像

### java17

说明：使用Ubuntu镜像，生成包含java17，vim、ifconfig的镜像

#### 下载java17

下载页面

[Java Downloads | Oracle](https://www.oracle.com/java/technologies/downloads/#java17)

直接下载

https://download.oracle.com/java/17/latest/jdk-17_linux-x64_bin.tar.gz



#### 放入指定目录

我放入的目录是C:\Users\mao\Desktop\test



#### 新建Dockerfile文件

在刚才的目录下建立一个Dockerfile文件，要和jdk的存放目录一致



```sh
PS C:\Users\mao\Desktop\test> ls


    目录: C:\Users\mao\Desktop\test


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         2022/6/17     21:47              0 Dockerfile
-a----         2022/6/17     21:42      181203151 jdk-17_linux-x64_bin.tar.gz


PS C:\Users\mao\Desktop\test>

```



#### 编写Dockerfile文件

使用记事本打开，输入以下命令



```dockerfile
#基础镜像，当前新镜像是基于哪个镜像的，指定一个已经存在的镜像作为模板，第一条必须是from
FROM ubuntu
# 镜像维护者的姓名和邮箱地址
MAINTAINER mao<1296193245@qq.com>
#用来在构建镜像过程中设置环境变量
ENV MYPATH /usr/local
# 指定在创建容器后，终端默认登陆的进来工作目录
WORKDIR $MYPATH

#更新依赖
RUN apt-get update
#安装vim编辑器
RUN apt-get -y install vim
#安装ifconfig命令查看网络IP
RUN apt-get install net-tools
#安装java17及lib库
# RUN apt-get -y install glibc
# 创建一个文件夹
RUN mkdir /usr/local/java
#ADD 是相对路径jar,把jdk-17_linux-x64_bin.tar.gz添加到容器中,安装包必须要和Dockerfile文件在同一位置
ADD jdk-17_linux-x64_bin.tar.gz /usr/local/java/
#配置java环境变量
ENV JAVA_HOME /usr/local/java/jdk-17.0.3.1
ENV JRE_HOME $JAVA_HOME/jre
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib:$CLASSPATH
ENV PATH $JAVA_HOME/bin:$PATH
 
#暴露端口
EXPOSE 80

#打印
CMD echo $MYPATH
CMD echo "success--------------ok"
# 如果不传入参数，默认运行/bin/bash
CMD /bin/bash
```



#### 构建

构建命令：

```sh
docker build -t 新镜像名字:TAG .
```





运行命令：

```sh
docker build -t java17:1.0 .
```





```sh
PS C:\Users\mao\Desktop\test> docker build -t java17:1.0 .
[+] Building 5.1s (12/12) FINISHED
 => [internal] load build definition from Dockerfile                                                                                                       0.0s
 => => transferring dockerfile: 1.23kB                                                                                                                     0.0s
 => [internal] load .dockerignore                                                                                                                          0.0s
 => => transferring context: 2B                                                                                                                            0.0s
 => [internal] load metadata for docker.io/library/ubuntu:latest                                                                                           0.0s
 => [1/7] FROM docker.io/library/ubuntu                                                                                                                    0.0s
 => [internal] load build context                                                                                                                          0.0s
 => => transferring context: 51B                                                                                                                           0.0s
 => CACHED [2/7] WORKDIR /usr/local                                                                                                                        0.0s
 => CACHED [3/7] RUN apt-get update                                                                                                                        0.0s
 => CACHED [4/7] RUN apt-get -y install vim                                                                                                                0.0s
 => CACHED [5/7] RUN apt-get install net-tools                                                                                                             0.0s
 => [6/7] RUN mkdir /usr/local/java                                                                                                                        0.4s
 => [7/7] ADD jdk-17_linux-x64_bin.tar.gz /usr/local/java/                                                                                                 3.5s
 => exporting to image                                                                                                                                     1.2s
 => => exporting layers                                                                                                                                    1.2s
 => => writing image sha256:282982c69086e51636a387c7156a1e246a6f7109c36a663c09e0302ec2c95a03                                                               0.0s
 => => naming to docker.io/library/java17:1.0                                                                                                              0.0s

Use 'docker scan' to run Snyk tests against images to find vulnerabilities and learn how to fix them
PS C:\Users\mao\Desktop\test>
```



#### 查看镜像



```sh
docker images
```



```sh
PS C:\Users\mao\Desktop\test> docker images
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
java17       1.0       282982c69086   12 seconds ago   489MB
tomcat       latest    c795915cb678   2 weeks ago      680MB
redis        latest    53aa81e8adfa   2 weeks ago      117MB
mysql        latest    65b636d5542b   2 weeks ago      524MB
ubuntu       latest    d2e4e1f51132   6 weeks ago      77.8MB
PS C:\Users\mao\Desktop\test>
```



#### 运行容器

```sh
docker run -it --name java17 java17:1.0 /bin/bash
```



```sh
PS C:\Users\mao\Desktop\test> docker run -it --name java17 java17:1.0 /bin/bash
root@20c9959d6873:/usr/local# pwd
/usr/local
root@20c9959d6873:/usr/local# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.2  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 02:42:ac:11:00:02  txqueuelen 0  (Ethernet)
        RX packets 10  bytes 876 (876.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

root@20c9959d6873:/usr/local# vim
root@20c9959d6873:/usr/local# ls
bin  etc  games  include  java  lib  man  sbin  share  src  test.txt
root@20c9959d6873:/usr/local# java -version
java version "17.0.3.1" 2022-04-22 LTS
Java(TM) SE Runtime Environment (build 17.0.3.1+2-LTS-6)
Java HotSpot(TM) 64-Bit Server VM (build 17.0.3.1+2-LTS-6, mixed mode, sharing)
root@20c9959d6873:/usr/local#
```







# Docker运行后端服务

## 创建springboot项目

项目名称为Docker_boot

## maven

maven的pom.xml文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.7.0</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>mao</groupId>
    <artifactId>Docker_boot</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>Docker_boot</name>
    <description>Docker_boot</description>
    <properties>
        <java.version>11</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>

```



## 配置

application.yml文件：



```yaml

#多环境开发---单文件
spring:
  profiles:
    # 当前环境
    active: dev

#-----通用环境---------------------------------------------------------


server:
  #配置端口号
  port: 8082



---
#------开发环境--------------------------------------------------------

spring:
  config:
    activate:
      on-profile: dev




# 开启debug模式，输出调试信息，常用于检查系统运行状况
#debug: true

# 设置日志级别，root表示根节点，即整体应用日志级别
logging:
 # 日志输出到文件的文件名
  file:
     name: server.log
  # 字符集
  charset:
    file: UTF-8
  # 分文件
  logback:
    rollingpolicy:
      #最大文件大小
      max-file-size: 16KB
      # 文件格式
      file-name-pattern: server_log-%d{yyyy/MM月/dd日/}%i.log
  # 设置日志组
  group:
  # 自定义组名，设置当前组中所包含的包
    mao_pro: mao
  level:
    root: info
    # 为对应组设置日志级别
    mao_pro: debug
    # 日志输出格式
# pattern:
  # console: "%d %clr(%p) --- [%16t] %clr(%-40.40c){cyan} : %m %n"




---
#----生产环境----------------------------------------------------


spring:
  config:
    activate:
      on-profile: pro






# 开启debug模式，输出调试信息，常用于检查系统运行状况
#debug: true

# 设置日志级别，root表示根节点，即整体应用日志级别
logging:
  # 日志输出到文件的文件名
  file:
    name: server.log
  # 字符集
  charset:
    file: UTF-8
  # 分文件
  logback:
    rollingpolicy:
      #最大文件大小
      max-file-size: 4MB
      # 文件格式
      file-name-pattern: server_log-%d{yyyy/MM月/dd日/}%i.log
  # 设置日志组
  group:
    # 自定义组名，设置当前组中所包含的包
    mao_pro: mao
  level:
    root: info
    # 为对应组设置日志级别
    mao_pro: warn
    # 日志输出格式
  # pattern:
  # console: "%d %clr(%p) --- [%16t] %clr(%-40.40c){cyan} : %m %n"



---
#----测试环境-----------------------------------------------------

spring:
  config:
    activate:
      on-profile: test



```





## controller



MyController：

```java
package mao.docker_boot.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import java.util.UUID;

/**
 * Project name(项目名称)：Docker_boot
 * Package(包名): mao.docker_boot.controller
 * Class(类名): MyController
 * Author(作者）: mao
 * Author QQ：1296193245
 * GitHub：https://github.com/maomao124/
 * Date(创建日期)： 2022/6/17
 * Time(创建时间)： 23:01
 * Version(版本): 1.0
 * Description(描述)： 业务类
 */

@RestController
public class MyController
{
    /**
     * 端口号，从配置文件里读
     */
    @Value("${server.port}")
    private String port;

    @RequestMapping("/docker")
    public String helloDocker()
    {
        return "hello docker and spring boot"+"<br>端口号："+port+"<br>"+ UUID.randomUUID();
    }

    @RequestMapping(value ="/index",method = RequestMethod.GET)
    public String index()
    {
        return "服务端口号: "+""+port+"<br>"+UUID.randomUUID();
    }
}

```





## 启动测试



```sh
C:\Users\mao\.jdks\openjdk-16.0.2\bin\java.exe -XX:TieredStopAtLevel=1 -noverify -Dspring.output.ansi.enabled=always "-javaagent:C:\Program Files\JetBrains\IntelliJ IDEA 2021.2.2\lib\idea_rt.jar=54055:C:\Program Files\JetBrains\IntelliJ IDEA 2021.2.2\bin" -Dcom.sun.management.jmxremote -Dspring.jmx.enabled=true -Dspring.liveBeansView.mbeanDomain -Dspring.application.admin.enabled=true -Dfile.encoding=UTF-8 -classpath H:\程序\大三下期\Docker_boot\target\classes;C:\Users\mao\.m2\repository\org\springframework\boot\spring-boot-starter-web\2.7.0\spring-boot-starter-web-2.7.0.jar;C:\Users\mao\.m2\repository\org\springframework\boot\spring-boot-starter\2.7.0\spring-boot-starter-2.7.0.jar;C:\Users\mao\.m2\repository\org\springframework\boot\spring-boot\2.7.0\spring-boot-2.7.0.jar;C:\Users\mao\.m2\repository\org\springframework\boot\spring-boot-autoconfigure\2.7.0\spring-boot-autoconfigure-2.7.0.jar;C:\Users\mao\.m2\repository\org\springframework\boot\spring-boot-starter-logging\2.7.0\spring-boot-starter-logging-2.7.0.jar;C:\Users\mao\.m2\repository\ch\qos\logback\logback-classic\1.2.11\logback-classic-1.2.11.jar;C:\Users\mao\.m2\repository\ch\qos\logback\logback-core\1.2.11\logback-core-1.2.11.jar;C:\Users\mao\.m2\repository\org\apache\logging\log4j\log4j-to-slf4j\2.17.2\log4j-to-slf4j-2.17.2.jar;C:\Users\mao\.m2\repository\org\apache\logging\log4j\log4j-api\2.17.2\log4j-api-2.17.2.jar;C:\Users\mao\.m2\repository\org\slf4j\jul-to-slf4j\1.7.36\jul-to-slf4j-1.7.36.jar;C:\Users\mao\.m2\repository\jakarta\annotation\jakarta.annotation-api\1.3.5\jakarta.annotation-api-1.3.5.jar;C:\Users\mao\.m2\repository\org\yaml\snakeyaml\1.30\snakeyaml-1.30.jar;C:\Users\mao\.m2\repository\org\springframework\boot\spring-boot-starter-json\2.7.0\spring-boot-starter-json-2.7.0.jar;C:\Users\mao\.m2\repository\com\fasterxml\jackson\core\jackson-databind\2.13.3\jackson-databind-2.13.3.jar;C:\Users\mao\.m2\repository\com\fasterxml\jackson\core\jackson-annotations\2.13.3\jackson-annotations-2.13.3.jar;C:\Users\mao\.m2\repository\com\fasterxml\jackson\core\jackson-core\2.13.3\jackson-core-2.13.3.jar;C:\Users\mao\.m2\repository\com\fasterxml\jackson\datatype\jackson-datatype-jdk8\2.13.3\jackson-datatype-jdk8-2.13.3.jar;C:\Users\mao\.m2\repository\com\fasterxml\jackson\datatype\jackson-datatype-jsr310\2.13.3\jackson-datatype-jsr310-2.13.3.jar;C:\Users\mao\.m2\repository\com\fasterxml\jackson\module\jackson-module-parameter-names\2.13.3\jackson-module-parameter-names-2.13.3.jar;C:\Users\mao\.m2\repository\org\springframework\boot\spring-boot-starter-tomcat\2.7.0\spring-boot-starter-tomcat-2.7.0.jar;C:\Users\mao\.m2\repository\org\apache\tomcat\embed\tomcat-embed-core\9.0.63\tomcat-embed-core-9.0.63.jar;C:\Users\mao\.m2\repository\org\apache\tomcat\embed\tomcat-embed-el\9.0.63\tomcat-embed-el-9.0.63.jar;C:\Users\mao\.m2\repository\org\apache\tomcat\embed\tomcat-embed-websocket\9.0.63\tomcat-embed-websocket-9.0.63.jar;C:\Users\mao\.m2\repository\org\springframework\spring-web\5.3.20\spring-web-5.3.20.jar;C:\Users\mao\.m2\repository\org\springframework\spring-beans\5.3.20\spring-beans-5.3.20.jar;C:\Users\mao\.m2\repository\org\springframework\spring-webmvc\5.3.20\spring-webmvc-5.3.20.jar;C:\Users\mao\.m2\repository\org\springframework\spring-aop\5.3.20\spring-aop-5.3.20.jar;C:\Users\mao\.m2\repository\org\springframework\spring-context\5.3.20\spring-context-5.3.20.jar;C:\Users\mao\.m2\repository\org\springframework\spring-expression\5.3.20\spring-expression-5.3.20.jar;C:\Users\mao\.m2\repository\org\slf4j\slf4j-api\1.7.36\slf4j-api-1.7.36.jar;C:\Users\mao\.m2\repository\org\springframework\spring-core\5.3.20\spring-core-5.3.20.jar;C:\Users\mao\.m2\repository\org\springframework\spring-jcl\5.3.20\spring-jcl-5.3.20.jar mao.docker_boot.DockerBootApplication
OpenJDK 64-Bit Server VM warning: Options -Xverify:none and -noverify were deprecated in JDK 13 and will likely be removed in a future release.

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.7.0)

2022-06-17 23:07:20.341  INFO 19932 --- [           main] mao.docker_boot.DockerBootApplication    : Starting DockerBootApplication using Java 16.0.2 on mao with PID 19932 (H:\程序\大三下期\Docker_boot\target\classes started by mao in H:\程序\大三下期\Docker_boot)
2022-06-17 23:07:20.343 DEBUG 19932 --- [           main] mao.docker_boot.DockerBootApplication    : Running with Spring Boot v2.7.0, Spring v5.3.20
2022-06-17 23:07:20.344  INFO 19932 --- [           main] mao.docker_boot.DockerBootApplication    : The following 1 profile is active: "dev"
2022-06-17 23:07:20.922  INFO 19932 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8082 (http)
2022-06-17 23:07:20.929  INFO 19932 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2022-06-17 23:07:20.929  INFO 19932 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.63]
2022-06-17 23:07:20.998  INFO 19932 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2022-06-17 23:07:20.999  INFO 19932 --- [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 618 ms
2022-06-17 23:07:21.227  INFO 19932 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8082 (http) with context path ''
2022-06-17 23:07:21.236  INFO 19932 --- [           main] mao.docker_boot.DockerBootApplication    : Started DockerBootApplication in 1.213 seconds (JVM running for 1.648)

```



访问

[localhost:8082/index](http://localhost:8082/index)

[localhost:8082/docker](http://localhost:8082/docker)



无问题



## 生成jar包



```sh
mvn package
```

或者

```sh
mvn package -DskipTests
```





```sh
PS H:\程序\大三下期\Docker_boot> mvn package
[INFO] Scanning for projects...
[INFO]
[INFO] --------------------------< mao:Docker_boot >---------------------------
[INFO] Building Docker_boot 0.0.1-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-resources-plugin:3.2.0:resources (default-resources) @ Docker_boot ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Using 'UTF-8' encoding to copy filtered properties files.
[INFO] Copying 1 resource
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.10.1:compile (default-compile) @ Docker_boot ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 2 source files to H:\程序\大三下期\Docker_boot\target\classes
[INFO]
[INFO] --- maven-resources-plugin:3.2.0:testResources (default-testResources) @ Docker_boot ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Using 'UTF-8' encoding to copy filtered properties files.
[INFO] skip non existing resourceDirectory H:\程序\大三下期\Docker_boot\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.10.1:testCompile (default-testCompile) @ Docker_boot ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to H:\程序\大三下期\Docker_boot\target\test-classes
[INFO]
[INFO] --- maven-surefire-plugin:2.22.2:test (default-test) @ Docker_boot ---
[INFO]
[INFO] -------------------------------------------------------
[INFO]  T E S T S
[INFO] -------------------------------------------------------
[INFO] Running mao.docker_boot.DockerBootApplicationTests
23:10:09.954 [main] DEBUG org.springframework.test.context.BootstrapUtils - Instantiating CacheAwareContextLoaderDelegate from class [org.springframework.test.context.cache.DefaultCacheAwareContextLoaderDelegate]
23:10:09.962 [main] DEBUG org.springframework.test.context.BootstrapUtils - Instantiating BootstrapContext using constructor [public org.springframework.test.context.support.DefaultBootstrapContext(java.lang.Class,org.springframework.test.context.CacheAwareContextLoaderDelegate)]
23:10:09.992 [main] DEBUG org.springframework.test.context.BootstrapUtils - Instantiating TestContextBootstrapper for test class [mao.docker_boot.DockerBootApplicationTests] from class [org.springframework.boot.test.context.SpringBootTestContextBootstrapper]
23:10:10.003 [main] INFO org.springframework.boot.test.context.SpringBootTestContextBootstrapper - Neither @ContextConfiguration nor @ContextHierarchy found for test class [mao.docker_boot.DockerBootApplicationTests], using SpringBootContextLoader
23:10:10.006 [main] DEBUG org.springframework.test.context.support.AbstractContextLoader - Did not detect default resource location for test class [mao.docker_boot.DockerBootApplicationTests]: class path resource [mao/docker_boot/DockerBootApplicationTests-context.xml] does not exist
23:10:10.007 [main] DEBUG org.springframework.test.context.support.AbstractContextLoader - Did not detect default resource location for test class [mao.docker_boot.DockerBootApplicationTests]: class path resource [mao/docker_boot/DockerBootApplicationTestsContext.groovy] does not exist
23:10:10.007 [main] INFO org.springframework.test.context.support.AbstractContextLoader - Could not detect default resource locations for test class [mao.docker_boot.DockerBootApplicationTests]: no resource found for suffixes {-context.xml, Context.groovy}.
23:10:10.007 [main] INFO org.springframework.test.context.support.AnnotationConfigContextLoaderUtils - Could not detect default configuration classes for test class [mao.docker_boot.DockerBootApplicationTests]: DockerBootApplicationTests does not declare any static, non-private, non-final, nested classes annotated with @Configuration.
23:10:10.045 [main] DEBUG org.springframework.test.context.support.ActiveProfilesUtils - Could not find an 'annotation declaring class' for annotation type [org.springframework.test.context.ActiveProfiles] and class [mao.docker_boot.DockerBootApplicationTests]
23:10:10.092 [main] DEBUG org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider - Identified candidate component class: file [H:\程序\大三下期\Docker_boot\target\classes\mao\docker_boot\DockerBootApplication.class]
23:10:10.093 [main] INFO org.springframework.boot.test.context.SpringBootTestContextBootstrapper - Found @SpringBootConfiguration mao.docker_boot.DockerBootApplication for test class mao.docker_boot.DockerBootApplicationTests
23:10:10.173 [main] DEBUG org.springframework.boot.test.context.SpringBootTestContextBootstrapper - @TestExecutionListeners is not present for class [mao.docker_boot.DockerBootApplicationTests]: using defaults.
23:10:10.174 [main] INFO org.springframework.boot.test.context.SpringBootTestContextBootstrapper - Loaded default TestExecutionListener class names from location [META-INF/spring.factories]: [org.springframework.boot.test.mock.mockito.MockitoTestExecutionListener, org.springframework.boot.test.mock.mockito.ResetMocksTestExecutionListener, org.springframework.boot.test.autoconfigure.restdocs.RestDocsTestExecutionListener, org.springframework.boot.test.autoconfigure.web.client.MockRestServiceServerResetTestExecutionListener, org.springframework.boot.test.autoconfigure.web.servlet.MockMvcPrintOnlyOnFailureTestExecutionListener, org.springframework.boot.test.autoconfigure.web.servlet.WebDriverTestExecutionListener, org.springframework.boot.test.autoconfigure.webservices.client.MockWebServiceServerTestExecutionListener, org.springframework.test.context.web.ServletTestExecutionListener, org.springframework.test.context.support.DirtiesContextBeforeModesTestExecutionListener, org.springframework.test.context.event.ApplicationEventsTestExecutionListener, org.springframework.test.context.support.DependencyInjectionTestExecutionListener, org.springframework.test.context.support.DirtiesContextTestExecutionListener, org.springframework.test.context.transaction.TransactionalTestExecutionListener, org.springframework.test.context.jdbc.SqlScriptsTestExecutionListener, org.springframework.test.context.event.EventPublishingTestExecutionListener]
23:10:10.185 [main] DEBUG org.springframework.boot.test.context.SpringBootTestContextBootstrapper - Skipping candidate TestExecutionListener [org.springframework.test.context.transaction.TransactionalTestExecutionListener] due to a missing dependency. Specify custom listener classes or make the default listener classes and their required dependencies available. Offending class: [org/springframework/transaction/interceptor/TransactionAttributeSource]
23:10:10.185 [main] DEBUG org.springframework.boot.test.context.SpringBootTestContextBootstrapper - Skipping candidate TestExecutionListener [org.springframework.test.context.jdbc.SqlScriptsTestExecutionListener] due to a missing dependency. Specify custom listener classes or make the default listener classes and their required dependencies available. Offending class: [org/springframework/transaction/interceptor/TransactionAttribute]
23:10:10.185 [main] INFO org.springframework.boot.test.context.SpringBootTestContextBootstrapper - Using TestExecutionListeners: [org.springframework.test.context.web.ServletTestExecutionListener@20f5281c, org.springframework.test.context.support.DirtiesContextBeforeModesTestExecutionListener@56c4278e, org.springframework.test.context.event.ApplicationEventsTestExecutionListener@301eda63, org.springframework.boot.test.mock.mockito.MockitoTestExecutionListener@3d246ea3, org.springframework.boot.test.autoconfigure.SpringBootDependencyInjectionTestExecutionListener@341814d3, org.springframework.test.context.support.DirtiesContextTestExecutionListener@4397ad89, org.springframework.test.context.event.EventPublishingTestExecutionListener@59cba5a, org.springframework.boot.test.mock.mockito.ResetMocksTestExecutionListener@1bd39d3c, org.springframework.boot.test.autoconfigure.restdocs.RestDocsTestExecutionListener@6f19ac19, org.springframework.boot.test.autoconfigure.web.client.MockRestServiceServerResetTestExecutionListener@119cbf96, org.springframework.boot.test.autoconfigure.web.servlet.MockMvcPrintOnlyOnFailureTestExecutionListener@71329995, org.springframework.boot.test.autoconfigure.web.servlet.WebDriverTestExecutionListener@768fc0f2, org.springframework.boot.test.autoconfigure.webservices.client.MockWebServiceServerTestExecutionListener@5454d35e]
23:10:10.189 [main] DEBUG org.springframework.test.context.support.AbstractDirtiesContextTestExecutionListener - Before test class: context [DefaultTestContext@7d9e8ef7 testClass = DockerBootApplicationTests, testInstance = [null], testMethod = [null], testException = [null], mergedContextConfiguration = [WebMergedContextConfiguration@f107c50 testClass = DockerBootApplicationTests, locations = '{}', classes = '{class mao.docker_boot.DockerBootApplication}', contextInitializerClasses = '[]', activeProfiles = '{}', propertySourceLocations = '{}', propertySourceProperties = '{org.springframework.boot.test.context.SpringBootTestContextBootstrapper=true}', contextCustomizers = set[org.springframework.boot.test.context.filter.ExcludeFilterContextCustomizer@4944252c, org.springframework.boot.test.json.DuplicateJsonObjectContextCustomizerFactory$DuplicateJsonObjectContextCustomizer@272ed83b, org.springframework.boot.test.mock.mockito.MockitoContextCustomizer@0, org.springframework.boot.test.web.client.TestRestTemplateContextCustomizer@15bbf42f, org.springframework.boot.test.autoconfigure.actuate.metrics.MetricsExportContextCustomizerFactory$DisableMetricExportContextCustomizer@20f5239f, org.springframework.boot.test.autoconfigure.properties.PropertyMappingContextCustomizer@0, org.springframework.boot.test.autoconfigure.web.servlet.WebDriverContextCustomizerFactory$Customizer@3e84448c, org.springframework.boot.test.context.SpringBootTestArgs@1, org.springframework.boot.test.context.SpringBootTestWebEnvironment@4b1c1ea0], resourceBasePath = 'src/main/webapp', contextLoader = 'org.springframework.boot.test.context.SpringBootContextLoader', parent = [null]], attributes = map['org.springframework.test.context.web.ServletTestExecutionListener.activateListener' -> true]], class annotated with @DirtiesContext [false] with mode [null].

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.7.0)

2022-06-17 23:10:10.592  INFO 296 --- [           main] m.d.DockerBootApplicationTests           : Starting DockerBootApplicationTests using Java 16.0.2 on mao with PID 296 (started by mao in H:\程序\大三下期\Docker_boot)
2022-06-17 23:10:10.593 DEBUG 296 --- [           main] m.d.DockerBootApplicationTests           : Running with Spring Boot v2.7.0, Spring v5.3.20
2022-06-17 23:10:10.594  INFO 296 --- [           main] m.d.DockerBootApplicationTests           : The following 1 profile is active: "dev"
2022-06-17 23:10:11.923  INFO 296 --- [           main] m.d.DockerBootApplicationTests           : Started DockerBootApplicationTests in 1.696 seconds (JVM running for 2.504)
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 2.53 s - in mao.docker_boot.DockerBootApplicationTests
[INFO]
[INFO] Results:
[INFO]
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
[INFO]
[INFO]
[INFO] --- maven-jar-plugin:3.2.2:jar (default-jar) @ Docker_boot ---
[INFO] Building jar: H:\程序\大三下期\Docker_boot\target\Docker_boot-0.0.1-SNAPSHOT.jar
[INFO]
[INFO] --- spring-boot-maven-plugin:2.7.0:repackage (repackage) @ Docker_boot ---
[INFO] Replacing main artifact with repackaged archive
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  12.120 s
[INFO] Finished at: 2022-06-17T23:10:15+08:00
[INFO] ------------------------------------------------------------------------
PS H:\程序\大三下期\Docker_boot>
```



项目地址：

https://github.com/maomao124/Docker_boot.git/



## 将jar包放入指定目录

我放入的目录是C:\Users\mao\Desktop\test



```sh
PS C:\Users\mao\Desktop\test> ls


    目录: C:\Users\mao\Desktop\test


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         2022/6/17     23:10       17610406 Docker_boot-0.0.1-SNAPSHOT.jar


PS C:\Users\mao\Desktop\test>

```



## 新建Dockerfile文件

在刚才的目录下建立一个Dockerfile文件，要和jdk的存放目录一致



```sh
PS C:\Users\mao\Desktop\test> ls


    目录: C:\Users\mao\Desktop\test


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         2022/6/17     23:17              0 Dockerfile
-a----         2022/6/17     23:10       17610406 Docker_boot-0.0.1-SNAPSHOT.jar


PS C:\Users\mao\Desktop\test>
```





## 编写Dockerfile文件

使用记事本打开，输入以下命令



```sh
# 基础镜像使用java
FROM java17:1.0
# 作者
MAINTAINER mao
# VOLUME 指定临时文件目录为/tmp，在主机/var/lib/docker目录下创建了一个临时文件并链接到容器的/tmp
VOLUME /tmp
# 将jar包添加到容器中并更名为Docker_boot.jar
ADD Docker_boot-0.0.1-SNAPSHOT.jar Docker_boot.jar
# 运行jar包
RUN bash -c 'touch Docker_boot.jar'
ENTRYPOINT ["java","-jar","Docker_boot.jar"]
#暴露端口作为微服务
EXPOSE 8082
```





## 构建

运行命令：

```sh
docker build -t docker_boot:1.0 .
```



```sh
PS C:\Users\mao\Desktop\test> docker build -t docker_boot:1.0 .
[+] Building 1.0s (8/8) FINISHED
 => [internal] load build definition from Dockerfile                                                                                                       0.0s
 => => transferring dockerfile: 501B                                                                                                                       0.0s
 => [internal] load .dockerignore                                                                                                                          0.0s
 => => transferring context: 2B                                                                                                                            0.0s
 => [internal] load metadata for docker.io/library/java17:1.0                                                                                              0.0s
 => [internal] load build context                                                                                                                          0.2s
 => => transferring context: 17.61MB                                                                                                                       0.1s
 => [1/3] FROM docker.io/library/java17:1.0                                                                                                                0.2s
 => [2/3] ADD Docker_boot-0.0.1-SNAPSHOT.jar Docker_boot.jar                                                                                               0.1s
 => [3/3] RUN bash -c 'touch Docker_boot.jar'                                                                                                              0.5s
 => exporting to image                                                                                                                                     0.1s
 => => exporting layers                                                                                                                                    0.1s
 => => writing image sha256:d25f3f4955aec0b9ad19ab1e5290a1cc4a50eb283917df4ef8f520df6f37a08f                                                               0.0s
 => => naming to docker.io/library/docker_boot:1.0                                                                                                         0.0s

Use 'docker scan' to run Snyk tests against images to find vulnerabilities and learn how to fix them
PS C:\Users\mao\Desktop\test>
```





## 查看镜像



```sh
docker images
```



```sh
PS C:\Users\mao\Desktop\test> docker images
REPOSITORY    TAG       IMAGE ID       CREATED          SIZE
docker_boot   1.0       d25f3f4955ae   55 seconds ago   525MB
java17        1.0       282982c69086   14 hours ago     489MB
tomcat        latest    c795915cb678   2 weeks ago      680MB
redis         latest    53aa81e8adfa   2 weeks ago      117MB
mysql         latest    65b636d5542b   2 weeks ago      524MB
ubuntu        latest    d2e4e1f51132   7 weeks ago      77.8MB
PS C:\Users\mao\Desktop\test>
```



## 运行容器



```sh
docker run -d -p8082:8082 --name docker_boot1 docker_boot:1.0
```



```sh
PS C:\Users\mao\Desktop\test> docker run -d -p8082:8082 --name docker_boot1 docker_boot:1.0
251d1d54e1e699d3afdcd6b1ab08f2cd4ada9ddd9bfcd1bde0c4b72b2d563e31
PS C:\Users\mao\Desktop\test> docker ps -a
CONTAINER ID   IMAGE             COMMAND                  CREATED         STATUS                        PORTS                            NAMES
251d1d54e1e6   docker_boot:1.0   "java -jar Docker_bo…"   4 seconds ago   Up 3 seconds                  80/tcp, 0.0.0.0:8082->8082/tcp   docker_boot1
20c9959d6873   java17:1.0        "/bin/bash"              14 hours ago    Exited (255) 31 minutes ago   80/tcp                           java17
9cf1e361139c   java17:1.0        "--name java17 /bin/…"   14 hours ago    Created                       80/tcp                           boring_vaughan
8a8076944128   redis             "docker-entrypoint.s…"   3 days ago      Exited (255) 16 hours ago     0.0.0.0:6380->6379/tcp           redis1
81902b3cfdc4   mysql             "docker-entrypoint.s…"   3 days ago      Exited (0) 3 days ago                                          mysql3
24ee3e986397   mysql             "docker-entrypoint.s…"   3 days ago      Exited (0) 3 days ago                                          mysql2
2d379d342bb6   mysql             "docker-entrypoint.s…"   4 days ago      Exited (0) 3 days ago                                          mysql1
3ca156e4541d   tomcat            "catalina.sh run"        4 days ago      Exited (255) 24 hours ago     0.0.0.0:8080->8080/tcp           tomcat1
PS C:\Users\mao\Desktop\test>
```





## 访问测试

[localhost:8082/index](http://localhost:8082/index)

[localhost:8082/docker](http://localhost:8082/docker)



```sh
PS C:\Users\mao\Desktop\test> docker exec -it 251d1d54e1e6 /bin/bash
root@251d1d54e1e6:/usr/local# ls
Docker_boot.jar  bin  etc  games  include  java  lib  man  sbin  server.log  share  src
root@251d1d54e1e6:/usr/local# vi server.log
```



```sh
2023-06-18 04:53:55.630  INFO 1 --- [main] mao.docker_boot.DockerBootApplication    : Starting DockerBootApplication v0.0.1-SNAPSHOT using Java 17.0.3.1 on 251d1d54e1e6 with PID 1 (/usr/local/Docker_boot.jar started by root in /usr/local)
2022-06-18 04:53:55.632 DEBUG 1 --- [main] mao.docker_boot.DockerBootApplication    : Running with Spring Boot v2.7.0, Spring v5.3.20
2022-06-18 04:53:55.633  INFO 1 --- [main] mao.docker_boot.DockerBootApplication    : The following 1 profile is active: "dev"
2022-06-18 04:53:56.548  INFO 1 --- [main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8082 (http)
2022-06-18 04:53:56.557  INFO 1 --- [main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2022-06-18 04:53:56.557  INFO 1 --- [main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.63]
2022-06-18 04:53:56.619  INFO 1 --- [main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2022-06-18 04:53:56.619  INFO 1 --- [main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 931 ms
2022-06-18 04:53:56.968  INFO 1 --- [main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8082 (http) with context path ''
2022-06-18 04:53:56.981  INFO 1 --- [main] mao.docker_boot.DockerBootApplication    : Started DockerBootApplication in 1.888 seconds (JVM running for 2.266)
2022-06-18 04:55:23.282  INFO 1 --- [http-nio-8082-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring DispatcherServlet 'dispatcherServlet'
2022-06-18 04:55:23.282  INFO 1 --- [http-nio-8082-exec-1] o.s.web.servlet.DispatcherServlet        : Initializing Servlet 'dispatcherServlet'
2022-06-18 04:55:23.283  INFO 1 --- [http-nio-8082-exec-1] o.s.web.servlet.DispatcherServlet        : Completed initialization in 1 ms
```









# Docker网络

## 命令



所有命令：

```sh
PS C:\Users\mao\Desktop\test> docker network --help

Usage:  docker network COMMAND

Manage networks

Commands:
  connect     Connect a container to a network
  create      Create a network
  disconnect  Disconnect a container from a network
  inspect     Display detailed information on one or more networks
  ls          List networks
  prune       Remove all unused networks
  rm          Remove one or more networks

Run 'docker network COMMAND --help' for more information on a command.
PS C:\Users\mao\Desktop\test>
```



### 查看网络

```sh
docker network ls
```



```sh
PS C:\Users\mao\Desktop\test> docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
e26ce6e77409   bridge    bridge    local
58028068163e   host      host      local
c0e15a3d2248   none      null      local
PS C:\Users\mao\Desktop\test>
```



### 查看网络源数据

```sh
docker network inspect 网络名字
```



```sh
PS C:\Users\mao\Desktop\test> docker network inspect bridge
[
    {
        "Name": "bridge",
        "Id": "e26ce6e774096478217bfed7fabc0eecfd79286ac7d640037e3c3874440cd4a5",
        "Created": "2022-06-18T04:22:16.059751393Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]
PS C:\Users\mao\Desktop\test>
```



### 新建网络

```sh 
docker network create 网络名字
```



```sh
PS C:\Users\mao\Desktop\test> docker network create abc
619d44f4afea6b7aca982a0ecdc36bf69d097d66fdf5f41004c1d16dbeb8839c
```

```sh
PS C:\Users\mao\Desktop\test> docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
619d44f4afea   abc       bridge    local
e26ce6e77409   bridge    bridge    local
58028068163e   host      host      local
c0e15a3d2248   none      null      local
PS C:\Users\mao\Desktop\test>

```



### 删除网络

```sh
docker network rm 网络名字
```



```sh
PS C:\Users\mao\Desktop\test> docker network rm abc
abc
PS C:\Users\mao\Desktop\test> docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
e26ce6e77409   bridge    bridge    local
58028068163e   host      host      local
c0e15a3d2248   none      null      local
PS C:\Users\mao\Desktop\test>
```



### prune

删除所有未使用的网络

```sh
docker network prune
```





## 网络模式

### bridge

