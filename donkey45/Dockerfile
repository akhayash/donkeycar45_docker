# Use the official Ubuntu 22.04 LTS base image
FROM ubuntu:18.04

# ENV APT_OPTIONS="-o Acquire::Retries=10"
ENV DEBIAN_FRONTEND=noninteractive

# Install System-Wide Dependencies
RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get install -y wget git && \
    apt-get install -y libhdf5-serial-dev hdf5-tools libhdf5-dev zlib1g-dev zip libjpeg8-dev liblapack-dev libblas-dev gfortran && \
    apt-get install -y python3-dev python3-pip && \
    apt-get install -y libxslt1-dev libxml2-dev libffi-dev libcurl4-openssl-dev libssl-dev libpng-dev libopenblas-dev && \
    apt-get install -y git nano && \
    apt-get install -y openmpi-doc openmpi-bin libopenmpi-dev libopenblas-dev && \
    apt-get install -y pkg-config libhdf5-dev && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /root

# Setup Virtual Environment
RUN pip3 install virtualenv && \
    python3 -m virtualenv -p python3.6 env45 --system-site-packages && \
    echo "source ~/env45/bin/activate" >> ~/.bashrc && \
    /bin/bash -c "source ~/.bashrc"

# Setup Python Dependencies
RUN pip3 install -U pip testresources setuptools && \
    pip3 install -U futures==3.1.1 protobuf==3.12.2 pybind11==2.5.0 && \
    pip3 install -U cython==0.29.21 pyserial && \
    pip3 install -U future==0.18.2 mock==4.0.2 h5py==2.10.0 keras_preprocessing==1.1.2 keras_applications==1.0.8 gast==0.3.3 && \
    pip3 install -U absl-py==0.9.0 py-cpuinfo==7.0.0 psutil==5.7.2 portpicker==1.3.1 six requests==2.24.0 astor==0.8.1 termcolor==1.1.0 wrapt==1.12.1 google-pasta==0.2.0 && \
    pip3 install -U pandas && \
    pip3 install -U gdown && \
    pip3 install -U azure-ai-ml azure-identity mlflow azure-mlflow mlflow-skinny && \

    apt-get  clean && \
    rm -rf /var/lib/apt/lists/*

# Install TensorFlow as a system package
RUN pip3 install --pre --extra-index-url https://developer.download.nvidia.com/compute/redist/jp/v45 tensorflow==2.3.1

# Install Cuda version 10.2
# apt-get purge remove nvidia-* && \
# apt-get purge remove cuda-* && \
RUN wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-ubuntu1804.pin && \
    mv cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600 && \
    wget https://developer.download.nvidia.com/compute/cuda/10.2/Prod/local_installers/cuda-repo-ubuntu1804-10-2-local-10.2.89-440.33.01_1.0-1_amd64.deb && \
    dpkg -i cuda-repo-ubuntu1804-10-2-local-10.2.89-440.33.01_1.0-1_amd64.deb && \
    apt-key add /var/cuda-repo-10-2-local-10.2.89-440.33.01/7fa2af80.pub && \
    apt-get update && \
    apt-get -y install cuda

# Install Donkeycar Python Code
RUN mkdir projects && \
    cd ~/projects && \
    git clone https://github.com/autorope/donkeycar && \
    cd donkeycar && \
    git fetch --all --tags -f && \
    git checkout 4.5.0 && \
    pip install -e .[nano]

# Install PyGame for USB camera (Optional)
RUN apt-get  install -y python-dev libsdl1.2-dev libsdl-image1.2-dev libsdl-mixer1.2-dev libsdl-ttf2.0-dev libsdl1.2-dev libsmpeg-dev python-numpy subversion libportmidi-dev ffmpeg libswscale-dev libavformat-dev libavcodec-dev libfreetype6-dev && \
    pip install pygame && \
    apt-get  clean && \
    rm -rf /var/lib/apt/lists/*
