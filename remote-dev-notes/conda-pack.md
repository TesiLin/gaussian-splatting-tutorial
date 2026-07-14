# conda pack 用法：打包 & 迁移 conda 环境

当服务器无法联网时，可以使用 `conda pack` 在本机打包 conda 环境，上传到服务器后直接解压使用。

---

## 1. 获取打包好的环境

从组共享目录中获取 `conda-gs.tar.gz`，复制到个人用户目录的 `envs` 文件夹中（即平台存储挂载的 `/root/miniforge3/envs/` 对应路径）。

---

## 2. SSH 连接服务器，解压环境

```bash
# ssh 接入服务器后

cd /root/miniforge3/envs

# 清理已有的 gs 环境
conda activate base
conda remove -n gs --all
rm -r gs

# 新建环境目录并解压
mkdir -p gs
tar -xzf conda-gs.tar.gz -C gs
```

---

## 3. 重装 CUDA 扩展

解压后的环境包含已编译的 CUDA 扩展，但这些二进制文件与当前 GPU 驱动/CUDA 版本可能不兼容，需要卸载后重新编译安装。

```bash
# 激活环境
conda activate gs

# 卸载旧的 CUDA 扩展
uv pip uninstall diff-gaussian-rasterization fused-ssim simple-knn diff-surfel-rasterization
```

```bash
# 进入项目目录
cd /workspace/code/gaussian-splatting-tutorial

# 清理之前的构建产物：每个子模块下的 build 和 .egg-info 文件夹
find ./code/3dgs/submodules/diff-gaussian-rasterization ./code/3dgs/submodules/fused-ssim \
     ./code/3dgs/submodules/simple-knn ./code/2dgs/submodules/diff-surfel-rasterization \
     -maxdepth 1 \( -name build -o -name '*.egg-info' \) -exec rm -r {} +
```

然后依次安装四个 CUDA 扩展：

```bash
# 3DGS CUDA extensions
uv pip install ./code/3dgs/submodules/diff-gaussian-rasterization --no-build-isolation
uv pip install ./code/3dgs/submodules/fused-ssim --no-build-isolation
uv pip install ./code/3dgs/submodules/simple-knn --no-build-isolation

# 2DGS CUDA extensions
uv pip install ./code/2dgs/submodules/diff-surfel-rasterization --no-build-isolation
```

至此，gs 环境配置完成。
