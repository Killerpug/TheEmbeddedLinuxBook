# The Linux system

## A short Timeline of computer advancements:

**1945-1955** vacuum tubes and punching cards.

**1955-65**: Transistors and batch systems:

Batch systems accumulate many jobs into tape and run all 
the tape at once. Separation between designers, operators, 
programmers(assembly or FORTRAN) and maintenance.

**1965-80**: ICs, multi programming and the birth of UNIX and Microsoft:

In the 1960s, Moore’s Law predicted that the number of transistors on an integrated circuit would **double every eighteen months**. The **IBM 360** (A family of compatible computers that ranged from low to high performance were built to satisfy the growth of the programs of the client), had the goal achieving compatibility of programs across the family of computers that share the same architecture, however that required that the piece of code called "operating system" meet all the conflict requirements for scientific and commercial environments, which obviously resulted in thousands of bugs in assembly code; but managed to satisfied most of the customers reasonably well. They popularized several key techniques like the multi programming to **keep computer working 100% of the time by keeping 3 jobs** on different memories, utilizing the idle time that the CPU waited for IO for another job.

**(1974) 8080 First general-purpose 8-bit Intel CPU**.

Kildall wrote the OS and found Digital Research, but a killing decision of not writing an OS for IBM in 1980 led Seattle Computer Products to create DOS(Disk Operating System) and Bill Gates hired **Tim Patterson** to revise it, becoming **MS-DOS**.

Doug at Stanford Research did some research on GUI's and Xerox PARC adopted into the machines they built, **Steve Jobs saw them and embarked on building an Apple with a GUI(LISA)**. Second attempt succeeded because it was user friendly, meaning that it was **intended for users who not only knew nothing about computers but furthermore had absolutely no intention whatsoever of learning**. Apple inspire Windows 95, the first freestanding version of a GUI built into a modified MS-DOS.

**MULTICS** was developed as a way to **share a computer across multiple users**(yes this is where cloud computing started) and it was a success because  people knew how to write small, efficient programs(a skill that has been lost). However this effort did not take over the world because it was written in PL/I which was an obsolete compiler but many discoveries helped to develop UNIX OS. **UNIX began with a MULTICS** version and grew into multiple incompatible versions, which led to the development of the **POSIX standard by the IEEE that standardizes interfaces to run a program into any UNIX system**.

**1980-Present**: Personal Computers MINIMIX(1987) and Linux:

The detection and replacement of faulty/crashed modules on the fly without a reboot or disturbing running programs was its goal: reliability, dependability and availability. This system is described on Operating Systems Design and implementation (Tanenbaum & Woodhull).

MINIMIX led to **Linux**, a free production OS created by Linus Torvalds. With the development of Large Scale Integration circuits, chips containing thousands of transistors, personal computers became affordable.

# The Unix OS

**UNIX**, is a family of operating systems that derive or behave like AT&T Unix(1969). Unix and Unix-like Operating Systems have been standardized to comply with POSIX standard. 
The main features of Unix that allowed it to create general-purpose reusable/modular programs that can be combined to create the first "scripting language" that enable us to produce complex workflows are:

- tree filesystem
- file descriptors
- pipes
- shell syntax operations

[AT&T Archives: The UNIX Operating System](https://www.youtube.com/watch?v=tc4ROCJYbm0)
