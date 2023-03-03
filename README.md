# new-setup

## Description

The next respository summarizes the basics steps to run a small cluster of GPUs. Basic tips and many links are provided to facilitate research.

## Table of Contents

- [Server](#server)
- [Virtual environment](#venv)
- [CUDA and Python: Numba](#numba)

## Server

For avoiding the need of writting the password every time you need to login in the server you can use **ssh-keygen** and **ssh-copy-id**.

The basic instructions are:
1. Create public and private keys using ssh-key-gen on local-host --> `ssh-keygen`
```
your_user@local-host$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/jsmith/.ssh/id_rsa):[Enter key]
Enter passphrase (empty for no passphrase): [Press enter key]
Enter same passphrase again: [Pess enter key]
Your identification has been saved in /home/jsmith/.ssh/id_rsa.
Your public key has been saved in /home/jsmith/.ssh/id_rsa.pub.
The key fingerprint is:
33:b3:fe:af:95:95:18:11:31:d5:de:96:2f:f2:35:f9 jsmith@local-host
```
2. Copy the public key to remote-host using ssh-copy-id --> `ssh-copy-id -i ~/.ssh/id_rsa.pub remote-host`
```
your_user@local-host$ ssh-copy-id -i ~/.ssh/id_rsa.pub remote-host
your_user@local-host's password:
Now try logging into the machine, with "ssh 'remote-host'", and check in:

.ssh/authorized_keys

to make sure we haven't added extra keys that you weren't expecting.
```
**Note:** ssh-copy-id appends the keys to the remote-host’s .ssh/authorized_key.

3.  Login to remote-host without entering the password --> `ssh remote-host`
```
jsmith@local-host$ ssh remote-host
Last login: Sun Nov 16 17:22:33 2008 from 192.168.1.2
[Note: SSH did not ask for password.]

jsmith@remote-host$ [Note: You are on remote-host here]
```
All the info is from this [link](https://urldefense.com/v3/__https://www.thegeekstuff.com/2008/11/3-steps-to-perform-ssh-login-without-password-using-ssh-keygen-ssh-copy-id/__;!!KwNVnqRv!A_x86MYnut7a45t1fPXr1g2zn_sYDNoHeCdK5_ysde-LDEh9dtsR7aG_QtV1xpQLKKOdqENFuXXv4tv5lTwVETFGEIA$).

We recommend to create an *alias* in ```.bashrc``` for automatizing the login, i.e., ```alias server="ssh my_user@my_server_ip"```. Then for login type *server* and enjoy!

## Venv

First be sure that *python3* is already installed. If not, you can install it this way: 
```
sudo apt install python3
```
Finally, we can create the virtual environment:
```
python3 -m venv ~/.python_venv
```
feel free to change *the path* for something more suitable for you.

Edit your ```.bashrc``` on your server and include an alias for activating your *venv*: i.e., ```alias myenv="source ~/.virtual/bin/activate"```, change the folder structure with yours. For exiting the *venv*, just type ```deactivate```.

## Numba 

First check the [Nvidia website](https://developer.nvidia.com/how-to-cuda-python). Also, this [link](https://github.com/ContinuumIO/gtc2017-numba) is a good start point for Numba (Python + CUDA). We will be using Numba in Deep Learning Research.

Prerequisites before installing:
- (1) CUDA-capable GPUs
- (2) CUDA Toolkit installed

> Check (1) with this command on your server: ```lspci | grep -i nvidia```. \\
> Install CUDA Toolkit downloading [this](https://developer.nvidia.com/cuda-downloads) and following the next instruction (for linux) [here](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).
