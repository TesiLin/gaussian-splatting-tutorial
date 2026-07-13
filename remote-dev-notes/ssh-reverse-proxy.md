# 通过 SSH 反向隧道让 Linux 服务器使用本地电脑网络

场景：
本地电脑可以联网，远程 Linux 服务器需要借用本地电脑的网络。

作用：
方便拉取github仓库和配conda

示例服务器：

```
服务器地址：10.188.10.10
SSH 端口：22
登录用户：root
```

示例本地代理端口：1080

## 1 在本地电脑执行 SSH 隧道命令

在本地电脑的终端里执行：

```
ssh -N -R 127.0.0.1:1080 root@10.188.10.10 -p 22
```

执行后不要关闭这个终端窗口。

## 2 登录远程 Linux 服务器

重新打开一个终端，正常登录服务器：

```
ssh root@10.188.10.10 -p 22
```

## 3 在 Linux 服务器上设置代理环境变量

在 Linux 服务器终端里执行：

```
export ALL_PROXY=socks5h://127.0.0.1:1080
export all_proxy=socks5h://127.0.0.1:1080
```

## 4 测试是否可用

在 Linux 服务器上执行：

```
curl -I https://www.baidu.com
```

如果能返回 HTTP 响应头，说明代理可用。

## 5 结束代理

关闭第 1 步中本地电脑运行 ssh -N ... 的终端窗口即可。

## Q1 解决 pip 报 Missing dependencies for SOCKS support

如果执行 `pip install uv` 时出现：

```
ERROR: Could not install packages due to an OSError: Missing dependencies for SOCKS support.
```

原因是 `pip` 读取了 `ALL_PROXY=socks5h://...`，但当前 Python 环境还没有安装 SOCKS 支持。可以先用 `curl` 走代理下载 PySocks 的 wheel，再让 `pip` 本地安装。

### 1 先设置 SOCKS 代理

```
export ALL_PROXY=socks5h://127.0.0.1:1080
export all_proxy=socks5h://127.0.0.1:1080
```

### 2 用 curl 通过代理下载 PySocks

```
curl -L --socks5-hostname 127.0.0.1:1080 \
  -o PySocks.whl \
  https://files.pythonhosted.org/packages/8d/59/b4572118e098ac8e46e399a1dd0f2d85403ce8bbaad9ec79373ed6badaf9/PySocks-1.7.1-py3-none-any.whl
```

### 3 临时取消代理

```
unset ALL_PROXY
unset all_proxy
```

这一步只是让 `pip` 不再尝试走 SOCKS 代理。此时安装的是本地文件，不需要服务器联网。

### 4 重命名为合法的 wheel 文件名

`pip` 安装 wheel 时要求文件名符合规范，不能直接叫 `PySocks.whl`。

```
mv PySocks.whl PySocks-1.7.1-py3-none-any.whl
```

### 5 本地安装 PySocks

```
python -m pip install ./PySocks-1.7.1-py3-none-any.whl
```

### 6 重新设置代理并安装 uv

```
export ALL_PROXY=socks5h://127.0.0.1:1080
export all_proxy=socks5h://127.0.0.1:1080

python -m pip install uv
```

如果仍然报错，可以确认 PySocks 是否安装到了当前 Python 环境：

```
python -m pip show PySocks
python -m pip --version
```
