# realSensel515

## Setup Jetson Orin Nano

```
docker build -t realsense-orin .

docker run -it --rm \
--privileged \
--network host \
-v /dev:/dev \
-v /run/udev:/run/udev \
realsense-orin

