# Base: L4T JetPack 6.x (r36), arm64
FROM nvcr.io/nvidia/l4t-base:r36.2.0

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends \
    git cmake build-essential \
    ca-certificates \
    libusb-1.0-0-dev libssl-dev \
    libudev-dev pkg-config \
    libglfw3-dev libgl1-mesa-dev libglu1-mesa-dev \
    udev \
    && rm -rf /var/lib/apt/lists/*

ARG RS_VERSION=2.55.1
RUN git clone --depth 1 --branch v${RS_VERSION} \
    https://github.com/IntelRealSense/librealsense.git /opt/librealsense && \
    cd /opt/librealsense && \
    # Instala regras udev para acesso à câmera via USB
    ./scripts/setup_udev_rules.sh || true && \
    mkdir build && cd build && \
    cmake .. \
      -DCMAKE_BUILD_TYPE=Release \
      -DBUILD_EXAMPLES=ON \
      -DBUILD_GRAPHICAL_EXAMPLES=ON \
      # RSUSB: backend user-space, sem necessidade de patch no kernel do host
      -DFORCE_RSUSB_BACKEND=true \
      # Habilita aceleração CUDA (GPU Ampere do Orin Nano)
      -DBUILD_WITH_CUDA=true \
    && make -j$(nproc) && make install && ldconfig && \
    rm -rf /opt/librealsense/build
