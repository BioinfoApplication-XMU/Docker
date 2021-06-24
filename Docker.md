

# Docker



## Docker basic commands

### 命令查找网站：

> #### https://www.runoob.com/docker/docker-command-manual.html

```shell


docker info

docker search anaconda # docker搜索可用镜像

docker pull continuumio/anaconda3 # docker拉取镜像

docker images #查看所有镜像
docker ps #查看当前正在运行的容器
docker ps -a #查看所有容器


docker cp :用于容器与主机之间的数据拷贝
docker container ls -all # 查看已有容器

docker rm -f bioinfo_tsinghua # 刪除容器
docker rmi bioinfo_tsinghua # 删除镜像

docker rename  原名字 新名字 # 容器重命名


docker start :启动一个或多个已经被停止的容器

docker stop :停止一个运行中的容器

docker restart :重启容器

docker commit :从容器创建一个新的镜像。
```



## 基于已有linux镜像构建新镜像

### 教程网站：

> #### https://yeasy.gitbook.io/docker_practice/
>
> #### https://lulab2.gitbook.io/teaching/
>
> #### https://www.huaweicloud.com/articles/9294cfdde23bf68bbc46f79bf867cdd9.html



**我已经创建了基础镜像(xmu_bi)和容器(xmu_bi),里面安装了R和python,conda也配置好了，后续要装什么包的话，可以通过镜像创建多个容器，在每个容器里特定封装每个pipeline需要的东西**



```bash


# Step1
docker pull conda/miniconda3-centos7

# Step2
docker run --name=xmu_bi  -dt -h xmu_bi --restart unless-stopped -v /cluster/huanglab/hhuang/docker_test/:/usr/Downloads conda/miniconda3-centos7

# Step3
docker exec -it xmu_bi bash


# Step4
# 使用conda创建环境 （R）
eg. conda install r-base python fastqc


################################################
# 打开R报错
'''
Error: package or namespace load failed for 'utils':
 .onLoad failed in loadNamespace() for 'utils', details:
  call: system(paste(which, shQuote(names[i])), intern = TRUE, ignore.stderr = TRUE)
  error: error in running command
Error: package or namespace load failed for 'stats':
 .onLoad failed in loadNamespace() for 'utils', details:
  call: system(paste(which, shQuote(names[i])), intern = TRUE, ignore.stderr = TRUE)
  error: error in running command
During startup - Warning messages:
1: package 'utils' in options("defaultPackages") was not found
2: package 'stats' in options("defaultPackages") was not found

'''
# 解决办法
yum install -y which
##################################################

# Step5
# 装完所需要的包后，通过容器创建新镜像
docker commit xmu_bi xmu
# docker commit container_name image_name

# Step6
# 保存镜像成压缩包
docker save image_name -o XMU_BI.tar.gz
# docker save image_name -o 压缩包名称


# whereis anaconda
# /usr/local/envs/
#  /opt/conda/envs
# 找到你要的环境的目录 eg.your_anaconda3_path/envs
# docker cp /cluster/huanglab/hhuang/app/envs/xmu_bi/ conda:/usr/local/envs/
```







### Windows 下docker 运行

```bash
# Step1
先安装docker桌面版

# Step2
打开windows powershell（管理员权限）

# Step3
docker load -i C:\Users\Dell\Desktop\xmu_bi.tar.gz


# Step4
docker run --name=XMU_bioinfo  -dt -h XMU_bioinfo --restart unless-stopped xmu_bi
# -v 宿主机目录：容器内目录
# 挂载本机的目录
# 可挂载分析数据

# Step5
docker exec -it bioinfo_roc_survival bash

# Step6
# 不用了退出
exit # 退出鏡像

# Step7
# 查看正在运行的容器
docker ps

# Step8
# 删除容器（如果想删除的话）
docker rm -f 容器名字

```



























