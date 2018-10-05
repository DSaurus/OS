OS Lab1 WSL配置
====================

WSL(Windows Subsystem for Linux)简介
----------------------
[维基百科](https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux)


WSL配置
----------------------

- 在开始菜单输入 windows feature 打开启用或关闭 windows 功能
- 滑动滚动条到最后，将适用于 Linux 的 windows 子系统选项打勾
- 确定后会提示重启系统，重启即可
- 从[这里](https://aka.ms/wsl-ubuntu-1604)下载 ubuntu16.04 安装包，不要双击打开，你需要把它解压到一个固定的目录下，注意该目录就是 linux 根目录所在目录，固定后就不能移动或删除了
- 解压后，**以管理员权限**打开 ubuntu.exe ，按照提示即可完成 WSL 的安装（包括设置ubuntu的用户名和密码）
- 安装后想要使用 WSL，可以直接使用 win+R，输入 bash，或者在 cmd 里输入 bash 即可运行 WSL

WSL下的 qemu 配置过程
----------------------

下列过程都在WSL环境下执行(win+R，输入 bash 即可进入WSL环境)

- 首先执行 `sudo apt update`
- 之后依次执行下列操作
```shell
sudo apt install build-essential
sudo apt install binutils
sudo apt install libgtk2.0-dev
sudo apt install g++-multilib
sudo apt install libsdl-dev
```
- 接下来要配置 qemu 了，首先从操作系统官网上下载 lab1 的压缩包，里面包含 resource, src, doc。 然后需要进入 qemu 目录。
- windows 下和 linux 下目录的对应关系是：c:\xxx\yyy -> /mnt/c/xxx/yyy, /home/<user> -> wsl_install_directory/rootfs/home/<user>。 按照这个对应关系，可以使用`cd`命令找到并进入qemu目录。
- 在 qemu 目录下执行 `./configure  --disable-kvm  --disable-werror --prefix=/usr/local/qemu    --target-list="i386-softmmu x86_64-softmmu"`
- 如果一切正确，就可以执行`make`指令，之后执行`sudo make install`指令，配置结束。

WSL下的 lab1 配置过程
----------------------
- 首先打开lab1的目录（/src/lab1_1)，进入conf目录，用文本编辑器打开env.mk，修改最后一行为 QEMU=/usr/local/qemu/bin/qemu-system-i386 (整个一行不要有额外的空格，#为注释标记，要去掉)
- 在WSL下进入lab1的目录，执行`make qemu-nox`，如果以上步骤正确的话，会看到qemu的输出
- 如何退出qemu: `ctrl+A x` 

其他问题
---------------------
建议不要在windows目录下，对该目录使用git进行操作，最好在WSL环境下使用git