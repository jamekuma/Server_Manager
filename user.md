创建用户&赋予权限:
```bash
sudo useradd -m -s /bin/bash -d /data/<username> <username>
sudo passwd <username>
```

给予sudo权限:
```bash
sudo usermod -aG sudo <username>
```

迁移用户主目录:
```bash
sudo usermod -d <new-folder> -m <username>
```
例如，把sigs账户迁移到/data1/sigs目录下（/data1下的sigs目录不存在，自动创建）：
```bash
sudo usermod -d /data1/sigs -m sigs
```

更改文件/文件夹所属用户&用户组：
```bash
chown [-R] 用户名称 文件或目录
chgrp [-R] 用户组名称 dirname/filename ...
```
例：
```bash
sudo chown -R xiongjianyu .
sudo chgrp -R xiongjianyu .
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
