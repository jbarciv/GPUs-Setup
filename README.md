# GPUs-Setup

## Description

The next respository summarizes the basics steps to run a small cluster of GPUs. Basic tips and many links are provided to facilitate research.

## Table of Contents

- [Server](#server)
- [Virtual environment](#venv)
- [TensorFlow 2 and Keras](#tensorflow2)
- [GPU Drivers](#gpus)

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

## TensorFlow2
When you install TensorFlow 2.0+,  Keras will be automatically installed, as well.

**Note:** TensorFlow and Keras require Python 3.6+ (Python 3.8 requires TensorFlow 2.2+) , and the latest version of pip.

To install TensorFlow for CPU and GPU processors, run the following command:
```
pip install tensorflow
```

## GPUs 

If you’re fine with using the CPU to train your neural network, your installation is done. If you want to use your GPU to the training, you’ll need to do the following:

- For Nvidia GPUs:
  - Ensure you’re running a **CUDA®-enabled card**
  - Install v11 or later of the **CUDA® Toolkit**
  - If you’re working with Deep Neural Networks, you’ll should also install the latest version of the **cuDNN library**

We will follow the installation guide [here](https://docs.nvidia.com/cuda/cuda-quick-start-guide/index.html) and also [here](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/)

### Prerequisites
#### CUDA-capable GPUs
Use this command:
```
lspci | grep -i nvidia
```
#### Driver Installation
#### CUDA Toolkit installed
