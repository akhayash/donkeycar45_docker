# Use the official Ubuntu 22.04 LTS base image
# FROM ubuntu:18.04
FROM nvcr.io/nvidia/cuda:10.2-cudnn7-devel-ubuntu18.04

# ENV APT_OPTIONS="-o Acquire::Retries=10"
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/3bf863cc.pub && \
    apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub 

# Install System-Wide Dependencies
RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get install -y wget git 

WORKDIR /root

# Install conda
RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh && \
    chmod +x ./Miniconda3-latest-Linux-x86_64.sh && \
    bash ./Miniconda3-latest-Linux-x86_64.sh -b -p /opt/conda && \
    rm Miniconda3-latest-Linux-x86_64.sh

# Set conda path
ENV PATH="/opt/conda/bin:${PATH}"

# Install Donkeycar Python Code
RUN mkdir projects && \
    cd ~/projects && \
    git clone https://github.com/waveshare/donkeycar && \
    cd donkeycar && \
    git checkout master 

# Activate environment
ARG env_name=donkey
ENV CONDA_DEFAULT_ENV ${env_name}

RUN conda env create -f /root/projects/donkeycar/install/envs/ubuntu.yml && \
    echo "conda activate ${env_name}" >> ~/.bashrc 
   
ENV PATH /opt/conda/envs/${env_name}/bin:$PATH
ENV LD_LIBRARY_PATH /usr/local/cuda-10.2/compat/:$LD_LIBRARY_PATH

WORKDIR /root/projects/donkeycar

RUN pip install -e .[pc] && \
    # GPU enabled package can be found in conda repository. Waveshare wiki uses non gpu enabled tensorflow
    conda install tensorflow==1.13.1=gpu_py37hc158e3b_0    
    # conda install tensorflow==1.13.1

# Install Cuda version 10.2`sudo dpkg -i cuda-repo-ubuntu1804_10.0.130-1_amd64.deb`
# RUN apt-get install -y gnupg2 && \
    # apt-key adv --keyserver keyserver.ubuntu.com --recv-keys A4B469963BF863CC && \
    # wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-repo-ubuntu1804_10.0.130-1_amd64.deb && \
    # dpkg -i cuda-repo-ubuntu1804_10.0.130-1_amd64.deb && \
    # apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub && \
    # apt-get update -y && \
    # apt-get install -y cuda 

# Install PyGame for USB camera (Optional)
RUN apt-get install -y python-dev libsdl1.2-dev libsdl-image1.2-dev libsdl-mixer1.2-dev libsdl-ttf2.0-dev libsdl1.2-dev libsmpeg-dev python-numpy subversion libportmidi-dev ffmpeg libswscale-dev libavformat-dev libavcodec-dev libfreetype6-dev && \
    pip install pygame

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Remove to fix a problem to enable GPU
#RUN rm -rf \
#    /usr/lib/x86_64-linux-gnu/libcuda.so* \
#    /usr/lib/x86_64-linux-gnu/libnvcuvid.so* \
#    /usr/lib/x86_64-linux-gnu/libnvidia-*.so* 
    # /usr/local/cuda/compat/lib/*.515.65.01