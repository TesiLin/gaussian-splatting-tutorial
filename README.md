# Gaussian Splatting Tutorial

[中文教程](README_CN.md)

This project is prepared for the GIS summer practice course. Its goal is to learn a new computer vision technique, Gaussian Splatting, and run a simple hands-on practice with 3DGS and 2DGS.

## Install

### GPU settings

Please refer to [AiStation_environments.md](./remote-dev-notes/AiStation_environments.md).

More information related to the platform / SSH / IDE / ..., please refer to [remote-dev-notes](./remote-dev-notes/README.md).

Steps:
1. Create a GPU environment and mount the data and code directories
2. Set up SSH reverse proxy
3. Proceed to the next step to pull the code and create the conda environment

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

## Dataset

Download the dataset and place it under the `dataset` folder.

The expected folder structure is:

```text
dataset/
+-- dxq0629_bbox_959_1961/
    +-- images/
    +-- sparse/
+-- dxq0629_colmap/
    +-- images/
    +-- sparse/
```

## Usage

Run 3DGS:

```Shell
cd code/3dgs
bash assemble_dxq.sh
```

Run 2DGS:

```Shell
cd code/2dgs
bash scripts/quicktest_dxq0629.sh
```

## Visualize

- Gaussian splatting results: deploy the [SuperSplat](https://github.com/playcanvas/supersplat) locally for preview.
- Mesh results: use [MeshLab](https://www.meshlab.net/#download) for preview

## Acknowledgements

This tutorial is built on top of the following open-source projects:

- [3D Gaussian Splatting](https://github.com/graphdeco-inria/gaussian-splatting)
- [2D Gaussian Splatting](https://github.com/hbb1/2d-gaussian-splatting)
