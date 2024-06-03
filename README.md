# 服务器操作手册

## 1. 用户账户管理

### 1.1 新用户注册
1. **联系管理员**：新用户需要联系服务器管理员（在实验室群加微信）申请账号。
2. **账户信息**：管理员将提供用户名和初始密码。
3. **首次登录**：新用户首次登录后需要更改密码。建议使用强密码，包括大小写字母、数字和特殊字符。

### 1.2 密码更改
1. **登录服务器**：
    ```sh
    ssh username@server_ip
    ```
2. **更改密码**：
    ```sh
    passwd
    ```
    按照提示输入当前密码和新密码。

### 1.3 用户权限
- 每个用户的权限被限制在其个人目录下。
- 在公共硬盘上，不要随意更改和访问他人的文件！！
- 使用``` rm ```命令的时候请千万注意不要删除他人文件。

## 2. 数据管理
### 2.1 选择存储空间较大的硬盘
1. **查看硬盘信息**：
    ```sh
    df -h
    ```
    该命令将显示所有挂载的硬盘和分区的使用情况。请注意`Size`（大小）和`Avail`（可用空间）列。
2. **选择硬盘**：根据显示的硬盘信息，选择存储空间较大的硬盘。
3. **存储数据**：将数据存储在选定硬盘的挂载点。例如，如果选择的硬盘挂载在`/mnt/large_disk`，则将数据存储在该目录下。创建一个以自己名字命名的文件夹，将使用到的所有文件存储在该文件夹中。比如，名字为“张三“的同学：
    ```sh
    mkdir -p /mnt/large_disk/zhangsan
    ```
    大家完成项目或者毕业之后请将数据整理到存储服务器或者删除，否则后续将会被清理。
   
### 2.2 数据存储
- 用户应将数据存储在个人目录下的`data`文件夹内。
    ```sh
    mkdir -p /mnt/large_disk/zhangsan/data
    ```
### 2.3 数据挂载
   如果需要的数据集已经在存储服务器上了，那么可以直接通过挂载从存储服务器读取数据不需要拷贝，目前的局域网带宽是1024MB/s，适合对带宽要求不大的任务，大家可以根据需要自行设计方案。

## 3. 深度学习环境配置
- 每个用户应在个人目录下创建和管理自己的 Anaconda 环境或者使用Docker。
- 建议在环境中安装所需的所有包，以避免系统包之间的冲突。
- 关于Anaconda和Docker的使用方法下面作简单介绍。
### 3.1 Anaconda安装
1. **下载Anaconda**：
  相关文件请下载到自己的目录中。
   ```sh
    cd /mnt/large_disk/zhangsan
    wget https://repo.anaconda.com/archive/Anaconda3-2023.03-Linux-x86_64.sh
    ```
2. **安装Anaconda**：
    ```sh
    bash Anaconda3-2023.03-Linux-x86_64.sh
    ```
    按照提示完成安装。

### 3.2 创建虚拟环境
1. **创建新环境**：
    ```sh
    conda create -n env_name python=3.x
    ```
2. **激活环境**：
    ```sh
    conda activate env_name
    ```
### 3.2 使用Docker配置环境
1. **安装Docker**：
    - 如果服务器上未安装Docker，请联系管理员。
2. **拉取Docker镜像**：
    - 用户可以根据需求选择合适的Docker镜像。例如，拉取一个包含pytorch 2的镜像。
4. **运行Docker容器**：
    - 运行一个带有GPU支持的Docker容器：
        ```sh
        docker run --gpus all -it --name tf_container tensorflow/tensorflow:latest-gpu bash
        ```
5. **管理Docker容器**：
    - 查看正在运行的容器：
        ```sh
        docker ps
        ```
    - 停止容器：
        ```sh
        docker stop tf_container
        ```
    - 启动容器：
        ```sh
        docker start tf_container
        ```
    - 进入已经运行的容器：
        ```sh
        docker exec -it tf_container bash
        ```

## 4. GPU使用管理

### 4.1 GPU查询
1. **查询GPU状态**：
    ```sh
    nvidia-smi
    ```

### 4.2 分配GPU
- 建议用户使用环境变量`CUDA_VISIBLE_DEVICES`来指定使用的GPU。
    ```sh
    export CUDA_VISIBLE_DEVICES=0,1  # 指定使用GPU 0和1
    ```


## 5. 常见问题处理

### 5.1 账户锁定
- 如果多次输入错误密码导致账户锁定，请联系管理员解锁。

### 5.2 GPU不足
- 如果所有GPU都在使用中，请联系同样在使用该服务器的同学自行协商使用时间，如有需要可联系管理员。
- ![Red Text](自行协商)

## 6. 联系管理员
- 任何问题或疑问，请联系服务器管理员。

希望以上操作手册能够帮助到大家更好地使用服务器。
