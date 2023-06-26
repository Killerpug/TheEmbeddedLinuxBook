# Yocto Introduction
More information can be found on the official project website:
[Yocto Official Documentation](https://www.yoctoproject.org/).
## What is Yocto?
First of all, you have to understand that this is not a Linux distribution. It is an open-source project that provides developers with useful tools such as software stacks, layers, templates, and methods, to create a custom Linux-based system from scratch for different hardware architectures, this allows developers to boot on different embedded hardware, making embedded Linux simpler and more accessible.  
[//]:# (An example would help to give more context about why and when to implement yocto. maybe showcase a project or link to some demo or project using yocto and highlight characteristics.)
## Why is this important?
The platform we will work on (Automotive Grade Linux) is based on this project using the same tools and some compatibility layers, so it is important first to understand the basics of Yocto and then apply this to AGL.  
## Compatibility
As an open project, managed by the Linux Foundation, it can support multiple architectures including:
- x86 (32/64 bits)
- PPC
- ARM
- MIPS

[//]:# (briefly describe each component, why do we need each component?)
## Main components
The Yocto project works thanks to a collection of multiple components, including parts of another project called OpenEmbedded, tools such as:
- Bitbake
- OpenEmbedded-Core
- Metadata   

The integration of the OpenEmbedded tools and the Yocto components make up the **Poky** platform, we can see it as an enhanced Buildroot.  

[//]:# (I dont clearly see the purpose of this paragraph, maybe need rephrasing.)
## To develop software
For adding a single executable file, a toolchain, source files and instructions to compile are sufficient. However, for entire projects that we need dependencies to compile and run, this becomes more complex and we will need additional steps that will form a whole recipe from which variants could be derived depending on the target hardware architecture.
To interpret and execute these recipes we will use a Bitbake tool.

## Start with Yocto
The first thing to do is to initialize a new environment within the project and edit the **conf/local. conf** file in which we can choose the target hardware by means of the **MACHINE** variable before compiling the project by means of bitbake.
Unlike similar projects Yocto stands out in the ease of adding new recipes or modify existing ones, for this we use recipe files that usually with the extension **. bb** (hello. bb), and to add something to this recipe we use the extension **. bbappend** keeping the name of the original file (hello. bbappend).

