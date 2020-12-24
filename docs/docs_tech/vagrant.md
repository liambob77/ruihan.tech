# Vagrant Notes

## Vagrant 命令简介

```javascript
vagrant init {boxname} 初始化
vagrant status  查看虚拟机运行状态
vagrant up 启动虚拟机
vagrant halt  关闭虚拟化开发环境
vagrant reload 修改配置文件后，重启虚拟化开发环境
vagrant port 查看端口映射
vagrant box add {target} --name box {boxname}
vagrant box list 查看当前可用的虚拟化开发环境
vagrant box remove boxname 删除指定的box环境
vagrant global-status 查看环境
vagrant destroy 销毁虚拟机
vagrant package 当前正在运行的VirtualBox虚拟环境打包成一个可重复使用的box
```

## 创建虚拟机

```javascript
vagrant init phusion/ubuntu-14.04-amd64
vagrant up
vagrant ssh
vagrant halt
```

## 打包创建虚机

1、打包虚拟机

```javascript
vagrant package
```

2、当前目录就会生成package.box，之后新建虚拟机则可使用这个box。

```javascript 
vagrant box add my_box ~/package.box
vagrant init my_box
vagrant up
```
