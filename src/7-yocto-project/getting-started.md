# Getting Started
## Before build
#### 1. At least 90 Gbytes of free disk space.
#### 2. At least 8 Gbytes of RAM.
#### 3. Runs a supported Linux distribution ([Supported Linux Distrubutions](https://docs.yoctoproject.org/ref-manual/system-requirements.html#supported-linux-distributions)):
    - Fedora
    - openSUSE
    - CentOS
    - Debian
    - Ubuntu
#### 4. Install the next tools:
    - Git 1.8.3.1 or greater
    - tar 1.28 or greater
    - Python 3.8.0 or greater.
    - gcc 8.0 or greater.
    - GNU make 4.0 or greater
#### 5. Install QEMU emmulator:
    ```
    $ sudo apt update && upgrade
    $ sudo apt install libvirt-daemon
    $ sudo systemctl enable libvirtd
    $ sudo systemctl start libvirtd
    $ sudo apt install qemu-kvm 
    ```
#### 6. Build host packages:
    ``` $ sudo apt install gawk wget git diffstat unzip texinfo gcc build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev python3-subunit mesa-common-dev zstd liblz4-tool file locales ```
    ``` $ sudo locale-gen en_US.UTF-8 ```
## Install Poky:
Poky is the reference operating system distribution built with Yocto Project tools, and OpenEmbedded is a build framework of recipes and packages. 
The Yocto Project uses Poky to build images (kernel, system, and application software) for targeted hardware, and OpenEmbedded supports many hardware architectures with cross-compilation infrastructure. The community uses it to validate Yocto Project features and functionality, but it also serves as example for any user who builds their own custom distribution.

#### 1. Overview
To build poky we use the bitbake tool wich handles the parsing and execution of the following data files:
- Recipes: Provides details about particular pieces of software
- Class Data: How to build a Linux kernel
- Configuration Data: Machine-specific settings, policy decisions, etc. Acts a the glue to bind everithing together.
Bitbake knows how to combine multiple data sources together and refers to each data source as a layer.

#### 2. Download poky 
You can direct download the compress file from oficcial page or clone de code repositorie from git using the path specificted in the same page: [Download Poky](https://www.yoctoproject.org/software-overview/downloads/).

#### 3. First time running a build
1. Go to the poky folder:
    ```cd poky```
2. Fist you must set the enviromment using the following command:
    ``` $ source poky-init-build-env [build_dir]```  
    The build_dir is the dir containing all the build's object files. The default build dir is poky-dir/build. A different build_dir can be used for each of the targets. For example, ~/build/x86 for a qemux86 target, and ~/build/arm for a qemuarm target. Please refer to poky-init-build-env for more detailed information.   
3. Examine your local configuration file (conf/local.conf):
    For this example, the defaults are set to build for a qemux86 target, which is suitable for emulation. The package manager used is set to the RPM package manager.
4. Build the target using:
    ``` $ bitbake <target>```  
    The target is the name of the recipe you want to build. Common targets are the images in meta/recipes-core/images, /meta/recipes-sato/images, etc. Or, the target can be the name of a recipe for a specific piece of software such as busybox. For more details about the standard images available, see the 'Reference: Images' appendix.   
5. Emmulate the result with QEMU:
    ``` $ runquemu [option ] [...]```
    Once an image has been built it often needs to be installed. The images/kernels built by Poky are placed in the tmp/deploy/images directory. 
6. Exit QEMU:
    ``` Ctrl + C```
