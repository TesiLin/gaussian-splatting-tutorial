# 使用 tmux 防止长时间任务因网络波动中断

在服务器上执行耗时较长的命令时（例如 `bash xxx.sh`、`pip install`、`uv pip install`、`conda install`、训练脚本等），**建议先进入 tmux 会话再运行**。这样即使本地电脑网络波动、SSH 断开、电脑合盖或终端意外关闭，服务器上的任务也会继续运行，不会被中断。

具体操作问ai或者搜教程，非常简单。关键词：linux，tmux
