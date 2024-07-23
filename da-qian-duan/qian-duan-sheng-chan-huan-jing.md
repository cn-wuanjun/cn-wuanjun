# 前端生产环境

### Centos8环境初始化

```bash
dnf module list nodejs
dnf module enable nodejs:16-epel
dnf install nodejs
npm i -g yarn
npm i -g pm2
dnf install git
git clone
# 配置npm和yarn私有仓库
yarn
npm run prd
```

#### 线上服务器build出现内存溢出

```bash
export NODE_OPTIONS="--max-old-space-size=8192"
```

### 使用NVM管理node版本

安装方法见github项目介绍：[https://github.com/nvm-sh/nvm?tab=readme-ov-file#installing-and-updating](https://github.com/nvm-sh/nvm?tab=readme-ov-file#installing-and-updating)

#### 安装步骤

执行以下命令

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc
```

#### 如果在国内遇到网络错误

1. 访问国内dns解析站点：[https://tool.chinaz.com/dns/raw.githubusercontent.com](https://tool.chinaz.com/dns/raw.githubusercontent.com)
2. 找到合适的解析ip，一般是地理位置近的
3. 然后修改服务器上的`/etc/hosts`文件，增加一行

```
185.199.108.133 raw.githubusercontent.com
```

然后重新执行上面的命令即可

#### 使用nvm

```bash
nvm install 18 --lts # 安装指定版本的长期稳定维护版本
nvm use 18 # 切换到指定的版本
nvm alias default 18 # 设置默认版本
```
