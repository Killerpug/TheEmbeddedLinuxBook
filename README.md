# TheEmbeddedLinuxBook
An introduction to embedded linux using Automotive Grade Linux as development platform

# Getting started
For those who want to read, go directly to the [main webpage](https://killerpug.github.io/TheEmbeddedLinuxBook/). 

# Developing content
This project uses:
- [mdbook](https://crates.io/crates/mdbook) and [mdbook-plantuml](https://github.com/sytsereitsma/mdbook-plantuml): To build the html that you will read. I like this project since you can create books with markdown formatting plus make use of plantuml diagrams (using mdbook-plantuml preprocessing) which are useful to give a general view of what we try to achieve on each topic.

## Setting up
1. [Install rust](https://www.rust-lang.org/tools/install). Mdbook is package created in Rust(awesome language btw) and distributed it as a crate in Cargo( Rust build system).

2. Install mdbook and mdbook-plantuml(PlantUML support)

    ```cargo install mdbook```

    ```cargo install mdbook-plantuml --no-default-features```

3.  Install java Runtime Environment. Required for Plantuml diagrams.

    run ```sudo apt install default-jre``` for Linux

    or download package from [java](https://www.java.com/en/download/)

4. Download plantuml.jar and GraphViz following the instructions in [PlantUML](https://plantuml.com/starting). Put the **plantuml.jar** executable in the project root. 

Graphviz depends on the OS:
For Windows: download graphviz.exe and install. Or use chocolatey ```choco install graphviz```
For Linux: ```sudo apt install graphviz```

You are all set!

## Building the book
1. First build the book and then deploy it locally

```
    mdbook build
    mdbook serve
``` 
Now book is deployed locally at URL http://localhost:3000.

## Checking spelling
We use aspell for checking word sytax and lychee to look for broken links. Install and perform checking before PR, otherwise spellChecker or linkChecker actions will fail.

### Installing checker tools
```
    sudo apt-get install aspell
    cargo install lychee
```

### Check your text 
lychee ./src/**/*.md            -> look for broken links
bash tools/spellcheck.sh list   -> look for misspelled words