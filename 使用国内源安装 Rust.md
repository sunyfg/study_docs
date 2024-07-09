# 使用国内源安装 Rust

## 下载安装脚本

参考 Rust 官网的新手文档

https://www.rust-lang.org/learn/get-started

在终端命令行中执行

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

但是这个安装过程非常慢，且没有进度条，估计是下载速度不行。

参照网上的做法，可以先下载官方的安装脚本，然后将其中的下载源替换为国内的源即可。

所以，先保存 Rust 官方的安装脚本:

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs > rust.sh
```

## 替换国内源

VIM 打开 rust.sh，替换 RUSTUP_UPDATE_ROOT 为：

```
RUSTUP_UPDATE_ROOT="https://mirrors.ustc.edu.cn/rust-static/rustup"
```

保存退出。

然后修改环境变量：

```
export RUSTUP_DIST_SERVER=https://mirrors.tuna.tsinghua.edu.cn/rustup
```

## 安装

给安装脚本加上可执行权限，然后执行即可开始下载并安装 Rust：

```
chmod +x rust.sh
./rust.sh
```

首先会看到提示：

```
Current installation options:


   default host triple: x86_64-unknown-linux-gnu
     default toolchain: stable (default)
               profile: default
  modify PATH variable: yes

1) Proceed with installation (default)
2) Customize installation
3) Cancel installation
>
```

这里使用默认设置，敲回车即可。

## 安装日志

```
info: profile set to 'default'
info: default host triple is x86_64-unknown-linux-gnu
info: syncing channel updates for 'stable-x86_64-unknown-linux-gnu'
info: latest update on 2023-10-05, rust version 1.73.0 (cc66ad468 2023-10-03)
info: downloading component 'cargo'
  7.8 MiB /   7.8 MiB (100 %)   4.5 MiB/s in  7s ETA:  0s
info: downloading component 'clippy'
info: downloading component 'rust-docs'
 13.8 MiB /  13.8 MiB (100 %)   4.3 MiB/s in  3s ETA:  0s
info: downloading component 'rust-std'
 24.7 MiB /  24.7 MiB (100 %)   5.6 MiB/s in  5s ETA:  0s
info: downloading component 'rustc'
 61.6 MiB /  61.6 MiB (100 %)   5.7 MiB/s in 17s ETA:  0s
info: downloading component 'rustfmt'
info: installing component 'cargo'
info: installing component 'clippy'
info: installing component 'rust-docs'
 13.8 MiB /  13.8 MiB (100 %)   1.1 MiB/s in  1m 29s ETA:  0s
info: installing component 'rust-std'
 24.7 MiB /  24.7 MiB (100 %)  14.2 MiB/s in  2s ETA:  0s
 13 IO-ops /  13 IO-ops (100 %)  12 IOPS in  1s ETA:  0s
info: installing component 'rustc'
 61.6 MiB /  61.6 MiB (100 %)  16.9 MiB/s in  3s ETA:  0s
 16 IO-ops /  16 IO-ops (100 %)   3 IOPS in 10s ETA:  0s
info: installing component 'rustfmt'
info: default toolchain set to 'stable-x86_64-unknown-linux-gnu'

stable-x86_64-unknown-linux-gnu installed - rustc 1.73.0 (cc66ad468 2023-10-03)

Rust is installed now. Great!

To get started you may need to restart your current shell.
This would reload your PATH environment variable to include
Cargo's bin directory ($HOME/.cargo/bin).

To configure your current shell, run:
source "$HOME/.cargo/env"
```

## 查看版本

退出终端，重新打开终端：

```
$ cargo --version
cargo 1.73.0 (9c4383fb5 2023-08-26)
```

可以看到版本号，说明安装成功。

注：cargo 是官方的 Rust 包管理工具。

Cargo，中文意思为“（船或飞机装载的）货物” 等

