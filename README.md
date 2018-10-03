# docker_and_git
install docker

    开启hype-v

    error:Hardware assisted virtualization and data execution protection must be enabled in the BIOS 解决：开启 BIOS cpu 虚拟化

    这样增加镜像拉取的速度

%programdata%\docker\config\daemon.json（Windows） 来配置 Daemon。

请在该配置文件中加入（没有该文件的话，请先建一个）： `` { "registry-mirrors": ["http://hub-mirror.c.163.com"] } ``

备注：有些同学不明白这是什么意思--请看下面关于 composer 的镜像原理，当然，dockers的镜像原理，亦如是： 镜像原理： 一般情况下，安装包的数据（主要是 zip 文件）一般是从 github.com 上下载的，安装包的元数据是从 packagist.org 上下载的。 然而，由于众所周知的原因，国外的网站连接速度很慢，并且随时可能被“墙”甚至“不存在”。 “Packagist 中国全量镜像”所做的就是缓存所有安装包和元数据到国内的机房并通过国内的 CDN 进行加速， 这样就不必再去向国外的网站发起请求，从而达到加速 composer install 以及 composer update 的过程， 并且更加快速、稳定。因此，即使 packagist.org、github.com 发生故障（主要是连接速度太慢和被墙）， 你仍然可以下载、更新安装包。

4 error: docker pull ,UNAUTHORIZED incorrect username or passwd? 解决：网站和客户端，都是用 id 登陆，而不要使用 Email 登陆。（Windows上面才会出现这种问题吧，Linux上面，无需登陆，如果仅仅是这一步。）

5 Error starting userland proxy: mkdir /port/tcp:0.0.0.0:30009:tcp:172.17.0.3:80: input/output error. 解决：未正常关闭docker，开机后，重启docker。

` `` response:https://stackoverflow.com/questions/40668908/running-docker-for-windows-error-when-exposing-ports

The last Windows 10 update (Fall Creators Update, 2017) has a new "feature". It automatically starts any applications that were running when you last shutdown.

This reconstitutes Docker for Windows in a bad state. That made it appear those ports were in use by something else - it was the ghost of itself. This explained why those ports were still in use even though I stopped/started my containers and even reboot!

The solution in this case is to simply restart Docker daemon.

To prevent this after the next shutdown, don't use the shutdown button. Type this instead:

shutdown /s /t 0

    Stop all the running containers docker stop $(docker ps -a -q) then

    Stop the Docker on your machine & restart it.

Then run the required command. This will solve the issue. ` ``

6 ok,docker 可以开始正常拉取啦。。
工作流程

    。。。。。

    拉取镜像

    docker pull miqumi/images-ubuntu16.04-0509

    创建数据卷

    docker run -v /c/Users/admin/Desktop/svnfiles/trunk:/var/www/work --name vol-svnfiles003 miqumi/images-ubuntu16.04-0509 /bin/bash

    加载数据卷并从镜像中开启一个容器

    docker run -it --volumes-from vol-svnfiles003 --name umiqu0509 -p 30009:80 miqumi/images-ubuntu16.04-0509 /bin/bash

    关闭容器，启用容器，进入容器

    docker stop miqu0403001
    docker start miqu0403001
    docker exec -it miqu0403001 /bin/bash

备注

    这个镜像 ubuntu16.04 + php7.0 + protobuf + composer + git + yii
    http://localhost:30009/basic/web/index.php (yii basic 框架)
    http://localhost:30009/work/backend/web/index.php（本地的后台）
    在 /var/www/work 目录下可以进行控制台开发。。
    这里数据卷和容器中的文件是双向同步，小心删除。。

    打包镜像 runoob@runoob:~$ docker commit -m="ubuntu server" -a="miqumi" d835391486b0 miqumi/images-ubuntu16.04-0509 各个参数说明：

-m:提交的描述信息

-a:指定镜像作者

e218edb10161：容器ID

runoob/ubuntu:v2:指定要创建的目标镜像名

    可能发生的错误 mi@mi-TM1701:/mi$ sudo docker commit -m="miqu-server-70%" -a="miqumi" df0bab30be2d chenhang/miqu-server-70% invalid reference format

解决: mi@mi-TM1701:/mi$ sudo docker commit -m="miqu-server-001" -a="miqumi" df0bab30be2d chenhang/miqu-server-001 sha256:fb3519f269bf25408ac78c89d7e99efd968c446bf2932da46107f9ca63c2f9d0

