# GPUs-Setup

## Description

The next respository summarizes the basics steps to run a small cluster of GPUs. Basic tips and many links are provided to facilitate installation. The underlying intention is based on neuroevolutionary algorithms for research purposes.

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

We will follow the installation from the [Cuda installation guide for linux](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/).

### Prerequisites
#### CUDA-capable GPUs
Use this command:
```
lspci | grep -i nvidia
```
#### Verify You Have a Supported Version of Linux
```
uname -m && cat /etc/*release
```
#### Verify the System Has gcc Installed
```
gcc --version
```
#### Verify the System has the Correct Kernel Headers and Development Packages Installed
It is best to manually ensure the correct version of the kernel headers and development packages are installed prior to installing the CUDA Drivers, as well as whenever you change the kernel version.
```
uname -r
```
For Ubuntu, the kernel headers and development packages for the currently running kernel can be installed with:
```
sudo apt-get install linux-headers-$(uname -r)
```
### CUDA Drivers Installation & Cuda toolkit
1. Go to: [NVIDIA download drivers](https://developer.nvidia.com/cuda-downloads)
2. Select the GPU and OS version from the drop-down menus.
3. Download and install the NVIDIA graphics driver as indicated on that web page. For more information, select the ADDITIONAL INFORMATION tab for step-by-step instructions for installing a driver.
4. Restart your system to ensure that the graphics driver takes effect.

We recommend for a cluster to install `deb(local)` as the installer type. The instructions will be summarized in the [NVIDIA download drivers](https://developer.nvidia.com/cuda-downloads) but are also explained in depth and generically in the [Cuda installation guide for linux (Ubuntu)](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#ubuntu). 
##### Some notes about drivers and Cuda versions
From [this best practices guide](https://docs.nvidia.com/cuda/cuda-c-best-practices-guide/index.html#deploying-cuda-applications):
- Each version of the CUDA Toolkit (and runtime) requires a minimum version of the NVIDIA driver. Applications compiled against a CUDA Toolkit version will only run on systems with the specified minimum driver version for that toolkit version. See next figure for an example.
![alt text](https://docs.nvidia.com/cuda/cuda-c-best-practices-guide/_images/CTK-and-min-driver-versions.png)
- For convenience, the **NVIDIA driver is installed as part of the CUDA Toolkit installation**. Note that this driver is for development purposes and is not recommended for use in production with Tesla GPUs.
- For running CUDA applications in production with Tesla GPUs, it is recommended to download the latest driver for Tesla GPUs from the NVIDIA driver downloads site at https://www.nvidia.com/drivers.
-During the installation of the CUDA Toolkit, the installation of the NVIDIA driver may be skipped on Windows (when using the interactive or silent installation) or on Linux (by using meta packages).
- For meta packages on Linux, see https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#package-manager-metas.

#### Downloading cuDNN for Linux
See [here](https://docs.nvidia.com/deeplearning/cudnn/install-guide/index.html) for all the info.
##### Prerequisites: installing Zlib
For Ubuntu users, to install the zlib package, run:
```
sudo apt-get install zlib1g
```
##### Installation instructions
These are the installation instructions for Debian 11, Ubuntu 18.04, Ubuntu 20.04, and 22.04 users.
1. Enable the repository. The following commands enable the repository containing information about the appropriate cuDNN libraries online for Debian 11, Ubuntu 18.04, Ubuntu 20.04, and 22.04.
```
wget https://developer.download.nvidia.com/compute/cuda/repos/${OS}/x86_64/cuda-${OS}.pin
sudo mv cuda-${OS}.pin /etc/apt/preferences.d/cuda-repository-pin-600
sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/${OS}/x86_64/3bf863cc.pub
sudo add-apt-repository "deb https://developer.download.nvidia.com/compute/cuda/repos/${OS}/x86_64/ /"
sudo apt-get update
```
Where ${OS} is `debian11`, `ubuntu1804`, `ubuntu2004`, or `ubuntu2204`.

2. Install the cuDNN library:

Before you start, please note that these libraries may already be installed...
```
sudo apt-get install libcudnn8=${cudnn_version}-1+${cuda_version}
sudo apt-get install libcudnn8-dev=${cudnn_version}-1+${cuda_version}
```
Where:
${cudnn_version} is `8.8.1.*`

${cuda_version} is `cuda12.0` or `cuda11.8`
