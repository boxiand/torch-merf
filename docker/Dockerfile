ARG CUDA_VERSION=11.8.0
ARG OS_VERSION=22.04
ARG USER_ID=1000
# Define base image.
FROM dromni/nerfstudio:main
ARG CUDA_VERSION
ARG OS_VERSION
ARG USER_ID

# metainformation
# LABEL org.opencontainers.image.version = "0.1.18"
# LABEL org.opencontainers.image.source = "https://github.com/nerfstudio-project/nerfstudio"
# LABEL org.opencontainers.image.licenses = "Apache License 2.0"
# LABEL org.opencontainers.image.base.name="docker.io/library/nvidia/cuda:${CUDA_VERSION}-devel-ubuntu${OS_VERSION}"

# Variables used at build time.
## CUDA architectures, required by Colmap and tiny-cuda-nn.
## NOTE: All commonly used GPU architectures are included and supported here. To speedup the image build process remove all architectures but the one of your explicit GPU. Find details here: https://developer.nvidia.com/cuda-gpus (8.6 translates to 86 in the line below) or in the docs.
ARG CUDA_ARCHITECTURES=90;89;86;80;75;70;61;52;37

# Set environment variables.
## Set non-interactive to prevent asking for user inputs blocking image creation.
ENV DEBIAN_FRONTEND=noninteractive
## Set timezone as it is required by some packages.
ENV TZ=Europe/Berlin
## CUDA Home, required to find CUDA in some packages.
ENV CUDA_HOME="/usr/local/cuda"

# install torch-merf
RUN cd /home/user && \
    git clone https://github.com/boxiand/torch-merf && \
    python3 -m pip install -r torch-merf/requirements.txt && \
    python3 -m pip install torch-merf/raymarching

RUN cd /home/user && \
    python3 -m pip install torch-merf/gridencoder && \
    python3 -m pip install torch-merf/freqencoder && \
    python3 -m pip install torch-merf/shencoder

# install zipnerf-pytorch
RUN cd /home/user && \
    git clone https://github.com/boxiand/zipnerf-pytorch.git &&\
    pip install -r requirements.txt &&\
    pip install ./extensions/cuda && \
    git clone https://github.com/NVlabs/nvdiffrast && \
    pip install ./nvdiffrast && \
    export CUDA=cu117; pip install torch-scatter -f https://data.pyg.org/whl/torch-2.0.0+${CUDA}.html
    
# Bash as default entrypoint.
CMD /bin/bash -l
