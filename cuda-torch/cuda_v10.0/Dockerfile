# Start with cuDNN base image
FROM nvidia/cuda:10.1-cudnn7-devel-ubuntu18.04
MAINTAINER Guilherme Gadelha <guilherme@copin.ufcg.edu.br>

# Install git, apt-add-repository and dependencies for iTorch
RUN apt-get update && apt-get install -y \
  git \
  software-properties-common \
  libssl-dev \
  libzmq3-dev \
  python-dev \
  python-pip \
  sudo

# Install Jupyter Notebook for iTorch
RUN pip install --upgrade pip
RUN pip install jupyter

# Run Torch7 installation scripts
RUN git clone https://github.com/nagadomi/distro.git /root/torch --recursive && \
    cd /root/torch && \
    bash install-deps && \
    TORCH_LUA_VERSION=LUA51 ./install.sh -b

# Export environment variables manually
ENV LUA_PATH='/root/.luarocks/share/lua/5.1/?.lua;/root/.luarocks/share/lua/5.1/?/init.lua;/root/torch/install/share/lua/5.1/?.lua;/root/torch/install/share/lua/5.1/?/init.lua;./?.lua;/root/torch/install/share/luajit-2.1.0-beta1/?.lua;/usr/local/share/lua/5.1/?.lua;/usr/local/share/lua/5.1/?/init.lua'
ENV LUA_CPATH='/root/.luarocks/lib/lua/5.1/?.so;/root/torch/install/lib/lua/5.1/?.so;./?.so;/usr/local/lib/lua/5.1/?.so;/usr/local/lib/lua/5.1/loadall.so'
ENV PATH=/root/torch/install/bin:$PATH
ENV LD_LIBRARY_PATH=/root/torch/install/lib:$LD_LIBRARY_PATH
ENV DYLD_LIBRARY_PATH=/root/torch/install/lib:$DYLD_LIBRARY_PATH
ENV LUA_CPATH='/root/torch/install/lib/?.so;'$LUA_CPATH

# Install iTorch
RUN luarocks install itorch

# Set ~/torch as working directory
WORKDIR /root/torch

# Install DFC-VAE dependencies
RUN sudo apt-get install libprotobuf-dev protobuf-compiler -y && \ 
    luarocks install loadcaffe && \
    luarocks install https://raw.githubusercontent.com/szym/display/master/display-scm-0.rockspec && \
    luarocks install nngraph && \
    sudo apt-get install libmatio2 -y && \
    luarocks install matio && \
    luarocks install manifold && \
    sudo apt-get install libatlas3-base -y

# Install NVIDIA-GPU dependencies
#RUN luarocks install cutorch && \
#    luarocks install cunn && \
#    luarocks install cudnn


# Download DFC-VAE code
#RUN cd ..
#RUN git clone https://github.com/houxianxu/DFC-VAE.git DFC-VAE
#RUN cd DFC-VAE/celebA

# Download dataset and annotations
#RUN sudo apt install wget -y
#RUN wget https://gist.githubusercontent.com/charlesreid1/4f3d676b33b95fce83af08e4ec261822/raw/4ec8b6b6f306a70fc229d01404ded90162f56a82/get_drive_file.py > get_drive_file.py
#RUN python3 get_drive_file.py 0B7EVK8r0v71pZjFTYXZWM3FlRnM img_align_celeba.zip
#RUN python3 get_drive_file.py 0B7EVK8r0v71pblRyaVFSWGxPY0U list_attr_celeba.txt

# Crop dataset images
#RUN unzip img_align_celeba.zip
#RUN cd ..
#RUN DATA_ROOT=celebA th data/crop_celebA.lua

# Download pretrained models
#RUN cd data/pretrained && bash download_models.sh && cd ../..

# Download encoder and decorder
#RUN mkdir ../checkpoints \
#    cd ../checkpoints \
#    python3 ../celebA/get_drive_files.py 0B2Nu6PGoZ7p4UDRpYUcxOW9Rd2s cvae_content_123_encoder.t7 \   
#    python3 ../celebA/get_drive_files.py 0B2Nu6PGoZ7p4ZHRMTnFrUXVIck0 cvae_content_123_decoder.t7