'%' is special..
install centos6.5-lnmp

    docker search centos:6.5
    docker pull ..
    lnmp link vol

    创建数据卷 docker run -v /c/Users/admin/Desktop/miqu/miqu:/home/website --name vol-miqu imagine10255/centos6-lnmp-php56 /bin/bash
    启动数据卷 docker run -it --volumes-from vol-miqu --name miqu0402001 -p 30001:80 imagine10255/centos6-lnmp-php56 /bin/bash

    install phpize(可以通过 yum 来安装，首先要匹配 PHP 的版本 yum install php56w-devel)

    因为这个 docker 镜像，已经包含了 composer , 在 GitHub 上搜索 protobuf PHP, 选择了 星量 最高的版本，php5 的分支，然后安装。 git clone -b php5 https://github.com/allegro/php-protobuf.git 然后按照简介安装即可。

    到了这里，应该是安装成功了，开始提交容器，也就是打包镜像。 docker commit -m="miqu-lnmp-server-80%" -a="chenhang" 7e6ef059eaab chenhang/miqu-cnmp-server-5.6

    can't save 1.upcase letter or special letter ,such as % 2.because dockerfile ...

response: 需要按照作者的规定来修改文件。
git server

    problem one : [root@iZ94yvyhbwlZ miqu]# git push -u local_proj master Counting objects: 9967, done. Delta compression using up to 2 threads. Compressing objects: 100% (7768/7768), done. fatal: Out of memory, malloc failed58.36 MiB | 4.59 MiB/s error: pack-objects died with strange error

response: ` 方法2：使用文件的其他交换空间 如果您没有任何额外的磁盘，您可以在文件系统的某个位置创建一个文件，然后将该文件用于交换空间。

以下dd命令示例在/ root目录下创建名为“myswapfile”的交换文件，大小为1024MB（1GB）。

＃dd if = / dev / zero of = / root / myswapfile bs = 1M count = 1024 1024 + 0条记录 1024 + 0记录

＃ls -l / root / myswapfile -rw-r - r-- 1 root root 1073741824 Aug 14 23:47 / root / myswapfile 更改交换文件的权限，以便只有root可以访问它。

＃chmod 600 / root / myswapfile 使用mkswap命令将此文件作为交换文件。

＃mkswap / root / myswapfile 设置交换空间版本1，大小= 1073737 kB 启用新创建的交换文件。

＃swapon / root / myswapfile 要将该交换文件作为交换区域提供，即使在重新启动之后，请将以下行添加到/ etc / fstab文件中。

＃cat / etc / fstab / root / myswapfile swap swap defaults 0 0 验证新创建的交换区是否可供您使用。

＃swapon -s 文件名类型大小已用优先级 / dev / sda2分区4192956 0 -1 / root / myswapfile文件1048568 0 -2

＃free -k 缓存总共使用的空闲共享缓冲区 Mem：3082356 3022364 59992 0 52056 2646472

    / + buffers / cache：323836 2758520 交换：5241524 0 5241524 注意：在swapon -s命令的输出中，如果交换空间是从交换文件创建的，则Type列将会显示“file”。

如果您不想重新启动以验证系统是否采用了/ etc / fstab中提到的所有交换空间，则可以执行以下操作，这将禁用并启用/ etc / fstab中提到的所有交换分区

＃swapoff -a

＃swapon -a `
centos6.5-php5.6-phpize 的安装，

    参考资料 http://wangzq-phper.iteye.com/blog/2297792

备注：防止丢失，资料备注如下：

因为要在 CentOS 用 PHP 操作 Oracle 数据库，要安装新的 PHP 扩展 oci8 。

关于安装 PHP 扩展，以前总以为要重新编译 PHP，今天查阅大量资料发现原来可以像apache模块一样动态扩展。今天就以 oci8 举例。

一、进入要安装的扩展的源码目录（没有就到官方下载源代码）
cd /root/php-5.5.35/ext/oci8

运行 phpize ，如果不知道 phpize 在哪个目录，可以运行 # which phpize 命令
/usr/bin/phpize # 这一步可能会出现以下错误

按照字面的意思，可能是你没安装 php-devel 这个扩展包。phpize是用来扩展php扩展模块的，通过phpize可以建立php的外挂模块，phpize 是属于php-devel的内容，所以只要运行yum install php-devel 就行
yum install php-devel # 却出现以下提示

这说明仓库里默认的 phpize 和 PHP 版本不一致。从下面的命令可以看出：php-devel 版本是 5.3.3 。这就需要我们重新安装 phpize 。

安装与当前 PHP 版本一致的 php-devel 。PHP 版本可以通过 php -v 查看。

