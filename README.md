# Multiple Versions of CUDA Libraries On The Same Arch Linux Machine

This file contains some useful aliases and way to handle multiple CUDA versions in Arch-based distributions.
After doing all steps from [CUDA installation](#cuda-installation), reboot system. Initially, CUDA will not be set up. 
You need to set it using alias **_set-cuda-X-Y_**. 
If you want to change version, you must first use **_unset-cuda_** alias, and then set new version.

## Edit content of `~/.bashrc`

```
# some useful aliases, not related to CUDA
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
```

-------------------

```
# Alias definitions.
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them here directly.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.

if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi

# All things that you want to add to the path on login add before this comment!
RVG_PATH=$PATH
```

## Create if does not exist, otherwise edit `~/.bash_aliases`

```
alias clean-path='export PATH="${RVG_PATH}"'  
alias clear-path='export PATH="${RVG_PATH}"'  
  
alias unset-cuda='export PATH="${RVG_PATH}"'  
alias unset-cudas='export PATH="${RVG_PATH}"'  
alias set-cuda-9-2='export PATH="${RVG_PATH}:/opt/cuda-10.0/bin"'  
alias set-cuda-8-0='export PATH="${RVG_PATH}:/opt/cuda-11.0/bin"'
  
alias gpu-temp="nvidia-smi --query-gpu=temperature.gpu --format=csv,noheader"  
```


## CUDA installation

1. Download CUDA & CUDNN from either https://archive.archlinux.org/packages/c or https://archive.org/download/archlinux_pkg_cuda if too old.
2. Install CUDA & CUDNN: `pacman -U cuda-X-Y cudnn`.
3. Rename `/opt/cuda` to `/opt/cuda-X.Y`: `mv /opt/cuda /opt/cuda-X.Y` or `cp /opt/cuda /opt/cuda-X.Y`
4. Rename `/etc/ld.so.conf.d/cuda.conf` to `/etc/ld.so.conf.d/cuda-X.Y.conf`, and change the content of the file to point to correct location.
5. Uninstall CUDA & CUDNN: `pacman -R cudnn cuda` (first remove cudnn).

----------------
5. (Optional) Remove file `/etc/profile.d/cuda.sh.` if it still exists.


Repeat steps 1-5 to install all versions you need. At the end, if you want to have the latest version that will receive regular updates, you can install it with `pacman -S cuda cudnn`. `pacman` will update only that version.
After system reboot, you should be able to activate *X.Y* version, simply typing in terminal **set-cuda-X-Y**.
