# Building OpenCV #

## Raspbian Stretch #

Start with a fresh Stretch build (e.g. 2018-11-13-raspbian-stretch-lite.img)

#### Increase swap space ####

```
sudo vi /etc/dphys-swapfile
```
Change CONF_SWAPSIZE to 1024

#### Restart swap service ####
```
sudo /etc/init.d/dphys-swapfile stop
sudo /etc/init.d/dphys-swapfile start
```

#### Install required packages ####
```
sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get install -y build-essential cmake pkg-config
sudo apt-get install -y libjpeg-dev libtiff5-dev libjasper-dev libpng12-dev
sudo apt-get install -y libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
sudo apt-get install -y libxvidcore-dev libx264-dev
sudo apt-get install -y libgtk2.0-dev libgtk-3-dev
sudo apt-get install -y libatlas-base-dev gfortran
sudo apt-get install -y python2.7-dev python3.5-dev python-numpy python3-numpy
sudo apt-get install -y git

mkdir Downloads
```

#### Download and build checkinstall as pre-built version is broken ####
```
cd ~/Downloads
git clone https://github.com/giuliomoro/checkinstall
cd checkinstall
sudo make install
```

#### Download and build OpenCV ####
```
cd ~/Downloads
wget https://github.com/opencv/opencv/archive/3.4.5.zip
unzip 3.4.5.zip

cd ~/Downloads/opencv-3.4.5
mkdir build_rpi3_release_fp_tbb
cd build_rpi3_release_fp_tbb
cmake -DCMAKE_CXX_FLAGS="-DTBB_USE_GCC_BUILTINS=1 -D__TBB_64BIT_ATOMICS=0" -DENABLE_VFPV3=ON -DENABLE_NEON=ON -DBUILD_TESTS=OFF -DWITH_TBB=ON -DCMAKE_BUILD_TYPE=Release ..

make -j 4
```

#### Build .deb ####
```
echo "opencv 3.4.5 build_rpi3_release_fp_tbb" > description-pak
echo | sudo checkinstall -D --install=no --pkgname=opencv --pkgversion=3.4.5 --provides=opencv --nodoc --backup=no --exclude=$HOME
```
