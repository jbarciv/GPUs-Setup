# new-setup

## Description

The next respository summarizes the basics steps to run a small cluster of GPUs. Basic tips and many links are provided to facilitate research.

## Table of Contents

- [Server](#server)
- [Virtual environment](#venv)
- [CUDA and Python: Numba](#numba)

## Server

For avoiding the need of writting the password every time you need to login in the server you can use this [link](https://urldefense.com/v3/__https://www.thegeekstuff.com/2008/11/3-steps-to-perform-ssh-login-without-password-using-ssh-keygen-ssh-copy-id/__;!!KwNVnqRv!A_x86MYnut7a45t1fPXr1g2zn_sYDNoHeCdK5_ysde-LDEh9dtsR7aG_QtV1xpQLKKOdqENFuXXv4tv5lTwVETFGEIA$).

We recommend to create an *alias* in ```.bashrc``` for automatizing the login, i.e., ```alias server="ssh my_user@my_server_ip"```. Then for login type *server* and enjoy!

## Venv

First be sure that *python3* is already installed. If not, you can install it this way: `sudo apt install python3`.

Then we are going to install these packages:
`sudo apt install python3-pip`,
`sudo apt-get install python3-venv` or `sudo pip install virtualenv` if *pip* does not work use this:
`python3 -m pip install <paquete>`. Where paquete is *virtualenv*.

Finally, we can create the virtual environment:

`python3 -m venv ~/.python_venv`

feel free to change *the path* for something more suitable for you.

Edit your ```.bashrc``` on your server and include an alias for activating your *venv*: i.e., ```alias myenv="source ~/.virtual/bin/activate"```, change the folder structure with yours. For exiting the *venv*, just type ```deactivate```.

## Numba 

First check the [Nvidia website](https://developer.nvidia.com/how-to-cuda-python). Also, this [link](https://github.com/ContinuumIO/gtc2017-numba) is a good start point for Numba (Python + CUDA). We will be using Numba in Deep Learning Research.

Prerequisites before installing:
- (1) CUDA-capable GPUs
- (2) CUDA Toolkit installed

> Check (1) with this command on your server: ```lspci | grep -i nvidia```. \\
> Install CUDA Toolkit downloading [this](https://developer.nvidia.com/cuda-downloads) and following the next instruction (for linux) [here](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).
