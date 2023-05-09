# Recipes
Official page: https://docs.yoctoproject.org/overview-manual/concepts.html#recipies

## Recipes files
Recipies files or also known as bb files, are files that contain information and instructions that build tool (bitbake) takes for generate the packages. This informations for instructions can be resume as:
1. Descriptive information about package
2. Section information
2. The recipe version
3. The Licience
3. Existing dependencies 
4. Where the source code resides and how to fetch it 
5. Whether the source code requires any patches, where to find them, and how to apply them.
6. How to configure and compile the source code.
7. Where on the target machine to install the package or packages created.
## Variables for bb files
https://docs.yoctoproject.org/dev/ref-manual/variables.html
### 1. Description
Variable name:
- DESCRIPTION = 
The variable contains a string value that should provide a package description that will be used by package managers. If not set, this value takes the value of the SUMMARY variable.
### 2. Section
Variable name:
- SECTION = 
Here you will difine what type of recipe it is, the section in wich packages should be categorized, it could be: utilities, applications, graphics, kernel, etc.
### 3. License
Variable name:  
LICENCE =   
In this variable, you specify the typoe of license you want to use for your recipe for example:

- MIT
- BSD
- GPL  

However you need to provide a license file for your selected license in the next variable. If LICENCE = 'CLOSED', then you will not use de next variable.
### 3.1 License file
Variable name:  
LIC_FILES_CHKSUM =   
Here, is where you need to provide a licence file.
Note: The licenses files can be found under poky in **meta/files/common-licenses/**
### 4. Source for build
Variable name:  
SRC_URI =   
Specify what sources files (local or remote) you want to build. 
### 5. Dependencies for build
Variable name:  
DEPENDS =  
This are dependencies on other recipes whose contents (for example shared libraries or headers) are needed by the recipe at build time.
### 6. Recipe build tasks
https://docs.yoctoproject.org/dev/ref-manual/tasks.html  
Recipies use task to complete configuring, compiling, and packaging software. A continuation will describe normal task for building a recipe.  
6. 1. do_build  
6. 2. do_compile  
6. 3. do_configure  
6. 4. do_deploy  
6. 5. do_fetch  
6. 6. do_image  
6. 7. do_install  
6. 7. 1. do_install variables   

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

6. 8. do_package  
6. 9. do_patch  
6. 10. do_unpack  
### 7. Priority of the recipe
Variable name:  
PRIORITY =  
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

## Example for recipe
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
Recipes used append Metadata to other recipes, there are called append files, these files use de .bbappend file type. This file is used to modify the recipy file, this should have the same name that the recipe, at the same time bbappend files allows your layer to make additions or changes to the content of another layer's recipe without having to copy the pther recipe into your layer.  
Being able to append information to an existing recipe not only ovoid duplication, also automatically applies recipe changes in a different layer to your layer.
## Extending recipes with .bbappend files
It is not necessary to recreate entire recipe files from cero, you can use **.bbappend** files to suppement an existing recipe file with new information, providing that the original information in the recipe file, resides in an exiting layer.
# Add bb files into your build
To add recipes files is necessary to modify the file **conf/local.conf** where it's iomportant to add:
``` IMAGE_INSTALL_append = " recipe_name.bb" ``` 