所以安装与当前系统 PHP 对应的 php-devel 就可以了。运行 yum install php55w-devel 出现以下信息，安装成功。

二、运行 /usr/bin/phpize 出现以下信息，说明可以了。
./configure --with-php-config=/usr/bin/php-config
make
make install

安装完后会有这样的东西，Installing shared extensions: /usr/lib64/php/modules 。说明系统已经默认把 oci8.so 模块安装在 /usr/lib64/php/modules 目录中了。

三，修改php.ini
vi /etc/php.ini

由于我的 CentOS 里 PHP 是以这种方式扩展模块。所以只需在 /etc/php.d 里面添加相应的文件就可以了。

关于PHP扩展，可以通过 phpinfo(); 来查看。
centos6.5-php5.6-protobuf 的安装

    参考资料：https://github.com/allegro/php-protobuf

Requirements PHP 7.0 or above (for PHP 5 support refer to php5 branch) Pear's Console_CommandLine (for the protoc plugin) Google's protoc compiler version 2.6 or above

    问题：我安装完成 protobuf.so 并配置好 php.ini 文件，php -m ，phpinfo()等查看，确认扩展已经安装。可是 提示确认缺少 protoc

答案：以上的 requirements 已经很清楚的说明了需要什么，对于 lujun 而言。 参考资料：

1、protobuf是google公司提出的数据存储格式，详细介绍可以参考：https://code.google.com/p/protobuf/

2、下载最新的protobuf，下载地址：https://code.google.com/p/protobuf/downloads/list

3、下载protobuf2.5.o版本，protobuf-2.5.0.tar.gz解压并进行安装。

解压：tar xvf protobuf-2.5.0.tar.gz

安装步骤：（1）./configure （2）make （3）make check （4）make install

注意：安装成功后，将它的bin和lib目录分别加入到PATH和LD_LIBRARY_PATH环境变量，以方便直接调用。

通常建议安装到/usr/local目录下，执行configure时，指定--prefix=/usr/local/protobuf即可

设置环境变量过程：编辑/etc/profile，在文件末尾添加：

export PATH=$PATH:/usr/local/protobuf/bin export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/protobuf/lib

备注：大致如上，需要改动的地方：

    需要 protobuf2.6.0 及以上的版本
    加入到环境变量中的办法，稍有不同。（最快的办法莫过于将 命令文件复制一份到 /usr/bin 目录下哦）
    git clone https://github.com/google/protobuf.git(https://github.com/google/protobuf/tree/master) ,这次我在自己的docker 环境 ubuntu16.04 + php7.0 中，直接拉取最新的。安装如下： ` ./autogen.sh

./configure --prefix=/usr/local/protobuf

make -j8 && make install `

    问题： 这里对 miqu.proto 的编译，会以类名的方式产生文件，而实际工作中的编译是生成一个文件,如何改动呢？

docker 容器--centos6.5 + php5.6 + protobuf,为什么修改不了数据卷的文件权限？

    方法一：--privileged=true C:\Users\admin>docker run -it --privileged=true --volumes-from vol-miqu-svnfiles: --name miqu0426 -p 30426:80 miqumi/images-miqu-server-04182148 /bin/bash

Starting nginx: [ OK ] Starting php-fpm: [ OK ] Starting sshd: [ OK ]

结果：未能成功
git 基本使用命令

// 将代码克隆到本地

    git clone url

// 初始化本地配置文件，因为目前配置的是 子仓库 git，所以会加入 submodule

    git submoudle init

// 检出父仓库列出的commit

    $ git submodule update

// 或者使用组合指令。

    $ git submodule update --init --recursive

// 创建分支，分支名字是 branch_name

    git branch branch_name

// 切换到 branch_name 分支，需要在 branch_name 分支开发。。

    git checkout branch_name

// 开始开发

    git status (查看仓库的当前状态，一般会经常查看)
    git add -A (全部文件提交到本地仓库)
    git add filename (将某个文件提交到版本库)
    git commit -m 'message' (将文件提交到仓库)
    git status (查看仓库的当前状态，一般会经常查看) 备注： 先记住，提交本地分两步，就可以了。。

// 将 本地的 branch_name 分支 推送到 origin 即 远程仓库的 branch_name 分支 下

    git push origin branch_name:branch_name

// 将 origin 即 远程仓库的 master 分支 拉取到 本地的 master 分支下, 这个 master 分支一般 用来更新代码，而不在此分支开发

    git pull origin master:master

// 合并 master 分支,一般在 branch_name 分支下，操作这个命令，合并的操作，编辑器修改即可

    git merge master ()

继续在 branch_name 分支开发。。
