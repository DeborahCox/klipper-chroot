安装:  
1. 将本仓库克隆或下载到你需要安装klipper chroot容器的设备上.  
2. 获取 ubuntu-base rootfs:  
    在 http://mirrors.ustc.edu.cn/ubuntu-cdimage/ubuntu-base/releases/22.04/release 找到符合你设备架构的rootfs,下载到本目录,然后用以下命令解压到`rootfs`文件夹下:
    ```
    mkdir rootfs
    cd rootfs
    tar -xf ubuntu-base-*.tar.gz
    ```
    如有需要,进入`rootfs/etc/apt/sources.list`替换镜像源.
3. 先运行`mount.sh`挂载容器所需目录  
4. 进入容器,在其中创建并进入`klippy`用户,用于运行klipper:  
    ```
    sudo chroot rootfs /bin/bash
    apt update &&
    apt install sudo git -y
    useradd klippy
    usermod -a -G sudo klippy
    mkdir -p /home/klippy/
    chown klippy:klippy /home/klippy/
    su klippy
    cd ~
    ```
5. 在容器内的`klippy`用户,通过[kiauh](https://github.com/dw-0/kiauh)安装klipper, moonraker, klipperscreen和Mainsail/Fluidd
6. 安装完成后exit退出chroot
   运行umount脚本
   ```
   ./umount.sh
   ```
   制作根文件系统tar
   ```
   cd rootfs
   sudo tar cf - . | pv | pigz > ~/tspi-linux-sdk/ubuntu/ubuntu.tar.gz
   ```

使用:  
1. 运行`mount.sh`挂载容器所需目录  

其他脚本:  
- `start_klipperscreen.sh`: 启动klipperscreen  
- `chroot.sh`: 进入chroot shell,默认为`root`用户  
- `chroot_klippy.sh`: 进入chroot shell并切换到`klippy`用户  
- `umount.sh`: 解除挂载容器  

