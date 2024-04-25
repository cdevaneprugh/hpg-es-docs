# Introduction

The primary goal of this document is to serve as a guide for porting the [CESM](https://www.cesm.ucar.edu/) and [E3SM](https://e3sm.org/) Earth models to HiPerGator. I mainly follow the official Earth model documentation, and add steps specific to HiPerGator as needed. Additionally, I'm including a section that can serve as an introduction to the Linux operating system, which many students have limited exposure to.

## Glossary

__CESM__ - Community Earth System Model. CESM is a fully-coupled, community maintained, global climate model that provides state-of-the-art computer simulations of the Earth's past, present, and future climate states.

__E3SM__ - Energy Exascale Earth System Model. E3SM is the Department of Energy's climate model. Forked from CESM v1 and developed independently by the DOE, they describe E3SM as "an ongoing, state-of-the-science Earth system modeling, simulation, and prediction project that optimizes the use of DOE laboratory resources to meet the science needs of the nation and the mission needs of DOE."

__CLM__ - [Community Land Model](https://www.cesm.ucar.edu/models/clm). The land component used in CESM. It has the capability to model specific processes such as vegetation composition, heat transfer in soil, carbon-nitrogen cycling, canopy hydrology, and many more.

__HPG__ - HiPerGator. The supercomputer used at UF. I'll use HPG and HiPerGator interchangeably throughout the documentation.

## General Linux Information

If you are new to HiPerGator or using Linux I suggest you read through this section and utilize at least one of the resources linked below. While HiPerGator offers access to the system with a GUI or the ability to code via Jupyter notebooks, the Earth models will require you to use the command line exclusively, so it's important you are comfortable doing so.

### Linux Command Line and Bash Overview

The Linux command line, also referred to as the terminal or shell, is a text-based interface for interacting with the Linux operating system. It allows users to execute commands and perform various tasks efficiently, without relying on a graphical user interface.

**Command Line Advantages:**

- **Efficiency:** The command line provides quick access to powerful tools and utilities, enabling you to perform tasks more efficiently compared to a GUI.
- **Automation:** Command line scripting allows for the automation of repetitive tasks, saving time and effort.
- **Flexibility:** Advanced users can customize and tailor their workflow to suit their specific needs, leveraging the extensive capabilities of the command line.

**Bash:**
Bash (Bourne Again Shell) is one of the most commonly used command line interpreters in Linux. It is the default shell for most Linux distributions (including HiPerGator) and provides a powerful scripting environment for automation and system administration tasks.

### Overview of Basic Commands:

1. **pwd (Print Working Directory):**
   - Displays the current directory path.
   - Example: If you are in the "home" directory of the user "cooper," `pwd` would output `/home/cooper` in your terminal.
2. **ls (List):**
   - Lists files and directories in the specified directory (or current directory if one isn't given).
   - Example: `ls my_files` would list the contents of the directory "my_files."
3. **cd (Change Directory):**
   - Changes the current directory to the specified one.
   - Example: `cd dir1` will change your current working directory to `dir1` so long as that directory is in your current working path.
   - If no directory is specified, `cd` will move you to your `home` directory.
4. **touch:**
   - Creates an empty file.
   - Example: `touch filename.txt` creates an empty text file.
5. **mkdir (Make Directory):**
   - Similar to `touch`, but creates an empty directory instead. 
   - As with all these commands, you can use relative or absolute paths. For example `mkdir dir1` creates an empty directory named "dir1" in the current directory. `mkdir /home/cooper/dir1` creates "dir1" in Cooper's home directory.
6. **rm (Remove):**
   - Delete files or directories.
   - Example: `rm filename.txt` (for files), `rm -r directory` (for directories where the `-r` flag specifies to remove things recursively).
7. **cp (Copy):**
   - Copies files or directories.
   - Example: `cp file1 file2` (to copy file1 to file2), `cp -r dir1 dir2` (to copy dir1 to dir2 recursively).
8. **mv (Move):**
   - Moves or renames files or directories.
   - Example: `mv file1 file2` (to rename file1 to file2), `mv file1 /path/to/directory` (to move file1 to another directory).
9. **grep (Global Regular Expression Print):**
   - Searches for text patterns.
   - Example: `grep foo notes.txt` searches for the term "foo" in the file notes.txt.
10. **man (Manual):**
    - Displays manual pages for commands, providing detailed information about their usage and options.
    - Example: `man ls` displays the manual page for the `ls` command.
11. **Piping (|):**
    - Allows the output of one command to be used as input to another command.
    - Example: `command1 | command2` (output of `command1` is used as input for `command2`).
    - Example: `ls Documents | grep my_file` Will search the "Documents" directory for files titled "my_file." You could also add the `-r` flag to the `grep` command to search within each file for the search term.

### Text Editors

The three main text editors used on Linux are [Vim](https://en.wikipedia.org/wiki/Vim_(text_editor)), [Emacs](https://en.wikipedia.org/wiki/GNU_Emacs), and [Nano](https://en.wikipedia.org/wiki/GNU_nano). `nano` is the most basic, and easiest to use. `vim` and `emacs` are extremely customizable, but have a substantially steeper learning curve. Between `vim` and `emacs`, `vim` is arguably the most difficult to get comfortable with, but is powerful once you do. If you decide you are brave enough to try it, sites like [openvim](https://www.openvim.com/) or running the command `vimtutor` (which starts an interactive tutorial) on any Linux machine are good places to start. Additionally [here](https://vim.rtorr.com/) is a cheat sheet of `vim` commands that serve as a nice reference.

__WARNING__ The default text editor on Linux is usually `vim`. There may be a time when you accidentally open a file in `vim` and can't figure out how to exit the program. This may sound silly, but I promise you, the jokes about `vim` being confusing are endless in the Linux community. If you do find yourself trapped in and need to exit. Hit the `esc` key, then type `:q!` and hit `enter`. This will exit whatever file you got yourself into without saving any changes. If you _would_ like to make a quick change to the file you got into, hit `esc`, followed by the `i` key to enter "insert" mode. At this point you can type up whatever you need (use the arrow keys for navigation). To save, use `esc` to exit insert mode, then type `:wq` and hit `enter`. This will write the file (`w`) then quit out of the editor (`q`). 

### External Resources

Here are some resources to get started using the Linux command line and learn a bit more about what is going on "under the hood." You don't need to memorize everything in these links, but they can serve as a good starting point.

1. https://linuxjourney.com/

   The "Grasshopper" and "Journeyman" sections will give you a good chunk of background knowledge. I should note that because HiPerGator is not a traditional computer, some of the information (like package management) will not directly translate. However, it will help build a foundation of Linux knowledge and is certainly worth being exposed to. 

2. Hipergator's [Introduction to the Linux Command Line](https://github.com/UFResearchComputing/Linux_training/blob/main/non_HiPerGator.md)

   A good demonstration of basic Linux commands you will be using on a daily basis, along with some practice exercises. I highly recommend going through this entire lesson and watching the accompanying [training video](https://help.rc.ufl.edu/doc/Training_Videos).

3. jlevy's [art of the command line](https://github.com/jlevy/the-art-of-command-line)

   This write up covers a wide range of commands and could serve as a good reference sheet. 

4. YouTube has plenty of tutorials. [Here's a good one](https://www.youtube.com/watch?v=s3ii48qYBxA) that serves as an introduction to the command line.

5. [ChatGPT](https://chat.openai.com/) is actually pretty good at BASH scripting and serving as an assistant for Linux commands.

## HiPerGator Specific Information

HiPerGator is not set up like a traditional personal computer. While Google and sites like [stack overflow](https://stackoverflow.com/) are great resources to use when you run into problems, remember to check HiPerGator's [documentation](https://help.rc.ufl.edu/doc/UFRC_Help_and_Documentation) first, as it will have information specific to our system.

Additionally, when using Linux, you should build the habit of using the commands `man` and `--help` to get yourself out of trouble. `man` will access the built in documentation for Linux commands and programs that are on the system. For example, say you want to learn about the `ls` command, and all the options it has. You can type `$ man ls` (where the $ sign is your `bash` prompt) to pull up the documentation page. You can also type `$ ls --help` for a list available arguments and options. 

If you are new to HiPerGator I would highly recommend reading through their [FAQ](https://www.rc.ufl.edu/documentation/frequently-asked-questions/),  [Getting Started](https://help.rc.ufl.edu/doc/Getting_Started) page, and watching all of the available training videos provided in the [documentation](https://help.rc.ufl.edu/doc/Training_Videos).

### Module Systems and lmod

The way programs are used on HiPerGator is different than a traditional computer. HiPerGator uses a program called `lmod` to be able to have multiple versions of the same software installed for different use cases. [Here is a good article](https://www.admin-magazine.com/HPC/Articles/Lmod-Alternative-Environment-Modules?utm_source=ADMIN+Newsletter&utm_campaign=HPC_Update_31_Lmod_Alternative_to_Environment_Modules_2013-01-30) on module systems and how they work. While the HiPerGator [documentation](https://help.rc.ufl.edu/doc/Modules) has great info, the full documentation for `lmod` can be found [here](https://lmod.readthedocs.io/en/latest/). 

### Job Schedulers and slurm

Unlike a personal computer, HiPerGator is a collection of connected computers (called nodes) that share resources. The node that you login to (unsurprisingly called a login node) has a limited amount of resources, and is generally used for tasks like writing code, file management, and very light jobs. As such, you will submit more computationally "expensive" jobs to the SLURM scheduler to run on the dedicated compute nodes.

Please read the page on our scheduler [here](https://help.rc.ufl.edu/doc/HPG_Scheduling). The full documentation for the SLURM scheduler can be found [here](https://slurm.schedmd.com/documentation.html).

## Earth Models

Both Earth models (E3SM and CESM) use the same underlying infrastructure (CIME) to build and run cases (experiments). CIME generates the scripts necessary to download appropriate files, build the case, and push it to the scheduler to run on a compute node. In some sense, downloading and porting the Earth models is not so much about the specific Earth model, and more about getting CIME to interact with HiPerGator properly. The configuration files CIME uses are almost entirely the same (except for a couple extra parameters that E3SM uses). Ultimately the goal at UF is to have the Earth models installed in some shared directory on HiPerGator, but until then this can serve as a guide to do an install for your group. Going forward, I am assuming you are reasonably comfortable navigating HiPerGator and using basic Linux commands.

### Program Prerequisites

__Default Loaded Modules__

To download the external components of CESM (like CLM) we need a program called `subversion`. Additionally, the Earth models need to utilize `perl` scripts and `cmake`. While there is a `perl` library loaded by default, it is generally a better practice to specify which version we want to use. This will make future debugging easier. To load the programs needed, we can do something like:

`$ module load perl/5.24.1`

`$ module load subversion/1.9.7`

`$ module load cmake/3.26.4`

`$ module save default`

This will load the needed programs and save them as being loaded by default when you log in to HiPerGator.

__Netcdf__

Add info on netcdf, loading the modules, problems, where things go wrong, etc.

## Porting CESM

CESM has [two primary releases](https://www.cesm.ucar.edu/models), the current development release (v2.2.2 at the time of this writing), and the production release (v2.1.5). We will be using the production release for our purposes. I am following the [CESM documentation](https://escomp.github.io/CESM/versions/cesm2.1/html/index.html), as well as the [CIME porting documentation](https://esmci.github.io/cime/versions/master/html/users_guide/porting-cime.html) while adding the steps needed to get this working on HiPerGator.

__Downloading the code__

While the repository is not that large, it should be put on the `/blue` drive for fast access. It is completely up to the user and group how the file structure is organized. We decided to have a shared directory called `/earth_models` that contains the source code for the CESM and E3SM models, as well as the input data needed to run cases. Remember to make sure that all users in the group have read/write privileges to this directory.

1. Go to where your group's Earth models will be installed.

`$ cd /blue/GROUP/earth_models`

2. Follow directions in the [CESM documentation](https://escomp.github.io/CESM/versions/cesm2.1/html/downloading_cesm.html) to clone the repository and checkout the external components.

3. By default, CESM downloads the most recent version of the CIME code, which unfortunately causes some bugs. We can fix that by checking out an older, more stable version.

`$ cd cime`

`$ git pull origin maint-5.6`

`$ git checkout maint-5.6`

If you run `./checkout_externals -S` after changing the CIME branch, it may show an error that CIME is not using the correct version. You can ignore this.  

__Install the cprnc tool__ 

While this is _technically_ optional, [cprnc](https://github.com/ESMCI/cprnc) is a tool used to compare netcdf files. We installed this in `/blue/GROUP/earth_models` as it is a utility shared by both CESM and E3SM.

1. Go to the installation directory

`$ git clone https://github.com/ESMCI/cprnc.git`

`$ cd cprnc`

2. You need to make sure `cmake`, a compiler suite (intel or gnu), and netcdf libraries are loaded. For the gnu compiler built netcdf libraries:

`$ module gcc/12.2.0 openmpi/4.1.5 netcdf-c/4.9.2`

`$ module gcc/12.2.0 openmpi/4.1.5 netcdf-f/4.6.1`

Follow directions in the README. `cprnc` is a fortran program, and it may throw an error that it can't find the netcdf-c files, but this doesn't seem to matter.

`$ mkdir bld`

`$ cd bld/`

`$ cmake ../`

`$ make`

The executable will be located at `/blue/GROUP/earth_models/cprnc/bld/cprnc`.

### Porting and Validating CIME

In general, you can just follow the directions [here](https://esmci.github.io/cime/versions/master/html/users_guide/porting-cime.html). However, it's not necessary to validate your MPI environment. Additionally, in the first part of step 6.4 they discuss two methods for defining the machine. We will be using option 2, storing configuration files in your home directory.

__Set up the config files__

These are stored in a dot folder in your home directory. `~/.cime` and are appended to the default config files in the Earth models.

* The directories for input data need to be created manually.

__Initial testing__

Once the config files are setup, we need to run regression tests to ensure things are working correctly and CIME knows where to locate things on HiPerGator. The regression tests script will test various parameters of the Earth model in isolation, then send a dozen or two small cases to the scheduler to be run. This script can be run from the login node.

`$ /blue/GROUP/earth_models/CESM/cime/scripts/tests/scripts_regression_tests.py`

You'll need python to run the script. Version 3.11 seems to work just fine.

`$ module load python/3.11`

You can run the script with several options. Use the `--help` tag when running the script for the full list. For example, if you want to run the script and test the intel compilers, with a specific output directory, you could do something like:

`$ ./scripts_regression_tests.py --compiler intel --test-root PATH_TO_OUTPUT_DIR`

By default, the test output will be saved in the `$CIME_OUTPUT_ROOT` directory defined in your `config_machines.xml` file.

These tests can take up to an hour or two to finish everything (depending how busy your scheduler is as well as how many resources your group has paid for).

Once all these are passed we can move onto the ECT tests. As as aside note, when all the tests are run, occasionally you get some failures on individual components. They are usually the Q_TestBlessTestResults  T_TestRunRestart or Z_FullSystem. But if you run the test individually, and they pass, overall your system should be good.

__ECT Tests__

Follow the guide [here](https://www.cesm.ucar.edu/models/cesm2/python-tools) in order to complete these tests.

To run the script to create these tests we have to load an older version of python.cd

`$ module load python-core/2.7.14`

__add conda environment section__

Not sure the best way to do this. I should look into exporting a yum package to be able to install my environment for people to use.0
