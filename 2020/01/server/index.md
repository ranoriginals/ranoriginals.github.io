# 研究组服务器使用说明


## P100服务器 

#### 主要配置 

* CPU: Intel Xeon(R) E5-2637v4@3.50GHz \* 16 

* GPU: Quadro M2000 & Telsa P100 16GB \* 4 

* 内存: 128GB 

* 主要储存空间: 8T(`/work`)+8T(`/project`) 

#### IP地址：114.212.167.243，同时有域名：bixie.nju.edu.cn，可以指向该IP地址。使用SSH连接，可以直接连接外网，但是不能从外网访问 

#### 用户名一般是姓_名，比如张三的用户ID就是zhang_san，登陆密码请联系管理员进行设置，登陆方式为：

```bash
ssh -Y zhang_san@bixie.nju.edu.cn 

ssh -Y zhang_san@114.212.167.243
```

#### 用户登入后会进入`/home`文件夹，这一文件夹只用于存放一些用户设置项。如果需要进行计算以及存储数据，需切换到`/work`文件夹，用户可以自行建立一个文件夹以存放自己的数据。考虑到不同人的工作之间可能有交集，所以用户可以访问此文件夹中其他用户的目录，但是如果没有经过他人的允许，请不要随意访问他人的目录。 

#### 由于每个人在工作中所使用的程序有所不同，所以科研过程中用到的软件可以自行安装，安装包也可以放在自己的工作目录中。如果确认遇到安装权限问题，请联系管理员 

#### P100服务器上已经安装有常用软件，如果需要使用把对应的环境变量添加进自己的环境变量配置即可。 

CUDA: 

``` bash
# cuda 
export CUDA_HOME=/usr/local/cuda-10.0 
export PATH=${CUDA_HOME}/bin:$PATH 
export LD_LIBRARY_PATH=${CUDA_HOME}/lib64:$LD_LIBRARY_PATH 
export LIBRARY_PATH=${CUDA_HOME}/lib64:$LIBRARY_PATH 
```

PGI编译器: 

```bash
#pgi 
export PGI=/opt/pgi 
export PATH=${PGI}/linux86-64/18.10/bin:$PATH 
export MANPATH=$MANPATH:${PGI}/linux86-64/18.10/man 
export LM_LICENSE_FILE=$LM_LICENSE_FILE:${PGI}/license.dat 
```

SAC: 

``` bash
#sac 
export SACHOME=/usr/local/sac 
export SACAUX=${SACHOME}/aux 
export PATH=${SACHOME}/bin:${PATH} 
export SAC_DISPLAY_COPYRIGHT=1 
export SAC_PPK_LARGE_CROSSHAIRS=1 
export SAC_USE_DATABASE=0 
```

GMT: 

``` bash
#gmt 
export GMT5HOME=/opt/GMT-5.4.5 
export PATH=${GMT5HOME}/bin:$PATH 
export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:${GMT5HOME}/lib64 
```

Intel编译器: 

``` bash
#intel 
source /opt/intel/vtune_amplifier_2019/amplxe-vars.sh quiet 
source /opt/intel/inspector_2019/inspxe-vars.sh quiet 
source /opt/intel/advisor_2019/advixe-vars.sh quiet 
source /opt/intel/bin/compilervars.sh intel64 
```

OpenMPI: 

``` bash
# openmpi 
export PATH=/usr/local/mpi-3.1.4/bin:$PATH 
```

Taup: 

``` bash
# Taup 
export TAUPHOME=/opt/TauP-2.4.5 
export PATH=${TAUPHOME}/bin:${PATH} 
```
 

