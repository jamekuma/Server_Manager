# 用户管理
创建用户&赋予权限:
```bash
username=
sudo useradd -m -s /bin/bash -d /data/${username} ${username}
sudo passwd ${username}
```

更改文件/文件夹所属用户&用户组：
```bash
chown [-R] 用户名称 文件或目录
chgrp [-R] 用户组名称 dirname/filename ...
```
例：
```bash
username=
sudo chown -R ${username} /data/${username}
sudo chgrp -R ${username} /data/${username}
```

给予sudo权限:
```bash
sudo usermod -aG sudo ${username}
```

撤销sudo权限:
```bash
sudo gpasswd -d ${username} sudo
```

列出sudo成员：
```bash
getent group sudo
```

迁移用户主目录:
```bash
sudo usermod -d <new-folder> -m <username>
```
例如，把sigs账户迁移到/data1/sigs目录下（/data1下的sigs目录不存在，自动创建）：
```bash
sudo usermod -d /data1/sigs -m sigs
```



# 分发秘钥
首先生成秘钥：
```bash
ssh-keygen -t rsa
```
使用如下的bash脚本分发秘钥：
```bash
ip_list=("ip1" "ip2")
for ip in ${ip_list[*]}
do
    ssh-copy-id -i ./id_rsa.pub username@${ip}
done
```

# conda环境
导出环境到yaml配置文件：
```bash
conda env export > environment.yaml
```

根据配置文件导入环境操作：
```bash
conda env create -f environment.yaml
```

# 硬盘挂载
查看所有硬盘（包括未挂载的硬盘）：
```bash
sudo fdisk -l
```
挂载某个硬盘（以/dev/sdb1挂载到/data目录为例）：
```bash
sudo mount /dev/sdb1 /data
```
其中`/data`目录需要自己提前创建。

但是上述的挂载方法重启后失效。所以需要将分区信息写到/etc/fstab文件中让它永久挂载:··
```bash
sudo nano /etc/fstab
```
加入如下表项：
```bash
/dev/sdb1 /data ext4 defaults 0 2
```

如果是新硬盘，若上述挂载失败，则需格式化（**一定要注意不能有数据！否则会清空！**）：
```bash
sudo mkfs.ext4 /dev/sdb1
```

# tmux配置
在 .tmux.conf 中加入下列设置（tmux 2.1 之前的版本）：
```bash
set -g mode-mouse on
set -g mouse-resize-pane on
set -g mouse-select-pane on
set -g mouse-select-window on
```

在 tmux 2.1 中，对鼠标模式进行了重写，因此新版只需要增加一段设置即可：
```bash
set -g mouse on
```

# gcc/g++版本
以换成4.8为例
```bash
sudo rm /usr/bin/gcc
sudo rm /usr/bin/g++

sudo ln -s /usr/bin/gcc-4.8 /usr/bin/gcc
sudo ln -s /usr/bin/g++-4.8 /usr/bin/g++
```

