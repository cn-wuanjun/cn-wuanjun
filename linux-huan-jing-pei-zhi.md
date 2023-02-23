# Linux环境配置

#### NodeJs安装

centos8：

> [https://nodejs.org/en/download/package-manager/#centos-fedora-and-red-hat-enterprise-linux](https://nodejs.org/en/download/package-manager/#centos-fedora-and-red-hat-enterprise-linux)

```
dnf module install nodejs:<stream>
# 查看所有版本
dnf module list nodejs
# 例子：安装16版本
dnf module install nodejs:16/common
```

centos7以下：

```
cd /usr/local/
mkdir nodejs
cd nodejs

# 到nodejs中文网，查看下载地址：
wget https://npm.taobao.org/mirrors/node/v14.15.3/node-v14.15.3-linux-x64.tar.xz
xz -d node-v14.15.3-linux-x64.tar.xz
tar -xvf node-v14.15.3-linux-x64.tar
cd node-v14.15.3-linux-x64

#建立软连接，变为全局
ln -s /usr/local/nodejs/node-v14.15.3-linux-x64/bin/npm /usr/local/bin/
ln -s /usr/local/nodejs/node-v14.15.3-linux-x64/bin/node /usr/local/bin/
vim /etc/profile

# 以下两个路径为加入nodejs路径  
export NODE_HOME=/usr/local/nodejs/node-v14.15.3-linux-x64  
export PATH=$NODE_HOME/bin:$PATH

# 配置生效
source /etc/profile
# 成功
node -v
# v14.15.3
npm -v
# 6.14.9
```

注意：以上的node版本号可以在网站中找到不同的版本号进行替换
