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

