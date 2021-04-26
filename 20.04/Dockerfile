FROM ubuntu:20.04
ARG TENSORFLOW_TYPE=tensorflow

MAINTAINER hao jiang (hao.jiang@corerain.com)
ENV DEBIAN_FRONTEND noninteractive
ENV DEBCONF_NOWARNINGS yes
ARG MAKE_THREADS=4

RUN apt-get update -y \
    && apt-get install -y --no-install-recommends \
    autoconf \
    automake \
    build-essential \
    curl \
    ctags \
    ca-certificates \
    cmake \
    dirmngr \
    freeglut3-dev \
    gnupg2 \
    gpg-agent \
    graphviz \
    gawk \
    git \
    gcc-7 \
    g++-7 \
    id-utils \
    libssl-dev \
    libpq-dev \
    libtool \
    libglib2.0-0 \
    libsm6 \
    libxext6 \
    libxrender1 \
    libgtk-3-dev \
    libcairo2-dev \
    libmtdev-dev \
    libxkbcommon-x11-0 \
    libglu1-mesa-dev \
    libatlas-base-dev \
    libboost-all-dev \
    libhdf5-serial-dev \
    libleveldb-dev \
    liblmdb-dev \
    libsnappy-dev \
    mesa-common-dev \
    net-tools \
    openssh-server \
    pkg-config\
    python3 \
    python3-dev \
    python3-pip \
    sudo \
    software-properties-common \
    unzip \
    virtualenv \
    vim \
    wget \
    xauth \
    xclip \
    zip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
           /tmp/* \
           /var/tmp/* \
           /usr/share/doc/*

RUN echo "deb http://downloads.skewed.de/apt focal main" >> /etc/apt/sources.list \
    && apt-key adv --keyserver keys.openpgp.org --recv-key 612DEFB798507F25 \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
    python3-graph-tool \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
           /tmp/* \
           /var/tmp/* \
           /usr/share/doc/*

# Change gcc and g++ to 7
RUN rm /usr/bin/gcc \
    && rm /usr/bin/g++ \
    && ln -s /usr/bin/gcc-7 /usr/bin/gcc \
    && ln -s /usr/bin/g++-7 /usr/bin/g++

RUN mkdir -p /workspace/tmp

# Install OpenCV for C and C++
RUN cd /workspace/tmp/ \
    && wget -O opencv-3.4.zip \
    https://codeload.github.com/opencv/opencv/zip/3.4 \
    && unzip opencv-3.4.zip \
    && cd opencv-3.4 \
    && mkdir -p build \
    && cd build \
    && cmake .. \
    -DCMAKE_BUILD_TYPE=release \
    -DCMAKE_INSTALL_PREFIX=/usr/local/ \
    -DBUILD_PROTOBUF=OFF \
    -DBUILD_opencv_dnn=OFF \
    -DBUILD_TESTS=OFF \
    -DBUILD_PERF_TESTS=OFF \
    -DBUILD_EXAMPLES=OFF \
    -DBUILD_opencv_apps=OFF \
    -DWITH_CUBLAS=OFF \
    -DWITH_CUDA=OFF \
    -DWITH_CUFFT=OFF \
    -DWITH_OPENCL=OFF \
    -DWITH_OPENCLAMDBLAS=OFF \
    -DWITH_OPENCLAMDFFT=OFF \
    && make -j$MAKE_THREADS \
    && make install \
    && ldconfig \
    && rm -rf /workspace/tmp/*

# Install protobuf
RUN cd /workspace/tmp/ \
    && wget -O protobuf-3.5.1.1.zip \
    https://codeload.github.com/protocolbuffers/protobuf/zip/3.5.1.1 \
    && unzip protobuf-3.5.1.1.zip \
    && cd protobuf-3.5.1.1 \
    && ./autogen.sh \
    && ./configure \
    && make -j$MAKE_THREADS \
    && make install \
    && ldconfig \
    && rm -rf /workspace/tmp/*

# Install gtest
RUN cd /workspace/tmp/ \
    && wget -O googletest-release-1.8.0.zip \
    https://codeload.github.com/google/googletest/zip/release-1.8.0 \
    && unzip googletest-release-1.8.0.zip \
    && cd googletest-release-1.8.0 \
    && mkdir -p build \
    && cd build \
    && cmake .. \
    -G"Unix Makefiles" \
    -DCMAKE_BUILD_TYPE=release \
    -DCMAKE_INSTALL_PREFIX=/usr/local/ \
    -DBUILD_SHARED_LIBS=true \
    -DBUILD_STATIC_LIBS=true \
    && make -j$MAKE_THREADS \
    && make install \
    && ldconfig \
    && rm -rf /workspace/tmp/*

# Install gflags
RUN cd /workspace/tmp/ \
    && wget -O gflags-2.2.2.zip \
    https://codeload.github.com/gflags/gflags/zip/v2.2.2 \
    && unzip gflags-2.2.2.zip \
    && cd gflags-2.2.2 \
    && mkdir -p build \
    && cd build \
    && cmake .. \
    -DCMAKE_BUILD_TYPE=release \
    -DCMAKE_INSTALL_PREFIX=/usr/local/ \
    -DBUILD_SHARED_LIBS=true \
    -DBUILD_STATIC_LIBS=true \
    && make -j$MAKE_THREADS \
    && make install \
    && ldconfig \
    && rm -rf /workspace/tmp/*

# Install glog
RUN cd /workspace/tmp/ \
    && wget -O glog-0.3.4.zip \
    https://codeload.github.com/google/glog/zip/v0.3.4 \
    && unzip glog-0.3.4.zip \
    && cd glog-0.3.4 \
    && ./configure \
    && make -j$MAKE_THREADS \
    && make install \
    && ldconfig \
    && rm -rf /workspace/tmp/*

# Install Caffe
RUN cd /workspace/tmp/ \
    && git clone -b 1.0 --depth 1 https://github.com/BVLC/caffe.git \
    && cd caffe \
    && mkdir -p build \
    && cd build \
    && cmake .. \
    -DCMAKE_INSTALL_PREFIX=/usr/ \
    -DCPU_ONLY=1 \
    -DBUILD_python=0 \
    && make -j$MAKE_THREADS \
    && make install \
    && ldconfig \
    && rm -rf /workspace/tmp/*

RUN python3 -m pip install --upgrade pip \
    && python3 -m pip install --no-cache-dir "setuptools==42.0.2" \
    && python3 -m pip install --no-cache-dir \
    "numpy==1.16.4" \
    "matplotlib==3.1.2" \
    "opencv-python==4.1.2.30" \
    "click==6.7" \
    "h5py==2.10.0" \
    "Jinja2==2.10.3" \
    "pandas==0.25.3" \
    "Pillow==6.2.1" \
    "scipy==1.4.1" \
    "pydot==1.4.1" \
    "antlr4-python3-runtime==4.5" \
    "Cython==0.29.14" \
    "decorator==4.4.0" \
    "typing==3.7.4" \
    "typing_extensions==3.7.4" \
    "networkx==2.3" \
    "onnx==1.5.0" \
    "tqdm==4.32.2" \
    "pycairo==1.16.2" \
    "pyparsing==2.4.5" \
    "flask==1.1.1" \
    "flask-classful==0.14.2" \
    "graphviz==0.13.2" \
    "SgGo==0.5.3" \
    "pycryptodome==3.9.7" \
    "psutil==5.7.0" \
    "pyinstaller==3.6" \
    $TENSORFLOW_TYPE"==1.14.0"

RUN PYTHON_V=$(python3 -c 'import sys; print(str(sys.version_info[0])+"."+ \
                                             str(sys.version_info[1]))') \
    && echo "export LC_ALL=C.UTF-8" >> ~/.bashrc \
    && echo "export LANG=C.UTF-8" >> ~/.bashrc \
    && echo "export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib/python${PYTHON_V}/dist-packages/" >> ~/.bashrc \
    && echo rm /usr/bin/python || true \
    && echo rm /usr/bin/pip || true \
    && echo ln -s /usr/bin/python3 /usr/bin/python \
    && echo ln -s /usr/bin/pip3 /usr/bin/pip

