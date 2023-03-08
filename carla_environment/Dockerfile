FROM nvidia/cuda:11.6.1-devel-ubuntu20.04

# update packages
RUN apt update && apt upgrade -y

# install command
RUN apt update && DEBIAN_FRONTEND=noninteractive \
    apt install -y --no-install-recommends \
    build-essential sed wget sudo vim git curl python3 python3-pip python3-tk ansible gnupg lsb-release build-essential libssl-dev libffi-dev python3-dev cmake libtool
RUN apt-get update ; \
    apt-get install -y wget software-properties-common && \
    add-apt-repository ppa:ubuntu-toolchain-r/test && \
    wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key|apt-key add - && \
    apt-add-repository "deb http://apt.llvm.org/xenial/ llvm-toolchain-xenial-8 main" && \
    apt-get update
RUN curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
RUN apt update && apt install -y --no-install-recommends nodejs
RUN apt upgrade -y
RUN apt clean && \
    rm -rf /var/lib/apt/lists/*

ENV LIBRARY_PATH /usr/local/cuda/lib64/stubs

RUN pip3 install --upgrade pip
RUN pip3 install --no-cache-dir numpy matplotlib scipy tqdm sklearn pandas pygame jupyter jupyterlab notebook jupyterlab_vim jupyter_contrib_nbextensions scikit-image IPython ipdb pyparsing==2.4.7
#RUN pip3 install --no-cache-dir torch==1.8.0+cu111 torchvision==0.9.0+cu111 torchaudio==0.8.0 -f https://download.pytorch.org/whl/torch_stable.html
RUN pip3 install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu116
RUN pip3 install torch-scatter torch-sparse torch-cluster torch-spline-conv torch-geometric -f https://data.pyg.org/whl/torch-1.8.0+cu111.html
RUN git clone https://github.com/purewater0901/argoverse-api.git
RUN pip3 install -e /argoverse-api
RUN rm -rf ~/.cache/pip /src/

RUN jupyter serverextension enable --py jupyterlab

# oepn port
EXPOSE 8888
EXPOSE 2000

# create user
ARG DOCKER_UID=1000
ARG DOCKER_USER=docker
ARG DOCKER_PASSWORD=docker
RUN useradd -m \
  --uid ${DOCKER_UID} --groups sudo ${DOCKER_USER} \
  && echo ${DOCKER_USER}:${DOCKER_PASSWORD} | chpasswd

USER ${DOCKER_USER}

# create data direcotry
WORKDIR /work/data
WORKDIR /work
