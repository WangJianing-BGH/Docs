# 在 Apple M1 芯片的 Mac 上安装使用 Homebrew

## 什么是 Homebrew ？

`Homebrew` 是一个包管理器，用于安装 Apple 没有预装但你需要的 UNIX 工具（比如著名的wget）。Homebrew 是 Mac 上管理软件包的最实用工具之一。遗憾的是，截至目前，它还没有对搭载 Apple Silicon(M1) 的新 Mac 机型完成适配。

根据维护者在 GitHub 上发布的说明，Homebrew 正在积极适配新架构的过程中，但目前还面临一些较大障碍，如缺少基于 ARM 架构的持续集成框架、很多软件包依赖的框架或编译器（go、gcc、qt）未适配等。

但 Homebrew 目前在新 Mac 上仍然是可用的，并且已经发布了原生支持 ARM 架构的实验性版本。本文总结我在设置过程中探索出可行、相对实用的做法。

## 1.安装 ARM 架构的 Homebrew

### 1.0 安装

根据官方规划，ARM 版 Homebrew 必须安装在 `/opt/homebrew` 路径下，而非此前的 `/usr/local/Homebrew` 。由于官方的安装脚本还未更新，可以通过如下命令手动安装：

```shell
cd /opt

sudo mkdir homebrew  #创建homebrew目录

sudo chown -R $(whoami) /opt/homebrew   #如非必要不建议使用该命令，执行完上条命令后应直接执行下一命令，如果没有权限再执行该命令。

curl -L https://github.com/Homebrew/brew/tarball/master | tar xz --strip 1 -C homebrew
```

安装完成

如果没有提示错误，则步骤结束后您已经成功安装了 ARM 版 Homebrew，但此时在终端中运行 brew 命令并不能直接启动该版本。这是因为默认情况下，ARM 版 Homebrew 用来安装程序的路径 `/opt/homebrew/bin` 并不在环境变量 PATH 中，因此终端无法检索到该路径下的 brew 程序。

### 1.1添加到环境变量

编辑配置文件 `~/.zshrc` ，加入如下内容：

```shell
path=('/opt/homebrew/bin' $path)
export PATH
```
(Tips: 本文推定使用 macOS Big Sur 的默认终端 zsh，如使用 bash 或 fish 终端，则修改 `~/.bashrc` 或 `~/.config/fish/config.fish`。)

### 1.2重启终端

重新启动终端。然后直接执行 brew 就可以启动 RAM 版的 Homebrew 了。

