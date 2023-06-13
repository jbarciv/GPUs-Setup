# Prerequisites for running the code in the cluster

## Basic installation
```
pip install matplotlib
pip install sk-video
pip install opencv-python
pip install gym
pip install gym[atari]
pip install gym[atari,accept-rom-license]==0.21.0
```
For gym, sometimes there are some "wheel" problems so use that (the "" are for the zsh):
```
pip install "wheel==0.38.4"
pip install "gym[atari,accept-rom-license]==0.21.0"      

``` 


also 
```
sudo apt-get install libatlas-base-dev gfortran
sudo apt install cmake libz-dev
```
