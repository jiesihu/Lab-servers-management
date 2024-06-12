# 服务器操作手册 & 服务器管理员操作手册
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

## 2. 文件管理
如果已经被分配了**用户目录**可以跳过2.1。
### 2.1 选择存储空间较大的硬盘
1. **查看硬盘信息**：
    ```sh
    df -h
    ```
    该命令将显示所有挂载的硬盘和分区的使用情况。请注意`Size`（大小）和`Avail`（可用空间）列。
2. **选择硬盘**：根据显示的硬盘信息，选择存储空间较大的硬盘。
3. **存储数据**：将数据存储在选定硬盘的挂载点。例如，如果选择的硬盘挂载在`/mnt/large_disk`，则将数据存储在该目录下。创建一个以自己名字命名的文件夹，将使用到的所有文件存储在该文件夹中。
    ```sh
    mkdir -p /mnt/large_disk/username
    ```
    大家完成项目或者毕业之后请将数据整理到存储服务器或者删除，否则后续将会被清理。
   
### 2.2 数据存储
- 用户应将所有代码、数据存储在个人目录下。建议将conda也安装在个人目录下，避免主盘空间被环境占满。
    ```sh
    mkdir -p /mnt/large_disk/username
    ```
### 2.3 数据挂载
   如果需要的数据集已经在**存储服务器**上了，那么可以直接通过挂载从存储服务器读取数据不需要拷贝，目前的局域网带宽是1024MB/s，适合对带宽要求不大的任务，大家可以根据需要自行设计方案。

## 3. 深度学习环境配置
- 每个用户应在个人目录下创建和管理自己的 Anaconda 环境或者使用Docker。
- 建议在环境中安装所需的所有包，以避免系统包之间的冲突。
- 关于Anaconda和Docker的使用方法下面作简单介绍。
### 3.1 Anaconda安装
1. **下载Anaconda**：
个别服务器的用户目录的上级目录有miniconda的安装包，可直接运行安装。
如果没有，相关文件请下载到自己的目录中。
   ```sh
    cd /mnt/large_disk/username
    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
    ```
3. **安装Anaconda**：
    ```sh
    chmod +x Miniconda3-latest-Linux-x86_64.sh
    ./Miniconda3-latest-Linux-x86_64.sh
    ```
    按照提示完成安装。
   部分机器安装完后会出现```conda: command not found```可自行解决，不需要找管理员。

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
    - 如果服务器上未安装Docker或者没有提供Docker权限，请联系管理员。
2. **拉取Docker镜像**：
    - 直接查看当前docker镜像
      ```sh
      docker images
      ```
    - 如果没有合适的，用户可以根据需求选择合适的Docker镜像。例如，拉取一个包含pytorch 2的镜像。
    
