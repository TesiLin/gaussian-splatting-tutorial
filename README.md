# Gaussian Splatting Tutorial

This project is prepared for the GIS summer practice course. Its goal is to learn a new computer vision technique, Gaussian Splatting, and run a simple hands-on practice with 3DGS and 2DGS.

## Install

Use the public image on the platform: `cuda118_ubuntu2004_jdk-g22438017lhx:20250325`

Recommended mount paths:

| Data Path                  | Mount Path             | Usage      | Mount Type |
| -------------------------- | ---------------------- | ---------- | ---------- |
| /<USER_PATH>/vscode-server | /root/.vscode-server   | Direct use | Normal     |
| /<USER_PATH>/envs          | /root/miniforge3/envs/ | Direct use | Normal     |
| /<USER_PATH>/code          | /workspace/code        | Direct use | Normal     |

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

Download the dataset and place it under the `dataset` folder.  /wThe expected folder structure is:

```text
dataset/
+-- dxq0629_bbox_959_1961/
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

- Gaussian splatting results: use [SuperSplat](https://playcanvas.com/supersplat/editor) for preview
- Mesh results: use [MeshLab](https://www.meshlab.net/#download) for preview

## Acknowledgements

This tutorial is built on top of the following open-source projects:

- [3D Gaussian Splatting](https://github.com/graphdeco-inria/gaussian-splatting)
- [2D Gaussian Splatting](https://github.com/hbb1/2d-gaussian-splatting)
