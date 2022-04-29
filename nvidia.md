# 下载资源
nvidia各版本驱动：<https://www.nvidia.cn/Download/Find.aspx?lang=cn>
cuda各版本：<https://developer.nvidia.com/cuda-toolkit-archive>
cudnn各版本：<https://developer.nvidia.com/rdp/cudnn-archive>

# NVIDA驱动安装（新机器首次安装）
1. 删除原有驱动
```bash
sudo apt-get purge nvidia*
sudo apt-get autoremove
sudo ./NIVIDIA-Linux-X86_64-384.59.run --uninstall
```
2. 安装依赖
```bash
sudo apt-get install build-essential gcc-multilib dkms
```

3. 禁用nouveau驱动：
编辑 /etc/modprobe.d/blacklist-nouveau.conf 文件，添加以下内容：
```bash
blacklist nouveau
blacklist lbm-nouveau
options nouveau modeset=0
alias nouveau off
alias lbm-nouveau off
```

关闭nouveau：
```bash
echo options nouveau modeset=0 | sudo tee -a /etc/modprobe.d/nouveau-kms.conf
```

4. reboot
```bash
sudo update-initramfs -u
sudo reboot
```
重启后，执行：`lsmod | grep nouveau`。如果没有屏幕输出，说明禁用nouveau成功。

5. 获取kernel source （important）
```bash
sudo apt-get install linux-source
sudo apt-get install linux-headers-$(uname -r)
```

6. 关掉x graphic 服务
```bash
sudo systemctl stop lightdm(or sudo service lightdm stop)
sudo systemctl stop gdm
sudo systemctl stop kdm
```
登陆nvidia官网，可以得到适合自己电脑的驱动，下载下来

7. 安装nvidia驱动
```bash
sudo chmod NVIDIA*.run
sudo ./NVIDIA-Linux-x86_64-384.59.run -no-x-check -no-nouveau-check -no-opengl-files
```

–no-opengl-files：表示只安装驱动文件，不安装OpenGL文件。这个参数不可省略，否则会导致登陆界面死循环，英语一般称为”login loop”或者”stuck in login”。
–no-x-check：表示安装驱动时不检查X服务，非必需。
–no-nouveau-check：表示安装驱动时不检查nouveau，非必需。
-Z, --disable-nouveau：禁用nouveau。此参数非必需，因为之前已经手动禁用了nouveau。
-A：查看更多高级选项。


安装过程中一些选项

`The distribution-provided pre-install script failed! Are you sure you want to continue? `
选择`yes`继续。

`Would you like to register the kernel module souces with DKMS? This will allow DKMS to automatically build a new module, if you install a different kernel later?`
选择`No`继续。

问题大概是：`Nvidia's 32-bit compatibility libraries?`
选择`No`继续。

`Would you like to run the nvidia-xconfigutility to automatically update your x configuration so that the NVIDIA x driver will be used when you restart x? Any pre-existing x confile will be backed up.`
选择`Yes`继续

8. 挂载Nvidia驱动
```
sudo modprobe nvidia
```

9. 检查驱动是否安装成功
```bash
nvidia-smi
nvidia-settings #若弹出设置对话框，亦表示驱动安装成功
```
