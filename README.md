# realSensel515

## Setup Jetson Orin Nano

```bash
docker build -t realsense-orin .
```
```bash
docker run -it --rm \
--privileged \
--network host \
-v /dev:/dev \
-v /run/udev:/run/udev \
realsense-orin
```

### UDEV Rules
```bash
cd ~/librealsense
sudo cp config/99-realsense-libusb.rules /etc/udev/rules.d/
sudo udevadm control --reload-rules
sudo udevadm trigger
```
```bash
sudo usermod -aG video $USER
```