4. **运行Docker容器**：
    - 运行一个带有GPU支持的Docker容器：
        ```sh
        docker run --gpus all -it ghcr.io/pytorch/pytorch:2.3.1-cuda11.8-cudnn8-runtime bash
        ```
    - 运行带有GPU支持的容器并且挂载上自己的文件夹
    ```sh
    docker run --gpus all -it -v /mnt/sdb/username:/mnt/sdb/username ghcr.io/pytorch/pytorch:2.3.1-cuda11.8-cudnn8-runtime bash
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
- 如果所有GPU都在使用中，请联系同样在使用该服务器的同学**自行协商**使用时间，如有需要可联系管理员。

## 6. 联系管理员
- 任何问题或疑问，请联系服务器管理员。

希望以上操作手册能够帮助到大家更好地使用服务器。


# 服务器管理员操作手册
**为保证系统安全，避免在管理员账户运行代码！！！**
## 1. 创建用户并设定权限

### 1.1 创建新用户
1. **创建用户**：
    ```sh
    sudo adduser $username
    ```
    按照提示设置用户密码和其他信息。

2. **添加用户到特定组**（例如，`docker`组）：
    ```sh
    sudo usermod -aG docker username
    ```
    一般都需要添加到docker组中。

### 1.2 设置用户权限
- 确保新用户只能访问其个人目录：
    ```sh
    sudo chmod 700 /home/username
    ```
- 为用户创建其文件夹（可选）
  ```sh
  # 假设硬盘挂载位置为 /mnt/sdb
  sudo mkdir /mnt/sdb/username
  ```
  
- 如果需要为用户提供特定目录的写权限，可以设置ACL（访问控制列表）：
    ```sh
    # 假设硬盘挂载位置为 /mnt/sdb
    sudo chown -R username:username /mnt/sdb/username
    sudo chmod -R 700 /mnt/sdb/username
    ```

## 2. Docker安装与配置

### 2.1 安装Docker
1. **更新apt包索引**：
    ```sh
    sudo apt-get update
    ```

2. **安装必要的软件包**：
    ```sh
    sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common
    ```

3. **添加Docker官方GPG密钥**：
    ```sh
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```

4. **添加Docker APT源**：
    ```sh
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    ```

5. **更新APT包索引**：
    ```sh
    sudo apt-get update
    ```

6. **安装Docker CE**：
    ```sh
    sudo apt-get install -y docker-ce
    ```

7. **启动Docker服务**：
    ```sh
    sudo systemctl start docker
    sudo systemctl enable docker
    ```

### 2.2 配置Docker用户组
- 将所有需要使用Docker的用户添加到`docker`组：
    ```sh
    sudo usermod -aG docker username
    ```

- 提醒用户注销并重新登录以使更改生效。

## 3. GPU配置

### 3.1 安装NVIDIA驱动
1. **添加NVIDIA包存储库**：
    ```sh
    sudo add-apt-repository ppa:graphics-drivers/ppa
    sudo apt-get update
    ```

2. **安装NVIDIA驱动程序**：
    ```sh
    sudo apt-get install -y nvidia-driver-460
    ```

3. **重启系统**：
    ```sh
    sudo reboot
    ```

### 3.2 安装NVIDIA Docker支持
    参考官网教程：  
    https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html

## 4. 系统维护

### 4.1 定期更新和升级系统
1. **更新软件包列表**：
    ```sh
    sudo apt-get update
    ```

2. **升级所有包**：
    ```sh
    sudo apt-get upgrade -y
    sudo apt-get dist-upgrade -y
    ```

### 4.2 安全性检查和防火墙配置（可选）
1. **安装UFW（Uncomplicated Firewall）**：
    ```sh
    sudo apt-get install -y ufw
    ```

2. **配置防火墙规则**：
    ```sh
    sudo ufw allow ssh
    sudo ufw allow 2375/tcp  # Docker端口（可选）
    sudo ufw enable
    ```

### 4.3 定期备份 （可选）
- 建议使用`rsync`或其他备份工具定期备份重要数据：
    ```sh
    rsync -avh /path/to/data /path/to/backup/location
    ```
### 4.4 确保必要的软件包已安装
```
tmux

```

## 5. 硬盘管理

### 5.1 查看硬盘使用情况
- 使用`df`命令查看硬盘使用情况：
    ```sh
    df -h
    ```

### 5.2 挂载新硬盘
1. **查看所有磁盘**：
    ```sh
    lsblk
    ```

2. **创建挂载点**：
    ```sh
    sudo mkdir -p /mnt/new_disk
    ```

3. **挂载磁盘**：
    ```sh
    sudo mount /dev/sdX1 /mnt/new_disk
    ```

4. **更新`/etc/fstab`以自动挂载**：
    ```sh
    sudo nano /etc/fstab
    ```
    添加以下行：
    ```sh
    /dev/sdX1 /mnt/new_disk ext4 defaults 0 0
    ```

### 5.3 管理硬盘权限
- 设置挂载点的权限：
    ```sh
    sudo chown -R username:username /mnt/new_disk
    sudo chmod 755 /mnt/new_disk
    ```

## 6. 系统监控

### 6.1 安装监控工具
- 安装`htop`进行系统资源监控：
    ```sh
    sudo apt-get install -y htop
    ```

### 6.2 安装NVIDIA监控工具
- 安装`nvidia-smi`进行GPU监控：
    ```sh
    sudo apt-get install -y nvidia-utils-460
    ```

### 6.3 设置监控报警
- 安装并配置监控工具（例如Prometheus + Grafana）来监控系统性能，并设置报警规则。

## 7. 日志管理

### 7.1 查看系统日志
- 查看`syslog`日志：
    ```sh
    sudo tail -f /var/log/syslog
    ```

### 7.2 查看Docker日志
- 查看Docker容器日志：
    ```sh
    docker logs container_name
    ```

希望以上操作手册能帮助管理员更好地管理和维护服务器。

