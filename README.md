# Ubuntu 20.04 setup

### Network set

```bash
$ ip addr # check ethernet name
$ sudo gedit /etc/netplan/01-network-manager-all.yaml

network:
  ethernets:
    en01:
      addresses: [143.248.143.22/24]
      gateway4: 143.248.143.1
      nameservers:
        addresses: [143.248.1.177, 143.248.2.177]
  version: 2
  renderer: NetworkManager

$ sudo netplan apply
```

### 한글 세팅

- settings
    - Region & Language
        - Manage Installed languages
            - The language support is not installed completely : install
            - Install / Remove Languages
                - Korean installed check
                - sudo reboot
        - Input Sources ‘+’ click
            - Korean
                - Korean (Hangul)
                - setting & reboot
            - delete English

### 그래픽 드라이버 설치

```bash
# ubuntu reboot & no GUI login
# Ctrl + Alt + F2~F7 & CLI login
$ sudo update-pciids
$ lspci | grep -i nvidia
$ sudo apt-add-repository ppa:graphics-drivers/ppa
$ sudo apt update

$ apt-cache search nvidia | grep nvidia-driver-
$ sudo apt-get install nvidia-driver-*** # *** is version
$ sudo reboot
$ cat /proc/driver/nvidia/version
$ nvidia-smi

```

### Apt 저장소 미러 변경

```bash
$ sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup
$ sudo gedit /etc/apt/sources.list

Ctrl + H
replace 'kr.archive.ubuntu.com' to 'mirror.kakao.com'
replace 'security.ubuntu.com' to 'mirror.kakao.com'
```

### Terminator

```bash
$ sudo apt install terminator
$ ibus-setup

## 수직분할 안될 때
'Emoji' tab click
'Emoji annotation' <Control><Shift>e 삭제

## 무한 스크롤 및 투명도같은 설정
오른쪽 버튼 후, Preferences
Profiles & Background 에서 투명도 설정 (0.75)
Profiles & Scrolling 에서 무한 스크롤 체크
```

### VS code

```bash
https://code.visualstudio.com/ 들어가서 deb 파일 다운
$ cd Download
$ sudo apt install ./[설치파일].deb
$ code .
```

### gedit plugin

```jsx
$ sudo apt install gedit-plugins -y

gedit 에서 preferences/plugin 들어가기.
code comment 체크하고 gedit 다시 켜면 주석 사용가능.

[ctrl + m]: comment
[ctrl + shift + m]: uncomment
```

### Chrome 설치

```jsx
$ sudo apt update
$ sudo apt install wget -y
$ wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
$ sudo dpkg -i ./google-chrome-stable_current_amd64.deb
$ rm -rf google-chrome-stable_current_amd64.deb
```

### Wifi setup (laptop)

```jsx
$ sudo apt install net-tools
$ ifconfig # check lancard
$ sudo apt dist-upgrade # update kernel
$ 
```

### Ceres solver (1.14.x)

```jsx
$ git clone https://ceres-solver.googlesource.com/ceres-solver

# CMake
sudo apt-get install cmake
# google-glog + gflags
sudo apt-get install libgoogle-glog-dev libgflags-dev
# Use ATLAS for BLAS & LAPACK
sudo apt-get install libatlas-base-dev
# Eigen3
sudo apt-get install libeigen3-dev
# SuiteSparse (optional)
sudo apt-get install libsuitesparse-dev

$ cd ceres-solver
$ git checkout 1.14.0
$ cd ../ & mkdir ceres-bin
$ cd ceres-bin
$ cmake ../ceres-solver
$ make -j4
$ sudo make install
```

### CMAKE update

```jsx
# https://cmake.org/download/ 
cmake-3.25.0.tar.gz -> copy link address

$ tar -xvf cmake-3.25.0.tar.gz
$ cd cmake-3.25.0
$ ./bootstrap
$ make
$ sudo make install
$ cmake --version # new terminal or reboot

./bootstrap 과정에서의 에러
# Could NOT find OpenSSL, try to set the path to OpenSSL root folder in the system variable OPENSSL_ROOT_DIR
1. openssl 설치
2. sudo apt install libssl-dev
```

### COLMAP

```jsx
$ git clone https://github.com/colmap/colmap.git
$ cd colmap
$ git checkout dev
$ mkdir build
$ cd build
$ cmake ..
$ make -j4  # gcc under 8 !!!!
$ sudo make install
```

### git ssh

```jsx
$ cd ~/.ssh # or mkdir ~/.ssh
$ ssh-keygen -t rsa -b 4096 -C "dklee@kaist.ac.kr"
# enter * 4
$ gedit id_rsa.pub
# 모두 복사
# github or gitlab setting -> ssh 등록
# 복사한 키 붙여넣기
```

### GCC version change

```jsx
$ sudo apt install build-essential
$ sudo apt -y install gcc-7 g++-7 gcc-8 g++-8 gcc-9 g++-9

$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 7
$ sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-7 7
$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 8
$ sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-8 8
$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 9
$ sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-9 9

$ sudo update-alternatives --config gcc
# select number

$ gcc --version
$ g++ --version
```

### 윈도우, ubuntu 듀얼부팅 시간 다른 문제 해결