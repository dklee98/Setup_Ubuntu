# Ubuntu 20.04 setup

1. [Network setup](#network-setup)
2. [Korean setup](#korean-setup)
3. [Nvidia graphic driver](#nvidia-graphic-driver)
4. [Change Apt-archive](#change-apt-archive)
5. [Update CMAKE](#update-cmake)
6. [Install Terminator](#install-terminator)
7. [Install VScode](#install-vscode)
8. [Install gedit plugin](#install-gedit-plugin)
9. [Install Chrome](#install-chrome)
10. [Install Ceres Solver](#install-ceres-solver-114x)
11. [Install COLMAP](#install-colmap)
12. [Change gcc/g++ version](#chane-gcc-version)
13. [Add ssh for git](#add-ssh-for-git)
14. [Window, Ubuntu different time](#window-ubuntu-differnet-time-dual-boot)

## Network setup

```bash
$ ip addr # check your ethernet name
$ sudo gedit /etc/netplan/01-network-manager-all.yaml

    # in .yaml file
    network:
      ethernets:
        en01: # your ethernet name
          addresses: [143.248.143.22/24]   # yours
          gateway4: 143.248.143.1          # yours
          nameservers:
            addresses: [143.248.1.177, 143.248.2.177] # yours
      version: 2
      renderer: NetworkManager

$ sudo netplan apply
```

## Korean setup

- go to ubuntu `Settings`
    - `Region & Language`
        - `Manage Installed languages`
            - The language support is not installed completely : `install`
            - `Install / Remove Languages`
                - `Korean` installed check
                - $ sudo reboot
        - Input Sources `+ button` click
            - delete English
            - `Korean`
                - `Korean (Hangul)`
                - setting (한글 변경 key 또는 right alt로 한/영 변경 설정)
                - $ sudo reboot

## Nvidia graphic driver

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

## Change `Apt archive`

```bash
$ sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup
$ sudo gedit /etc/apt/sources.list

Ctrl + H
replace 'kr.archive.ubuntu.com' to 'mirror.kakao.com'
replace 'security.ubuntu.com' to 'mirror.kakao.com'
# 더 빠른 경로
```

## Update CMAKE

```bash
# https://cmake.org/download/ 
# Download cmake-3.25.0.tar.gz or latest version

$ tar -xvf cmake-3.25.0.tar.gz
$ cd cmake-3.25.0
$ ./bootstrap
$ make
$ sudo make install
$ cmake --version # new terminal or reboot

## Error in './bootstrap' seqeunce
## error msg: Could NOT find OpenSSL, try to set the path to OpenSSL root folder in the system variable OPENSSL_ROOT_DIR
# solve
1. install 'openssl' 
2. $ sudo apt install libssl-dev
```

## Install Terminator

```bash
$ sudo apt install terminator

## Cannot use 'Control + Shift + e'
$ ibus-setup
  'Emoji' tab click
  'Emoji annotation' delete '<Control><Shift>e'

## Preference 
  Right mouse click & Preferences
  in 'Profiles & Background',
    check 'Transparent background' & move to bar '0.75'
  in 'Profiles & Scrolling',
    check 'infinite scroll'
```

## Install VScode

```bash
# Download .deb file in https://code.visualstudio.com/
$ cd Download
$ sudo apt install ./[install_file].deb
$ code .
```

## Install gedit plugin

```bash
$ sudo apt install gedit-plugins -y

  # Right button & 'preferences/plugin' in gedit
  # check 'code comment'
  # restart gedit

  # you can use comment in gedit
    [ctrl + m]: comment
    [ctrl + shift + m]: uncomment
```

## Install Chrome

```bash
$ sudo apt update
$ sudo apt install wget -y
$ wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
$ sudo dpkg -i ./google-chrome-stable_current_amd64.deb
$ rm -rf google-chrome-stable_current_amd64.deb
```

<!-- ### Wifi setup (laptop)

```bash
$ sudo apt install net-tools
$ ifconfig # check lancard
$ sudo apt dist-upgrade # update kernel
$ 
``` -->

## Install Ceres solver (1.14.x)

```bash
$ git clone https://ceres-solver.googlesource.com/ceres-solver

# CMake
$ sudo apt-get install cmake
# google-glog + gflags
$ sudo apt-get install libgoogle-glog-dev libgflags-dev
# Use ATLAS for BLAS & LAPACK
$ sudo apt-get install libatlas-base-dev
# Eigen3
$ sudo apt-get install libeigen3-dev
# SuiteSparse (optional)
$ sudo apt-get install libsuitesparse-dev

$ cd ceres-solver
$ git checkout 1.14.0
$ cd ../ & mkdir ceres-bin
$ cd ceres-bin
$ cmake ../ceres-solver
$ make -j4
$ sudo make install
```

## Install COLMAP

```bash
$ git clone https://github.com/colmap/colmap.git
$ cd colmap
$ git checkout dev
$ mkdir build
$ cd build
$ cmake ..
$ make -j4  # gcc under 8 !!!!
$ sudo make install
```

## Chane GCC Version

```bash
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

## Add ssh for git

```bash
$ cd ~/.ssh # or mkdir ~/.ssh
$ ssh-keygen -t rsa -b 4096 -C "dklee@kaist.ac.kr"
# enter * 4
$ gedit id_rsa.pub
# copy all
# github or gitlab setting -> ssh 
# paste all
```

## Window, ubuntu differnet time (dual boot)
```bash
$ timedatectl set-local-rtc 1 --adjust-system-clock
$ timedatectl # RTC in local TZ: yes
```