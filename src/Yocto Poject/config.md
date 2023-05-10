# Configuration Files
This files have the **.conf** extention, here is difine the configuration variables that control the build process of the project. 
With this kind of files we can configurate diferent areas like machine configuration, distribution configuration, possible compiler tuning, general common configuration, and user configuration.  
The main file is **bitbake.conf**, which you can find inside of the source tree conf directory of Bitbake.
### bitbake.conf
This file is relate to the main build tool, Bitbake. This file is the first to be parsed and then the rest of configuration files.  
You can find it in: ```poky/meta/conf/bitbake.conf```.  
There are many diferents sections of which we can point out the following:
1. Standard target filesystem paths.  
This filesystem paths extensively used in different recipes, this section has different subsections. Don't worry you don't have to change these variables, but you should reference these while developing recipes.  
2. Architecture-dependent build variables 
Variables to define architecture-dependent metadata, this are prefixed with:
``` 
BUILD_
HOST_
TARGET_
SDK_
```  
3. Packages default variables.  
4. General work and output directories for the build system.  
5. Specific image creation and rootfs population information.  
6. Build flags and options.   
7. Download locations and utilities.
8. Including the rest of the config files.  
### local.conf
This configuration file contains all the local user configurations for your build enciroment, any variable set here averrides any variable set elsewhere.   
Edit this file to set the ```MACHINE```, which package types you wish to use ```PACKAGE_CLASSES```, and the location from which you want to access downloaded files ```DL_DIR```.
### bblayers.conf
Here are defined the layers, which are directory paths, use ```BBLAYERS``` variable to list the layers. 
### machine.conf
>Note: You won't fint any file with this name.  

Machine is a replacement for the name of your board which you will build the image, for example if you want to build for qemux86-64, your configuration name will be qemux86-64.conf aand you could find it at: ```poky/meta-yocto-bsp/conf/machine/```.  
All of the machine conbfigurations are done in this file.  
1. The top level header contains information in tags for documentation:  
```
#@TYPE: Machine
#@NAME: qemux86-64 machine
#@DESCRIPTION: 
```
2. The preferred xserver can be used:  
```
PREFERRED_PROVIODER_virtual/xserver ?=
XSERVER ?= "       \
                    \ 
                    \
                    "
```  
3. What extra recipes need to be buildt  
```
MACHINE_EXTRA_PRECOMMENDS = " "
```    
4. Set image dependencies:
```
EXTRA_IMAGEDEPENDS += "" 
```  
5. Architecture-specific configurastions
```
DEFAULTTUNE ?= ""
include conf/machine/include/tune-cortexa8.inc
```  
6. Type of filesystem images we want to create
```
IMAGE_FSTYPES += "tar.bz2 jffs2"
EXTRA_IMAGECMD_jffs2 = "-lnp"
```  
7. Serial debug console.
```
SERIAL_CONSOLE = "115200 ttyO0"
```  
8. Set our kernel options to see what recipe should be used for kernel compilation and the kernel version needed to built.
```
PREFERRED_PROVIDER_virtual/kernel ?= "linux-yocto"
PREFERRED_VERSION_linux-yocto ?= "3.14%"
```  
9. To create another kernel image 
```
KERNEL_IMAGETYPE = "uImage"
KERNEL_DEVICETREE = "am335x-bone.dtb am335x"
KERNEL_EXTRA_ARGS += ""
```  
10. Configure most of the aspects of the bootloaders
```
SPL_BINARY = ""
UBOOT_SUFFIX = ""
UBOOT_MACHINE = ""
UBOOT_ENTRYPOINT = ""
UBOOT_LOADADDRESS = ""
```  
11. Determine the features that we want our machine to support.  
```
MACHINE_FEATURES = ""
```  
