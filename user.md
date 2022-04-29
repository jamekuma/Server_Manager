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
