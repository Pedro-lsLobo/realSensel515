FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends \
    git cmake build-essential \
    ca-certificates \
    libusb-1.0-0-dev libssl-dev \
    libglfw3-dev libgl1-mesa-dev libglu1-mesa-dev \
    udev \
    && rm -rf /var/lib/apt/lists/*

ARG RS_VERSION=2.55.1
RUN git clone --depth 1 --branch v${RS_VERSION} \
    https://github.com/IntelRealSense/librealsense.git /opt/librealsense && \
    cd /opt/librealsense && \
    mkdir build && cd build && \
    cmake .. \
      -DCMAKE_BUILD_TYPE=Release \
      -DBUILD_EXAMPLES=ON \
      -DBUILD_GRAPHICAL_EXAMPLES=ON \
    && make -j$(nproc) && make install && ldconfig && \
    rm -rf /opt/librealsense/build