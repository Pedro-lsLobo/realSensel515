FROM dustynv/ros:humble-desktop-l4t-r36.2.0

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    git \
    cmake \
    build-essential \
    libssl-dev \
    libusb-1.0-0-dev \
    libudev-dev \
    pkg-config \
    libgtk-3-dev \
    python3-dev \
    python3-pip \
    python3-colcon-common-extensions \
    python3-rosdep \
    curl \
    wget \
    unzip \
    v4l-utils \
    usbutils \
    && rm -rf /var/lib/apt/lists/*

RUN rosdep init || true
RUN rosdep update

WORKDIR /opt

RUN git clone https://github.com/IntelRealSense/librealsense.git

WORKDIR /opt/librealsense

RUN git checkout v2.54.2

RUN mkdir build && cd build && \
    cmake .. \
    -DFORCE_RSUSB_BACKEND=ON \
    -DBUILD_WITH_CUDA=true \
    -DBUILD_EXAMPLES=true \
    -DBUILD_GRAPHICAL_EXAMPLES=true \
    -DCMAKE_BUILD_TYPE=Release && \
    make -j4 && \
    make install

RUN apt-get update && apt-get install -y \
    ros-humble-cv-bridge \
    ros-humble-image-transport \
    ros-humble-diagnostic-updater \
    ros-humble-tf2-ros \
    ros-humble-rviz2

WORKDIR /root/ws/src

RUN git clone https://github.com/IntelRealSense/realsense-ros.git -b ros2-master

WORKDIR /root/ws

RUN /bin/bash -c "source /opt/ros/humble/setup.bash && \
    rosdep install -i --from-path src --rosdistro humble -y && \
    colcon build --symlink-install"

RUN echo 'source /opt/ros/humble/setup.bash' >> ~/.bashrc && \
    echo 'source /root/ws/install/setup.bash' >> ~/.bashrc

CMD ["/bin/bash"]
