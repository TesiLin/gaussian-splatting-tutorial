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

### Tweaking parameters

Open the corresponding shell script and adjust the following parameters as needed:

| Parameter | Description | Suggested value |
|---|---|---|
| `-r <N>` | Downscale the training images by a factor of N | `4` (to avoid OOM on 20 GB cards) |
| `--iterations <N>` | Number of training iterations | 30k for 3DGS (`dxq0629_colmap`), 60k for 2DGS (`dxq0629_bbox_959_1961`) |

### FAQ

#### 1. FileNotFoundError: No such file or directory: 'output/.../cfg_args'

The training step (`train.py`) was skipped. Make sure both `train.py` and `render.py` are uncommented in the script, and that the `DATASET` variable points to the correct path:

- For 3DGS: `DATASET="../../dataset/dxq0629_colmap"`
- For 2DGS: `DATASET="../../dataset/dxq0629_bbox_959_1961"`

#### 2. RuntimeError: NVML_SUCCESS == r INTERNAL ASSERT FAILED (CUDACachingAllocator.cpp)

This is an out-of-memory (OOM) error — 20 GB VRAM may be insufficient for large scenes.

Solutions:

1. Increase the downscaling factor: change `-r 2` to `-r 4` in the training command.
2. Switch to a 40 GB GPU (e.g., A100).

> **Note:** 40 GB GPUs are a limited resource. Save your results and release/pause your GPU environment when you are done.

#### 3. Poor Gaussian splatting / mesh results

Insufficient training iterations. Increase `--iterations`:

- 3DGS on `dxq0629_colmap`: `--iterations 30000`
- 2DGS on `dxq0629_bbox_959_1961`: `--iterations 60000`

Example for 2DGS:

```Shell
python train.py -s "$DATASET" -m "$MODEL" --iterations 60000
```

## Visualize

- Gaussian splatting results: deploy the [SuperSplat](https://github.com/playcanvas/supersplat) locally for preview.
- Mesh results: use [MeshLab](https://www.meshlab.net/#download) for preview

## Acknowledgements

This tutorial is built on top of the following open-source projects:

- [3D Gaussian Splatting](https://github.com/graphdeco-inria/gaussian-splatting)
- [2D Gaussian Splatting](https://github.com/hbb1/2d-gaussian-splatting)
