FROM nvidia/cuda:11.1-devel-ubuntu20.04

# update packages
RUN set -x && \
    apt update && \
    apt upgrade -y

# install command
RUN set -x && DEBIAN_FRONTEND=noninteractive \
    apt install -y --no-install-recommends wget sudo vim git curl python3 python3-pip python3-tk
RUN curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
RUN apt update && apt install -y --no-install-recommends nodejs
RUN apt upgrade -y
RUN apt autoremove -y  && \
    apt clean && \
    rm -rf \
     /var/lib/apt/lists/* \
     /var/cache/apt/* \
     /usr/local/src/* \
     /tmp/*

RUN pip3 install --upgrade pip && \
    pip3 install --no-cache-dir numpy matplotlib jupyter jupyterlab notebook jupyterlab_vim && \
    pip3 install --no-cache-dir torch==1.8.0+cu111 torchvision==0.9.0+cu111 torchaudio==0.8.0 -f https://download.pytorch.org/whl/torch_stable.html && \
    rm -rf ~/.cache/pip

RUN jupyter serverextension enable --py jupyterlab

ENV LIBRARY_PATH /usr/local/cuda/lib64/stubs

# oepn port
EXPOSE 8888

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
