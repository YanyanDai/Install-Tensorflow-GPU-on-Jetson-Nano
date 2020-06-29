# Install-Tensorflow-GPU-on-Jetson-Nano

1. Install Dependency
$ sudo apt-get install libhdf5-serial-dev hdf5-tools
$ sudo apt-get install python3-pip
$ pip3 install -U pip
$ sudo apt-get install zlib1g-dev zip libjpeg8-dev libhdf5-dev 
$ sudo python3 -m pip install -U numpy grpcio absl-py py-cpuinfo psutil portpicker grpcio six mock requests gast h5py astor termcolor

2. Install TensorFlow
pip3 install --pre --extra-index-url https://developer.download.nvidia.com/compute/redist/jp/v42 tensorflow-gpu

If you want to install special TensorFlow version, use the following sentence:
$ pip3 install --extra-index-url https://developer.download.nvidia.com/compute/redist/jp/v42 tensorflow-gpu==$TF_VERSION+nv$NV_VERSION

ex) $ pip3 install --extra-index-url https://developer.download.nvidia.com/compute/redist/jp/v42 tensorflow-gpu==1.12.0+nv19.1

3. Test
$ pythons 3
>>> import tensor Flow

4. Trouble Shooting:
error:
2020-06-19 15:04:16.956852: W tensorflow/stream_executor/platform/default/dso_loader.cc:55] Could not load dynamic library 'libcudart.so.10.0'; dlerror: libcudart.so.10.0: cannot open shared object file: No such file or directory; LD_LIBRARY_PATH: /usr/local/lib:/opt/ros/melodic/lib


solution:
step 1) copy the CUDA 10.2 folder to the name CUDA10.0
sudo cp -r /usr/local/cuda-10.2 /usr/local/cuda-10.0

Step 2) Locate the file lib*.so.10.2* in the LD_LIBRARY_PATH path and create a symbolic link named lib*.so.10.0.
nvidia@nvidia-desktop:~$ sudo ln -s /usr/local/cuda-10.0/lib64/libcudart.so.10.2.89 /usr/local/cuda-10.0/lib64/libcudart.so.10.0

nvidia@nvidia-desktop:~$ sudo ln -s /usr/local/cuda-10.0/lib64/libcufft.so.10.1.2.89 /usr/local/cuda-10.0/lib64/libcufft.so.10.0

nvidia@nvidia-desktop:~$ sudo ln -s /usr/local/cuda-10.0/lib64/libcurand.so.10.1.2.89 /usr/local/cuda-10.0/lib64/libcurand.so.10.0

nvidia@nvidia-desktop:~$ sudo ln -s /usr/local/cuda-10.0/lib64/libcusolver.so.10.3.0.89 /usr/local/cuda-10.0/lib64/libcusolver.so.10.0

nvidia@nvidia-desktop:~$ sudo ln -s /usr/local/cuda-10.0/lib64/libcusparse.so.10.3.1.89 /usr/local/cuda-10.0/lib64/libcusparse.so.10.0

nvidia@nvidia-desktop:~$ sudo ln -s /usr/lib/aarch64-linux-gnu/libcublas.so.10.2.2.89 /usr/local/cuda-10.0/lib64/libcublas.so.10.0

Step 3) Revise PATH, LD_LIBRARY_PATH
gedit ~/.bashrc

add the following sentence:
export PATH=/usr/local/cuda-10.0/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-10.0/extras/CUPTI/lib64:$LD_LIBRARY_PATH

source ~/.bashrc

Step 4) Test
$ python3
>>>import tensorflow

2020-06-29 12:43:52.144355: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcudart.so.10.0
