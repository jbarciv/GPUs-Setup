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
**If you want to use olther versions of python:**

Use deadsnakes:
```
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt install python3.8
```

---------------------------------------------

First be sure that *python3* is already installed. If not, you can install it this way: 
```
sudo apt install python3
```
Secondly be sure that *pip* is already installed. If not, you can install it this way: 
```
sudo apt-get install python3-pip
```
Then install this also:
```
sudo apt install python3.10-venv
```
Finally, we can create the virtual environment:
```
python3 -m venv ~/.python_venv
```
feel free to change *the path* for something more suitable for you.

Edit your ```.bashrc``` on your server and include an alias for activating your *venv*: i.e., ```alias myenv="source ~/.python_venv/bin/activate"```, change the folder structure with yours. For exiting the *venv*, just type ```deactivate```.

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
1. Go to: [NVIDIA download drivers for Cuda 12](https://developer.nvidia.com/cuda-downloads) or [here for Cuda 11.2](https://developer.nvidia.com/cuda-11.2.0-download-archive).
2. Select the GPU and OS version from the drop-down menus.
3. Download and install the NVIDIA graphics driver as indicated on that web page. For more information, select the ADDITIONAL INFORMATION tab for step-by-step instructions for installing a driver.
4. Restart your system to ensure that the graphics driver takes effect.

**¡¡Please pay a lot attention of which version of Cuda you want/need to install... because TensorFlow of Pytorch sometimes do not go so fast as Cuda releases...!!**

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

##### Do not forget to add the *code samples* and the cuDNN library documentation:

Will be necesary to donwload the *.deb* file [here](https://developer.nvidia.com/rdp/cudnn-download) locally and then do a `scp from_local to_host@ip:my_folder` or directly use this in the server (for Cuda 12.x and Ubuntu 22.04):
```
wget https://developer.nvidia.com/downloads/compute/cudnn/secure/8.8.1/local_installers/12.0/cudnn-local-repo-ubuntu2204-8.8.1.3_1.0-1_amd64.deb/
```
or

```
wget https://developer.nvidia.com/downloads/compute/cudnn/secure/8.8.1/local_installers/11.8/cudnn-local-repo-ubuntu2004-8.8.1.3_1.0-1_amd64.deb
```

and do the following:
```
sudo dpkg -i cudnn-local-repo-${OS}-8.x.x.x_1.0-1_amd64.deb
sudo cp /var/cudnn-local-repo-*/cudnn-local-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get install libcudnn8-samples=8.8.1.*-1+cuda12.0
```
##### Verifying the Install on Linux
To verify that cuDNN is installed and is running properly, compile the mnistCUDNN sample located in the `/usr/src/cudnn_samples_v8` directory in the Debian file.
Copy the cuDNN samples to a writable path.
```
cp -r /usr/src/cudnn_samples_v8/ $HOME
```
Go to the writable path.
```
cd  $HOME/cudnn_samples_v8/mnistCUDNN
```
Compile the mnistCUDNN sample.
```
make clean && make
```
You maybe need to install this:
```
sudo apt-get install g++ freeglut3-dev build-essential libx11-dev libxmu-dev libxi-dev libglu1-mesa libglu1-mesa-dev
sudo apt-get install libfreeimage3 libfreeimage-dev
```
Finally, run the mnistCUDNN sample.
```
./mnistCUDNN
```
If cuDNN is properly installed and running on your Linux system, you will see a message similar to the following:
```
Test passed!
```

#### Some furthers checks
Let's check what is the output of:
```
nvcc --version
```
if nvcc is not "installed" don't worry... try this:
```
whereis nvcc
```
if the output is: `nvcc: ` and nothing else... the solution is adding this next two lines to the bottom of `.bashrc` file:
```
export PATH="/usr/local/cuda-12.1/bin:$PATH"
export LD_LIBRARY_PATH="/usr/local/cuda-12.1/lib64:$LD_LIBRARY_PATH"
```
(change cuda version if needed). Source `.bashrc` and try again `nvcc --version` you should see something like this:
```
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2023 NVIDIA Corporation
Built on Tue_Feb__7_19:32:13_PST_2023
Cuda compilation tools, release 12.1, V12.1.66
Build cuda_12.1.r12.1/compiler.32415258_0 
```
