# Gaussian Splatting 教程

[English Tutorial](README.md)

本项目为 GIS 暑期实践课程准备，目标是学习一种新的计算机视觉技术，Gaussian Splatting（高斯泼溅），并进行 3DGS 和 2DGS 的简单动手实践。

## 安装

### GPU 设置

请参阅 [AiStation_environments.md](./remote-dev-notes/AiStation_environments.md)。

更多关于平台 / SSH / IDE 等的信息，请参阅 [remote-dev-notes](./remote-dev-notes/README.md)。

步骤：
1. 创建一个GPU environment，并挂载好数据和代码目录
2. 设置SSH反向代理
3. 进入下一步，拉取代码和创建conda环境

### conda

```Shell
cd /workspace/code
git clone --recursive https://github.com/TesiLin/gaussian-splatting-tutorial.git

conda create --name gs -y python=3.8
conda activate gs

pip install uv

uv pip install torch==2.1.2+cu118 torchvision==0.16.2+cu118 --extra-index-url https://download.pytorch.org/whl/cu118

conda install -c "nvidia/label/cuda-11.8.0" cuda-toolkit

uv pip install -r requirements.txt --no-build-isolation
```

## 数据集

下载数据集并将其放置在 `dataset` 文件夹下。

预期的文件夹结构为：

```text
dataset/
+-- dxq0629_bbox_959_1961/
    +-- images/
    +-- sparse/
+-- dxq0629_colmap/
    +-- images/
    +-- sparse/
```

## 使用

运行 3DGS：

```Shell
cd code/3dgs
bash assemble_dxq.sh
```

运行 2DGS：

```Shell
cd code/2dgs
bash scripts/quicktest_dxq0629.sh
```

### 调整参数

打开对应的 shell 脚本，根据需要调整以下参数：

| 参数 | 说明 | 建议值 |
|---|---|---|
| `-r <N>` | 将训练图像分辨率降低为原来的 1/N | `4`（避免 20 GB 显存不足） |
| `--iterations <N>` | 训练迭代次数 | 3DGS 跑 `dxq0629_colmap` 建议 30000，2DGS 跑 `dxq0629_bbox_959_1961` 建议 60000 |

### 常见问题

#### 1. FileNotFoundError: No such file or directory: 'output/.../cfg_args'

训练步骤（`train.py`）未执行。请确保脚本中 `train.py` 和 `render.py` 均已取消注释，且 `DATASET` 变量指向了正确的数据路径：

- 3DGS：`DATASET="../../dataset/dxq0629_colmap"`
- 2DGS：`DATASET="../../dataset/dxq0629_bbox_959_1961"`

#### 2. RuntimeError: NVML_SUCCESS == r INTERNAL ASSERT FAILED (CUDACachingAllocator.cpp)

这是显存溢出（OOM）错误——20 GB 显存对大场景可能不够用。

解决方案：

1. 增大降采样系数：将训练命令中的 `-r 2` 改为 `-r 4`。
2. 换用 40 GB 显存的 GPU（如 A100）。

> **注意：** 40 GB 显卡资源有限，请已完成实习内容的同学尽早保存数据、释放环境资源（暂停环境运行）。环境为一次性容器，请在暂停前确认已经保存好训练结果。

#### 3. 高斯泼溅 / Mesh 结果质量很差

迭代次数不够，优化不充分。增加 `--iterations`：

- 3DGS 跑 `dxq0629_colmap`：`--iterations 30000`
- 2DGS 跑 `dxq0629_bbox_959_1961`：`--iterations 60000`

2DGS 示例：

```Shell
python train.py -s "$DATASET" -m "$MODEL" --iterations 60000
```

## 可视化

- Gaussian Splatting 结果：本地部署 [SuperSplat](https://github.com/playcanvas/supersplat) 进行预览。
- Mesh 结果：使用 [MeshLab](https://www.meshlab.net/#download) 进行预览。

## 致谢

本教程基于以下开源项目构建：

- [3D Gaussian Splatting](https://github.com/graphdeco-inria/gaussian-splatting)
- [2D Gaussian Splatting](https://github.com/hbb1/2d-gaussian-splatting)
