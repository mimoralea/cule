FROM nvidia/cuda:11.7.1-devel-ubuntu22.04

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=US

RUN apt-get -y update -qq && \
    apt-get install -y --no-install-recommends \
	software-properties-common \
        build-essential \
        ca-certificates \
        clang \
        ffmpeg \
	curl \
        unrar \
	wget \
	vim \
        gcc-9 \
        g++-9 \
        cmake \
        htop \
        git \
        libomp-dev \
        libsm6 \
        libssl-dev \
        libxrender-dev \
        libxext-dev \
        libxext6 \
	libz-dev \
        zlib1g-dev \
        zlib1g \
        unzip \
        libglu1-mesa-dev \
	libgl1-mesa-dev \
	libosmesa6-dev \
	xvfb \
	patchelf \
	ffmpeg \
	cmake \
	swig \
        iproute2 \
        ssh \
        zip \
    && \
    add-apt-repository -y ppa:deadsnakes/ppa && \
    apt-get -y update -qq && \
    apt install -y --no-install-recommends \
        python3.9 \
        python3.9-dev \
        python3.9-distutils \
        python3.9-venv \
    && \
    rm -rf /var/lib/apt/lists/*

RUN curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py && \
    python3.9 get-pip.py && rm get-pip.py && \
    rm /usr/local/bin/pip && rm /usr/local/bin/pip3

RUN update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 1 && \
    update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-9 1 && \
    update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.9 1 && \
    update-alternatives --install /usr/bin/python python /usr/bin/python3 1 && \
    update-alternatives --install /usr/local/bin/pip3 pip3 /usr/local/bin/pip3.9 1 && \
    update-alternatives --install /usr/local/bin/pip pip /usr/local/bin/pip3 1

ENV VIRTUAL_ENV=/opt/venv
RUN python -m venv $VIRTUAL_ENV

ENV LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/cuda/lib64"
ENV PATH="$VIRTUAL_ENV/bin:$PATH"

RUN pip install --upgrade "pip<=22.0.4" \
                          "setuptools<=59.8.0" \
                          "wheel<=0.37.1"

RUN pip install --upgrade "gym[atari,accept-rom-license]==0.21.0" \
                          "atari_py" \
                          "opencv-python<=3.4.17.63" \
                          "numpy<=1.21.5"

RUN wget -nv http://www.atarimania.com/roms/Roms.rar && \
    unrar x -o+ Roms.rar /tmp && \
    python -m atari_py.import_roms /tmp/ROMS/ && \
    python -m atari_py.import_roms /tmp/HC\ ROMS/ && \
    rm -rf /tmp/*

RUN pip install --upgrade "torch>2." \
                          torchvision \
                          torchaudio

RUN pip install --upgrade cython \
                          psutil \
                          pytz \
                          tensorboard \
                          tensorboardX \
                          tqdm

# RUN git clone -b master --recursive https://github.com/mimoralea/cule
RUN git clone -b master --recursive https://github.com/mimoralea/cule && \
    pip install -e cule
