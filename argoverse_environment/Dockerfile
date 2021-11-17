FROM nvidia/cuda:11.1-devel-ubuntu20.04
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# update packages and install commands
RUN apt update && DEBIAN_FRONTEND=noninteractive \
    apt install -y --no-install-recommends \
    wget sudo vim git curl python3 python3-pip python3-tk ansible gnupg lsb-release build-essential libssl-dev libffi-dev python3-dev
RUN curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
RUN apt update && apt install -y --no-install-recommends nodejs
RUN apt upgrade -y
RUN apt clean && \
    rm -rf /var/lib/apt/lists/*

# install ros
RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key  -o /usr/share/keyrings/ros-archive-keyring.gpg
RUN echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
RUN apt update && DEBIAN_FRONTEND=noninteractive apt install -y --no-install-recommends ros-galactic-desktop
RUN DEBIAN_FRONTEND=noninteractive apt install -y --no-install-recommends ros-galactic-desktop ros-galactic-lanelet2
RUN apt clean && \
    rm -rf /var/lib/apt/lists/*

# install pytorch and argoverse
RUN pip3 install --upgrade pip
RUN pip3 install --no-cache-dir numpy matplotlib scipy tqdm sklearn pandas jupyter jupyterlab notebook jupyterlab_vim pyparsing==2.4.7
RUN pip3 install --no-cache-dir torch==1.8.0+cu111 torchvision==0.9.0+cu111 torchaudio==0.8.0 -f https://download.pytorch.org/whl/torch_stable.html
RUN pip3 install torch-scatter -f https://data.pyg.org/whl/torch-1.8.0+cu111.html && \
    pip3 install torch-scatter torch-sparse torch-cluster torch-spline-conv torch-geometric -f https://data.pyg.org/whl/torch-1.8.0+cu111.html
RUN git clone https://github.com/argoai/argoverse-api.git
RUN pip3 install -e /argoverse-api && rm -rf ~/cache/pip /argoverse_api/
RUN rm -rf ~/.cache/pip /src/

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

RUN echo "source /opt/ros/galactic/setup.bash" >> ~/.bashrc
COPY  entrypoint.sh /
ENTRYPOINT [ "/entrypoint.sh" ]
CMD ["/bin/bash"]
