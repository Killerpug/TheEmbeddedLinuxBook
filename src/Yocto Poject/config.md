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
3. Packages default variables.  1
4. General work and output directories for the build system.  
5. Specific image creation and rootfs population information.  
6. Build flags and options.   
7. Download locations and utilities.
8. Including the rest of the config files.  
### local.conf
"Before you start your build process, you should open up the ```local.conf```."  
This configuration file contains all the local user configurations for your build enciroment, any variable set here averrides any variable set elsewhere.   
Edit this file to set the ```MACHINE```, which package types you wish to use ```PACKAGE_CLASSES```, and the location from which you want to access downloaded files ```DL_DIR```.
#### Most significant variables in ```local.conf```  
The file is well commented so you should understand almost all variables, then you will be shown some variables that are not specified in the initial document:   
1. INHERIT  
Causes the named class to be inherited globally, anonymous fuctions in this inherited class are not executed for the base configuration and in each individual recipes.  
Suppose you needed to inherit a class file called ```abc.bbclass``` form a configuration file as follows:    
```INHERIT += "abc"```  

2. BUILDHISTORY_COMMIT
This variable specifies whether or not to commit the build history output in a local Git repository, if set to "1" the local repository will be maintained automatically by the buildhistory class and commit will be created on every build for changes.  
```BUILDHISTORY_COMMIT ?= "1" ```   
With this the .bbclass file must be located in a ```classes``` subdirectory. 

3. SSTATE_DIR
In this variable you should define the directory for the shared state cache.
``` SSTATE_DIR = "$home/docs/exaple/sstate-cache"```    
4. SSTATE_MIRRORS  
Configure the system to search other mirror locations for prebuild cache data, this could be a filesystem directory or a remote URL susch HTTP or FTP. This locations need to contain the shared state cache ```SSTATE_CACHE``` results from previous builds.
If a mirror uses the same structure as SSTATE_DIR, you need to add ```PATH``` at the end as follow example:  
```
SSTATE_MIRRORS ?= "\
    file://.* https://someserver.tld/share/sstate/PATH;downloadfilename=PATH \
    file://.* file:///some-local-dir/sstate/PATH"
```  
5. DL_DIR  
In this variable you can set the central download directory by the build process to store downloads, this directory is self-maintaining and you should not have to touch it, by default the directory is downloads in the buiild directory. To change this you can specify in the variable as follows:  
```DL_DIR ?= "$home/exaple/topdir/downloads"```   
6. IMAGE_INSTALL  
In this variable you can specify the packages to install into an image through the image class. Image recipes set ```IMAGE_INSTALL``` to specify the packages to install into an image throung image, it's important that when you use this variable, use as follows:  
```IMAGE_INSTALL:append = " package-name"```  
>Note: When you define a package name afther the quotation marks you have to put a space. 
### bblayers.conf
Here are defined the layers, which are directory paths, use ```BBLAYERS``` variable to list the layers. This file tells Bitbake what layers you want considered during the build. The deault layers considered are the minimally needed by the build system.
To enable you layer, simply add your layer's path to the BBLAYERS bariable, the following example shows how to enable your new ```meta-example``` layer in the build image:

```
# POKY_BBLAYERS_CONF_VERSION is increased each time build/conf/bblayers.conf
# changes incompatibly
POKY_BBLAYERS_CONF_VERSION = "2"
BBPATH = "${TOPDIR}"
BBFILES ?= ""
BBLAYERS ?= " \
    /home/user/poky/meta \
    /home/user/poky/meta-poky \
    /home/user/poky/meta-yocto-bsp \
    /home/user/mystuff/meta-example \
    "

```  
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
