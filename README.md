# DogePanel

DogePanel 是在网页上管理 Linux 服务器的面板，基于 [1Panel](https://github.com/1Panel-dev/1Panel)。网站、PHP、MySQL、Redis、文件、计划任务、备份和防火墙，都在浏览器里管理。

English: see [below](#dogepanel-english).

## 安装

准备好：

- 一台 64 位的 Linux 服务器，推荐全新安装的 Debian 12、Ubuntu 22.04 / 24.04、Rocky Linux 9 或 AlmaLinux 9；
- 用 root 用户登录服务器（不是 root 的话，先运行 `sudo -i`）；
- 向维护者要**安装密码**。

**第 1 步**：复制下面这一行，粘贴到服务器的终端里，按回车。

```bash
curl -fsSLO https://github.com/domain1286666/DogePanel-releases/releases/latest/download/quick_start.sh && bash quick_start.sh
```

**第 2 步**：看到“请输入安装密码”时，输入维护者给你的密码，按回车。输入时屏幕上什么都不显示，这是正常的。

之后什么都不用做，等几分钟就装好了。

**第 3 步**：装好以后会显示登录信息，比如：

```
访问地址：
    http://10.0.0.8:28513/a1b2c3d4e5
用户名： user1234ab
密码： Ab3dEf6hIj9kLm2n
```

马上把这三项记下来，**密码只显示这一次**。

## 登录

1. 把访问地址里的 IP 换成服务器的**公网 IP**（在云服务商的控制台里能看到），在浏览器里打开。地址最后那一段不能少。
2. 打不开的话，多半是云服务商的**安全组**没有放行：在控制台里放行访问地址里的端口（TCP），再试一次。
3. 输入用户名和密码，勾选下面的“同意《飞致云社区软件许可协议》”，点“登录”。

登录以后：

- 在“软件”页面装好 OpenResty 和 PHP，就可以建网站了；
- 建议在“面板设置 → 安全”里开启两步验证和 HTTPS。

## 忘了地址或密码

在服务器上运行：

| 命令 | 作用 |
| --- | --- |
| `dgpctl user-info` | 查看访问地址和用户名 |
| `dgpctl update password` | 设置新密码 |
| `dgpctl status` | 查看面板是不是在运行 |
| `dgpctl restart` | 重启面板 |

## 升级

再运行一次安装命令就行，同样要输入安装密码：

```bash
curl -fsSLO https://github.com/domain1286666/DogePanel-releases/releases/latest/download/quick_start.sh && bash quick_start.sh
```

升级前会自动备份，新版本启动不了时会自动换回原来的版本。网站和数据不受影响。

## 常见问题

**提示 `curl: command not found`**

先装 curl，再运行安装命令。Debian、Ubuntu 运行 `apt install -y curl`；CentOS、Rocky、AlmaLinux 运行 `yum install -y curl`。

**国内服务器**

- 安装文件在 GitHub 上，国内有时下载慢或者中断，再运行一次安装命令就行。
- 服务器的时区是中国时，Docker 会自动从阿里云的镜像安装。
- 使用应用商店之前，先在“容器 → 配置”里设置“镜像加速”，不然可能拉不到 Docker 镜像。

**Docker 没装上**

面板照样能用：网站、数据库和软件页面都不用 Docker，只有应用商店和容器要用。按 [Docker 的安装说明](https://docs.docker.com/engine/install/) 装好 Docker 以后，就都能用了。Ubuntu 20.04 这类已经停止支持的系统，Docker 不再提供安装包，可以运行 `apt install -y docker.io` 装系统自带的 Docker。

**不想装 Docker**

在安装命令最后加上 ` --skip-docker`。

**想自己定端口、用户名和密码**

在安装命令最后加上 ` --interactive`，安装时会一项一项问你。

**提示 OpenSSL 太旧（CentOS 7 等老系统）**

看下面的“离线安装”。

## 卸载

运行 `dgpctl uninstall`。它会问你要不要删除面板的数据；`/www` 下的网站和备份不会删除。

---

## 进阶

### 安装参数

参数加在安装命令的最后，比如 `bash quick_start.sh --port 8443 --skip-docker`：

| 参数 | 作用 |
| --- | --- |
| `--port 8443` | 面板端口（默认随机） |
| `--interactive` | 安装时询问端口、安全入口、用户名、密码，以及要不要装 Docker |
| `--skip-docker` | 不装 Docker |
| `--docker-mirror aliyun` | Docker 从哪里安装：`auto`（默认：时区在中国大陆时用阿里云，否则用官方源）、`official`、`aliyun`、`azure`，或者镜像地址 |
| `--registry-mirror https://<地址>` | Docker Hub 的镜像加速地址 |
| `--language en` | 面板的语言：`zh`、`zh-Hant`、`en`、`ja`、`ko` 等 |
| `--release-password-file <文件>` | 从文件的第一行读安装密码，不用手动输入 |

`bash quick_start.sh --help` 列出它自己的参数。

### 离线安装

服务器不能访问 GitHub，或者 OpenSSL 低于 1.1.1（比如 CentOS 7）时：

1. 在一台能上网的 Linux 机器上（OpenSSL 1.1.1 以上，不需要 root），从[发布页](https://github.com/domain1286666/DogePanel-releases/releases/latest)下载 `quick_start.sh` 和 `1panel-<版本>-linux-amd64.tar.gz.enc`（ARM 服务器下载 `arm64` 的），放在同一个目录，解密：

   ```bash
   bash quick_start.sh --decrypt 1panel-<版本>-linux-amd64.tar.gz.enc
   ```

2. 把 `quick_start.sh` 和解出来的 `1panel-<版本>-linux-amd64.tar.gz` 复制到服务器的同一个目录，用 root 运行：

   ```bash
   bash quick_start.sh --package ./1panel-<版本>-linux-amd64.tar.gz
   ```

服务器的 OpenSSL 是 1.1.1 以上时，也可以直接把 `.enc` 文件和 `quick_start.sh` 复制过去，运行 `bash quick_start.sh --package ./1panel-<版本>-linux-amd64.tar.gz.enc`。

### 装好以后的目录

| 位置 | 内容 |
| --- | --- |
| `/opt/dogepanel/` | 面板的程序、数据和 `dgpctl` |
| `/www/` | 网站、网站日志、软件（`/www/server`）和备份（`/www/backup`） |

位置是固定的。磁盘不够时，把大硬盘挂载到 `/www`。

### 安全

- 每个文件都先校验，再解密、安装。`quick_start.sh` 里有它这个版本所有文件的 SHA-256；每个版本另外附带签名的清单 `manifest.json`、`manifest.json.sig`，装别的版本时用它校验（要 OpenSSL 3.0）。文件被改过、密码不对，都会停下来，不安装任何东西。
- 安装过程不下载其他脚本，不上报任何信息；只有安装 Docker 时，会连接所选的 Docker 软件包来源。
- 面板的密码只在安装结束时显示一次，不写进日志。
- 安装密码只用来下载安装文件，请不要公开。

### 每个版本里的文件

除了 `quick_start.sh`，每个版本的文件都用安装密码加密（`.enc`）。

| 文件 | 内容 |
| --- | --- |
| `quick_start.sh` | 安装和升级脚本，内置发版公钥和这个版本所有文件的 SHA-256 |
| `1panel-<版本>-linux-<架构>.tar.gz.enc` | 安装包 |
| `aapanel-export-linux-<架构>.enc` | 从别的服务器上的 aaPanel 迁移时，在 aaPanel 服务器上运行的导出工具（同一台服务器上的 aaPanel 不用它，在面板的“网站 → 从 aaPanel 导入”里接管） |
| `dogepanel-<版本>-source.tar.gz.enc` | 这个版本的完整源码 |
| `manifest.json`、`manifest.json.sig` | 每个文件（加密前和加密后）的 SHA-256，以及签名 |

导出工具和源码也用 `quick_start.sh` 解密，结果放在同一个目录，去掉 `.enc`：

```bash
bash quick_start.sh --decrypt aapanel-export-linux-amd64.enc
```

`quick_start.sh` 要和要解密的文件是同一个版本的；别的版本的文件，要把那个版本的 `manifest.json`、`manifest.json.sig` 放在旁边（要 OpenSSL 3.0）。

Release 页面上 GitHub 自动附带的 “Source code (zip/tar.gz)” 是这个仓库本身（只有说明文件），不是 DogePanel 的源码；DogePanel 的源码是 `dogepanel-<版本>-source.tar.gz.enc`。

### 软件的二进制包

`software-<日期>` 这些 Release 不是面板的版本，放的是面板“软件”页面二进制安装用的包（OpenResty、PHP、Redis、Valkey），不加密。面板安装时自己下载，并核对编进面板里的 SHA-256。

- 每个包里的 `.dgp/` 有编它的脚本和源码里的许可文件。
- 同一个 Release 里有编这些包用的上游源码包（原样）。Redis 8 按 AGPLv3 提供，对应的源码就是那里的 `redis-<版本>.tar.gz`。
- `manifest.json`、`manifest.json.sig` 是所有文件的 SHA-256 和签名。

### 许可

DogePanel 和 1Panel 一样以 GPL-3.0 发布，见 [LICENSE](LICENSE)。

---

# DogePanel (English)

DogePanel is a web panel for Linux servers, based on [1Panel](https://github.com/1Panel-dev/1Panel): websites, PHP, MySQL, Redis, files, scheduled tasks, backups and the firewall, in the browser.

## Install

You need a 64-bit Linux server (a fresh Debian 12, Ubuntu 22.04 / 24.04, Rocky Linux 9 or AlmaLinux 9 is best), root (`sudo -i`), and the installation password from the maintainer. Run:

```bash
curl -fsSLO https://github.com/domain1286666/DogePanel-releases/releases/latest/download/quick_start.sh && bash quick_start.sh
```

Type the installation password when asked; it is not shown as you type. Nothing else is asked. At the end, the address, user name and password of the panel are shown, the password only this once: write them down.

Open the address in a browser with the public IP of the server. In a cloud, first allow the port of the address (TCP) in the security group. Sign in with the user name and password, after ticking the license agreement under the button.

- Upgrade: run the same command again. The current version is backed up first and comes back if the new one does not start.
- `dgpctl user-info` shows the address and user name; `dgpctl update password` sets a new password; `dgpctl uninstall` uninstalls.
- `--interactive` asks for the port, user name, password and Docker. `--skip-docker` installs no Docker: only the app store and containers need it, websites and databases do not. If Docker cannot be installed, the panel is installed all the same; on systems that Docker no longer supports, such as Ubuntu 20.04, `apt install -y docker.io` installs the Docker of the system.
- Offline, or with OpenSSL older than 1.1.1 (CentOS 7): on another machine, decrypt the package with `bash quick_start.sh --decrypt 1panel-<version>-linux-amd64.tar.gz.enc`, copy the `.tar.gz` and `quick_start.sh` to the server, and run `bash quick_start.sh --package ./1panel-<version>-linux-amd64.tar.gz`.

Every file is checked before it is decrypted or installed: `quick_start.sh` has the SHA-256 of all files of its release built in, and each release has a signed manifest (`manifest.json`, `manifest.json.sig`) for other versions, which needs OpenSSL 3.0. Nothing is reported anywhere, and no other script is downloaded.

Every file of a release but `quick_start.sh` is encrypted (`.enc`) with the installation password: the package, the aaPanel export tool for migrating from aaPanel on another server, and the source code, `dogepanel-<version>-source.tar.gz.enc`. The "Source code" archives that GitHub adds to every release contain this repository only.

The `software-<date>` releases are not versions of the panel: they hold the binary packages that its Software page installs (OpenResty, PHP, Redis, Valkey), not encrypted, which the panel checks against the SHA-256 built into it. `.dgp/` in each package has the scripts it was built with and the license files of its source; the source archives of upstream are in the same release (Redis 8 is distributed under AGPLv3: its source is the `redis-<version>.tar.gz` there).

DogePanel is licensed under GPL-3.0, like 1Panel (see [LICENSE](LICENSE)).
