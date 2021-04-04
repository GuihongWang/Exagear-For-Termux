### 选择语言：[简体中文](https://gitee.com/tsxor/Exagear-For-Termux/blob/master/README.md)
### Choose a language:[English](https://gitee.com/tsxor/Exagear-For-Termux/blob/master/README-EN.md)
### Выберите язык:[Русскии](https://gitee.com/tsxor/Exagear-For-Termux/blob/master/README-RU.md)

# Exagear For Termux
**Exagear For Termux** - 修改过的Exagear，它可在Android上的Termux中运行。本项目以稳定和高速为主要目标，将替代不稳定且低速的“QEMU用户空间模拟+proot”方案。

## Exagear是干啥吃的？
Exagear是允许基于ARM的处理器（常见于树莓派(各种Pi)和安卓手机）运行Intel x86程序的“新”虚拟化技术。该项目（指Exagear）由创立于2012年的俄罗斯公司Exagear开发，于2019年终止开发，但[在2020年被~~伟大的~~华为延续](https://www.huaweicloud.com/kunpeng/software/exagear.html)，现在已经支持将x86_64指令翻译成ARM64指令了。

## 特色
* 支持System V IPC和POSIX IPC
* 翻译指令稳定高速
* 快速且便捷地部署x86系统

## 安装流程
### 在termux中操作:
0) 换源（加快下载速度，已经换过就跳过此步）（本步命令来自[国光的termux教程](https://www.sqlsec.com/2018/05/termux.html#toc-heading-17)）
```
sed -i 's@^\(deb.*stable main\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/termux-packages-24 stable main@' $PREFIX/etc/apt/sources.list
sed -i 's@^\(deb.*games stable\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/game-packages-24 games stable@' $PREFIX/etc/apt/sources.list.d/game.list
sed -i 's@^\(deb.*science stable\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/science-packages-24 science stable@' $PREFIX/etc/apt/sources.list.d/science.list
pkg update
```
1) 安装tar和git:
```
pkg update -y && pkg install tar git -y
```
2) 将本仓库复制到家目录:
```
git clone https://gitee.com/tsxor/Exagear-For-Termux ~/ExaTermux
```
3) 初始化proot-static模块:
```
cd ~/ExaTermux
git submodule init
git submodule update
```
4) 下载rootfs并解包至家目录下的ExaTermux/exagear-fs文件夹，此处以Debian 10为例。(此处下载链接没失效，但没有直接的镜像源，正在搬运中)（有镜像code的，但此处下载的是release，而所有镜像都没release）
```
wget https://github.com/termux/proot-distro/releases/download/v1.1-debian-rootfs/debian-buster-i386-2020.12.05.tar.gz
mkdir exagear-fs/ && tar -C exagear-fs/ --warning=no-unknown-keyword --delay-directory-restore --preserve-permissions --strip=0 -xvf debian-buster-i386-2020.12.05.tar.gz --exclude='dev'||: && cd exagear-fs/ && mv debian-buster-i386-2020.12.05/* ./ && rm -rfv debian-buster-i386-2020.12.05/ && cd ../
```
5) 大功告成！运行Exagear-For-Termux吧！
```
./start-exagear.sh
```
