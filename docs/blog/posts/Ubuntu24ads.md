---
draft: no
date: 2024-11-01
categories:
  - tools
tags:
  - Ubuntu 24.04
  - ADS
  - 课程软件
---
# Ubuntu 24.04 安装 ads2020Update2 破解版

课程需要使用 ads 仿真，但是只给了 windows 的安装包。以下是在 Ubuntu 24.04 上安装的过程。

<!-- more -->

首先安装必要的程序和库：

```bash
sudo ln -s /lib64/ld-linux-x86-64.so.2 /lib64/ld-lsb-x86-64.so.3
sudo apt install ksh
```

[官网](https://www.keysight.com/main/software.jspx?ckey=2212036&nid=-11143.0.00&cc=eng&lc=eng)下载 ads2020Update2。安装。

```bash
tar –xvf ads_2020_update2.0_linux_x64.tar
sudo SETUP.SH
```

默认的安装目录是`/usr/local/ADS2020_update2`

设置环境变量：

```bash
export HPEESOF_DIR="/usr/local/ADS2020_update2"
export ADS_LICENSE_FILE="/usr/local/ADS2020_update2/license.lic"
export PATH="$HPEESOF_DIR/bin:$PATH"
```

下载破解文件。

```bash
cp ./pubkey_verify $HPEESOF_DIR
cp ./license.lic $HPEESOF_DIR
sudo $HPEESOF_DIR/pubkey_verify -y
$HPEESOF_DIR/Licensing/2020.20/linux_x86_64/bin/aglmmgr
```

