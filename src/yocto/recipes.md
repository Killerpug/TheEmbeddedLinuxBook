# Recipes
Official page: https://docs.yoctoproject.org/overview-manual/concepts.html#recipies
[//]:# ("This information for instructions can be resumed as:" is not concise, shorten it)
## Recipes files
Recipe files or also known as BB files, are files that contain information and instructions that build tool (bitbake) takes for generating the packages. This information for instructions can be resumed as:
1. Descriptive information about package
2. Section information
3. The recipe version
4. The Licience
5. Existing dependencies 
6. Where the source code resides and how to fetch it 
7. Whether the source code requires any patches, where to find them, and how to apply them.
8. How to configure and compile the source code.
9. Where on the target machine to install the package or packages created.
## Variables for bb files
[Variables](https://docs.yoctoproject.org/dev/ref-manual/variables.html).
[//]:# (If you put a link then maybe instead only setup an example, we try to have as little redundant as possible on topics that are well explain in yocto. Then just complement information.) 
### 1. Description
Variable name:
- DESCRIPTION = 
The variable contains a string value that should provide a package description that will be used by package managers. If not set, this value takes the value of the SUMMARY variable.
### 2. Section
Variable name:
- SECTION = 
Here you will define what type of recipe it is, the section in which packages should be categorized, it could be: utilities, applications, graphics, kernel, etc.
### 3. License
Variable name:  
- LICENCE =   
In this variable, you specify the type of license you want to use for your recipe for example:

- MIT
- BSD
- GPL  

However, you need to provide a license file for your selected licensee in the next variable. If LICENCE = 'CLOSED', then you will not use the next variable.
### 3.1 License file
Variable name:  
- LIC_FILES_CHKSUM =   
Here, is where you need to provide a licence file.
Note: The license files can be found under poky in **meta/files/common-licenses/**
### 4. Source for build
Variable name:  
- SRC_URI =   
Specify what sources files (local or remote) you want to build. 
### 5. Dependencies for build
Variable name:  
- DEPENDS =  
This are dependencies on other recipes whose contents (for example shared libraries or headers) are needed by the recipe at build time.
### 6. Recipe build tasks
[Build task](https://docs.yoctoproject.org/dev/ref-manual/tasks.html)  
The recipes use task to complete configuring, compiling, and packaging software. A continuation will describe the normal task of building a recipe.   
    - do_build  
    - do_compile  
    - do_configure  
    - do_deploy  
    - do_fetch  
    - do_image  
    - do_install  
    - do_package  
    - do_patch  
    - do_unpack  

#### 6.1 do_install variables   

| Sintaxis             |      Path description |
|----------------------|-----------------------|
| bindir               |        /usr/bin       |
| sbindir   	       |        /usr/sbin      |
| libdir	           |        /usr/lib       |
| sysconfdir	       |        /etc           |
| servicedir	       |        /srv           | 
| sharedstatedir	   |        /com           |
| localstatedir	       |        /var           | 
| datadir	           |        /usr/share     |
| infodir	           |        /usr/info      |
| mandir	           |        /usr/man       |
| docdir	           |        /usr/doc       |
| systemd_unitdir	   |        /usr/lib/systemd |
| systemd_system_unitdir  | 	/usr/lib/systemd/ system |
| systemd_user_unitdir	  |     /usr/lib/systemd/user |  
| includedir	          |     /usr/include |  

### 7. Priority of the recipe
Variable name:  
- PRIORITY =  
Indicates the importance of a package, this depends on the purpose for which the distribution is being produced.
You can set:
- "required"
- "standard"
- "extra"
- "optional"
### 8. WORKDIR variable
It is the pathname of the work directory in which the OpenEmbedded build system builds a recipe.  
The WORKDIR directory is defined as:
``` ${TMPDIR}/work/${MULTIMACH_TARGET_SYS}/${PN}/${EXTENDPE}${PV}-${PR} ``` 

## Example for application recipe 
For recipe file with the name:  helloworld.bb 
```
    DESCRIPTION = "Program that print Hello World! to standar oputput. 
    PRIORITY = "optional" 
    SECTION = "Examples" 
    LICENSE = "MIT" 
    LIC_FILES_CHKSUM = "file://${COMMON_LICENCE_DIR}/MIT;md5=0835ade698e0bcf8506ecda2f7b4f302" 
    SRC_URI = "file://helloworld.c" 
    S = "${WORKDIR}" 
    do_compile() { 
    ${CC} ${CFLAGS} ${LDFLAGS} helloworld.c -o helloworld 
    } 
    do_install() { 
    install -d ${D}${bindir} 
    install -m 0755 helloworld ${D}${bindir} 
    } 
```
# .bbappend file
Recipes used to append Metadata to other recipes, there are calls append files, these files use the .bbappend file type. This file is used to modify the recipe file, this should have the same name that the recipe, at the same time bbappend files allow your layer to make additions or changes to the content of another layer's recipe without having to copy the other recipe into your layer.  
Being able to append information to an existing recipe not only avoid duplication, also automatically applies recipe changes in a different layer to your layer.
## Extending recipes with .bbappend files
It is not necessary to recreate entire recipe files from scratch,  you can use **. bbappend** files to supplement an existing recipe file with new information, providing that the original information in the recipe file, resides in an existing layer.
## Systemd recipes
To use systemd services has necessary enabled this in the final image, because this is not enabled by default. Add the next lines to the configuration file **local. conf** to enable system as a default unit manager.

```
DISTRO_FEATURES_append = " systemd"
DISTRO_FEATURES_BACKFILL_CONSIDERED += "sysvinit"
VIRTUAL-RUNTIME_init_manager = "systemd"
VIRTUAL-RUNTIME_initscripts = "systemd-compat-units"
```
[//]: # (this section is too cryptic, try to be more direct and clear with your explanation)
### Important varibales for systemd recipe
1.**inherit systemd**
Each target has a name instead of a number, ensure the recipe inherits from the systemd class:  
``` inherit systemd ```  
2. NATIVE_SYSTEMD_SUPPORT = "1"
3. **SYSTEMD_AUTO_ENABLE**  
This variable specifies whether the specified service in SYSTEMD_SERVICE should start automatically or not. By default, the service is enabled to automatically start at boot time as follows: 
```SYSTEMD_AUTO_ENABLE = "enable"```  
>Note: you can disable this by setting the variable to ``` "disable"```
4. **SYSTEMD_SERVICE**
This variable specifies the systemd service name for a package.
>Note:You can specify multiples services, each one separated by a space.
```SYSTEMD_SERVICE:${PN} = "serviceName.service"```
5. **SYSTEMD_PACKAGES**
This variable locates the systemd unit files when they are not found in the main recipe's package.  
By default, this variable is set such that the systemd unit files are assumed to reside in the recipes main package:
```SYSTEMD_PACKAGES = "${PN}"```
6. **FILES**  
This variable provides a package name override that identifies the resulting package, then provide a space separated list of files or paths that identify the files you want included as part of the resulting package:
```FILES:${PN} += "${systemd_unitdir}/system/serviceName.service"```
## Example for systemd service recipe 
[//]: # (In general examples work better to explain things, but an example is not complete without an explanation)
```
SUMMARY = "Systemd example recipe"
DESCRIPTION = "Systemd service example"
LICENSE = "CLOSED"

inherit systemd
NATIVE_SYSTEMD_SUPPORT = "1"
SYSTEMD_AUTO_ENABLE = "enable"
SYSTEMD_SERVICE:${PN} = "serviceName.service"

SRC_URI += "file://serviceName.service"

FILES:${PN} += "${systemd_unitdir}/system/hello.service"

S = "${WORKDIR}"

do_install:append(){
    # Install systemd stuff
    install -d ${D}${systemd_unitdir}/system
    install -m 0644 ${WORKDIR}/hello.service ${D}${systemd_unitdir}/system
}
```

### Control systemd from command line
   ```systemctl status```: show the status of all services.  
   ```systemctl status <service>```: show the status of one service.  
   ```systemctl [start|stop] <service>```: start or stop a service.  
   ```systemctl [enable|disable] <service>```: enable or disable a service at boot time.  
   ```systemctl list-units```: list all available units.  
   ```journalctl -a```: show all logs for all services.  
   ```journalctl -f```: show only the last log entries, and keep printing updates as they arrive.  
   ```journalctl -u```: show only logs from a particular service.  


## Add bb files into your build
To add recipes files is necessary to modify the file **conf/local. conf** where it's important to add:
``` IMAGE_INSTALL_append = " recipe_name" ``` 

## Activity
For this activity, you must create and include in your bootable image, a service using a git repository.
1. You can use the tools listed in this and previous chapters.
2. Your service should be initialized by System D automatically