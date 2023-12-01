# Setup Paperspace Instance for PyTorch


## Set up

Product: Paperspace CORE

Machine: M4000, 50GB, public, static IP, SSH login

## ML Problem

NLP Prefix tuning using PyTorch, too slow on CPU locally. (peft module, gpt2distl base model). Local run takes 3 hours with only 5 virtualtoken and no prefixprojection. 

## Update Driver

Default, ML repo installs torch version that is not compatible with paperspace core CUDA version. Getting warning while running training: your cuda versioin is too old for pytorch. 

### Tried: install new CUDA versioin -> turns out a bad idea: cannot install it because some c++/compiler issue. To solve it, probably need to recompile NVIDIA kernels. 

[Unload CUDA drm](https://unix.stackexchange.com/questions/440840/how-to-unload-kernel-module-nvidia-drm)
```
3) Unload nvidia-drm before proceeding.

3a) Isolate multi-user.target

sudo systemctl isolate multi-user.target
3b) Note that nvidia-drm is currently in use.

lsmod | grep nvidia.drm
3c) Unload nvidia-drm

sudo modprobe -r nvidia-drm
4d) Note that nvidia-drm is not in use anymore.

lsmod | grep nvidia.drm
5) Go to your download folder and run the cuda installation.

sudo sh cuda_10.1.168_418.67_linux.run
```

NVIDIA-Linux-x86_64-535.129.03.run

### In the end, install backward pytorch version

found that pytorch with CUDA11.70(current) is forward-compatible, better to install correct torch.

`pip3 install -U torch==1.13.0+cu117 --extra-index-url https://download.pytorch.org/whl/cu117`

Then run `pip -r requirements` for other dependencies.

Install this after installing `ipykernel`

### Install ipykernel

1. activate your conda env
2. `conda install -c anaconda ipykernel`
3. `python -m ipykernel install --user --name=firstEnv`
4. Install other python packages including torch...
