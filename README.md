# DogePanel 发布文件

这个仓库只放 DogePanel 的发布文件，没有代码。DogePanel 基于 [1Panel](https://github.com/1Panel-dev/1Panel)，和它一样以 GPL-3.0 发布，见 [LICENSE](LICENSE)。

除了 `quick_start.sh`，每个版本的文件都是加密的（`.enc`），密码由维护者提供。

## 安装和升级

用 root 运行，按提示输入发布文件的密码：

```bash
curl -fsSLO https://github.com/domain1286666/DogePanel-releases/releases/latest/download/quick_start.sh
bash quick_start.sh
```

- 已经安装时，同一个命令会升级。
- 不想手动输入密码：把密码写进一个只有 root 能读的文件，加上 `--release-password-file <文件>`。
- 离线安装：把 `1panel-<版本>-linux-<架构>.tar.gz.enc`、`manifest.json`、`manifest.json.sig` 和 `quick_start.sh` 放在同一个目录，运行 `bash quick_start.sh --package ./1panel-<版本>-linux-amd64.tar.gz.enc`。
- 其他参数交给安装包里的 `install.sh`，比如 `bash quick_start.sh --port 8443 --skip-docker`；`bash quick_start.sh --help` 列出它自己的参数。
- 每个文件都先校验签名，再解密、解压，需要 OpenSSL 3.0 或更新的版本。

## 每个版本里的文件

| 文件 | 内容 |
| --- | --- |
| `quick_start.sh` | 安装和升级脚本，内置发版公钥 |
| `1panel-<版本>-linux-<架构>.tar.gz.enc` | 安装包 |
| `aapanel-export-linux-<架构>.enc` | 从 aaPanel 迁移时，在 aaPanel 服务器上运行的导出工具 |
| `dogepanel-<版本>-source.tar.gz.enc` | 这个版本的完整源码 |
| `manifest.json`、`manifest.json.sig` | 每个文件（加密前和加密后）的 SHA-256，以及签名 |

导出工具和源码用 `quick_start.sh` 解密，它会先校验签名，解密结果放在同一个目录，去掉 `.enc`：

```bash
bash quick_start.sh --decrypt aapanel-export-linux-amd64.enc
```

要和 `manifest.json`、`manifest.json.sig` 放在同一个目录。这一步不需要 root，可以在任何有 OpenSSL 3.0 的 Linux 机器上做，再把解密后的文件复制过去。

Release 页面上 GitHub 自动附带的 “Source code (zip/tar.gz)” 是这个仓库本身（只有说明文件），不是 DogePanel 的源码；DogePanel 的源码是 `dogepanel-<版本>-source.tar.gz.enc`。

---

# DogePanel releases

This repository holds the release files of DogePanel, no code. DogePanel is based on [1Panel](https://github.com/1Panel-dev/1Panel) and, like it, licensed under GPL-3.0 (see [LICENSE](LICENSE)).

Every file of a release but `quick_start.sh` is encrypted (`.enc`); the maintainer gives out the password.

Install or upgrade, as root, and type the password when asked:

```bash
curl -fsSLO https://github.com/domain1286666/DogePanel-releases/releases/latest/download/quick_start.sh
bash quick_start.sh
```

- `--release-password-file <file>` reads the password from a file instead.
- Offline: put the `.tar.gz.enc` package, `manifest.json`, `manifest.json.sig` and `quick_start.sh` in one directory and run `bash quick_start.sh --package ./1panel-<version>-linux-amd64.tar.gz.enc`.
- `bash quick_start.sh --decrypt <file>.enc` decrypts the aaPanel export tool or the source code, `dogepanel-<version>-source.tar.gz.enc`, next to their manifest.
- Every file is checked against the signed manifest before it is decrypted; this needs OpenSSL 3.0 or newer.

The "Source code" archives that GitHub adds to every release contain this repository only, not the source code of DogePanel.
