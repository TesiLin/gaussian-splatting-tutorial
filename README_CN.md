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

## 可视化

- Gaussian Splatting 结果：本地部署 [SuperSplat](https://github.com/playcanvas/supersplat) 进行预览。
- Mesh 结果：使用 [MeshLab](https://www.meshlab.net/#download) 进行预览。

## 致谢

本教程基于以下开源项目构建：

- [3D Gaussian Splatting](https://github.com/graphdeco-inria/gaussian-splatting)
- [2D Gaussian Splatting](https://github.com/hbb1/2d-gaussian-splatting)
