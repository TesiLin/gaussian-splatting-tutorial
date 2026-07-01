# A tutorial for gaussian-splatting


## Install

```Shell
git clone --recursive https://github.com/TesiLin/gaussian-splatting-tutorial.git

conda create --name gs -y python=3.8
conda activate gs

pip install uv

uv pip install torch==2.1.2+cu118 torchvision==0.16.2+cu118 --extra-index-url https://download.pytorch.org/whl/cu118

conda install -c "nvidia/label/cuda-11.8.0" cuda-toolkit

uv pip install -r requirements.txt --no-build-isolation
```


## Dataset



## Train

