# 前端生产环境

#### Centos8环境初始化

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
