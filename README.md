# cerbero-cross-compile
cerbero cross compile


sudo docker run -it ubuntu:18.04 /bin/bash

apt update && apt install openssh-server sudo -y
useradd -rm -d /home/ubuntu -s /bin/bash -g root -G sudo -u 1000 ubuntu
echo 'ubuntu:password' | chpasswd

cd ~/
apt -y install -y curl git
curl https://pyenv.run | bash


apt install -y unzip


//// 1.20.4
wget https://gitlab.freedesktop.org/gstreamer/cerbero/-/archive/1.20.4/cerbero-1.20.4.zip
unzip cerbero-1.20.4.zip
cd cerbero-1.20.4
/////////////////
///	build for linux x86
///////////////////
./cerbero-uninstalled bootstrap
./cerbero-uninstalled package gstreamer-1.0

/////////////////
///	build for linux arm
///////////////////
source simple_setup.sh
cp config/cross-lin-arm.cbc config/cross-lin-arm-ability.cbc
nano config/cross-lin-arm-ability.cbc
toolchain_prefix='/opt/arm/arm-ca53-linux-gnueabihf-6.4/'
tools_prefix="arm-ca53-linux-gnueabihf-"

export LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libgettextsrc-0.19.8.1.so
export CPATH="/home/ubuntu/cerbero-1.20.4/build/dist/linux_armv7/include/"
export LIBRARY_PATH="/home/ubuntu/cerbero-1.20.4/build/dist/linux_armv7/lib/"

modify recipes/gst-plugins-base-1.0.recipe
'auto_features': 'disabled',
'videotestsrc': 'enabled',
'videoconvert': 'enabled',
'app': 'enabled',

wavpack(skip)

modify recipes/gst-plugins-good-1.0.recipe
'wavpack': 'disabled',
#self.enable_plugin('ximagesrc', 'capture', variant='x11')
#self.enable_plugin('pulseaudio', 'sys', variant='pulse', option='pulse')

./cerbero-uninstalled -c config/cross-lin-arm-ability.cbc bootstrap
./cerbero-uninstalled -c config/cross-lin-arm-ability.cbc package gstreamer-1.0






