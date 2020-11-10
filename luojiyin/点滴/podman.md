最近再使用Fedora 33 系统带的是Podman,不再是Docker

Podman 原来是 CRI-O 项目的一部分，后来被分离成一个单独的项目叫 libpod。Podman 的使用体验和 Docker 类似，不同的是 Podman 没有 daemon。

Docker 的组成包含 Docker CLI 和 Docker Daemon
Docker CLI将命令发送到 Docker Daemon，
 1 服务器端守护程序单点故障
 2 该进程拥有所有子进程(正在运行的容器)
 3 如果Docker守护进程出现故障，则每个子进程将失去跟踪
 4 构建容器导致安全漏洞
 5 所有Docker操作必须由完全根权限的一个或者多个用户执行


Podman 直接与镜像仓库 容器和镜像存储API进行交互， 通过runC容器运行时进程(不是守护进程) 使用Linux内核

Podman 使用 CNI (Go写的) 作为网络底层，实现比 Docker 网络层略微简单但原理相同。 相对于 lxd 与 nspawn CNI 可以避免编写大量的网络规则。

Podman 可以使用 slirp4netns，避免打 ip a 是看到一大坨 veth* 和 docker0, 并且性能更好

podman 没有 daemon 进程，由 podman cli 直接 clone 并创建命名空间，这带来了其他好处
 1 root less 可以不使用root运行
 2 容器的父进程都是在模拟终端器的 podman命令
 3 每个用户都有自己的podman命名空间
 4 可以用 systemd service 管理容器


podman支持在非root用户下运行，这需要配置/etc/subuid 和 /etc/subgid，
 使得podman可以将host的用户id映射到container里面去。


 由于国内神奇的网络，需要配置国内podman 镜像源，一番探索
 在  https://github.com/containers/podman/issues/3735   找到办法

 /etc/containers/registries.conf 增加配置


unqualified-search-registries = ["docker.io"]
[[registry]]
prefix = "docker.io"
location = "uyah70su.mirror.aliyuncs.com"

使用阿里云的源