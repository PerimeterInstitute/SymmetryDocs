# Welcome to Perimeter's HPC system "Symmetry"

Symmetries are important in physics. [Noether's
theorem](https://en.wikipedia.org/wiki/Noether%27s_theorem) states
that every differentiable symmetry of a physical system leads to a
conservation law. In honour of this principle, Perimeter's HPC system
is called "Symmetry".

## Overview

Symmetry is intended to serve the needs of Perimeter researchers,
filling a gap between personal devices such as laptops and desktops,
and large national sytems offered e.g. by [Compute
Canada](https://www.computecanada.ca). As such, each node of Symmetry
is significantly more powerful than a laptop, but cannot compete with
a national system such as
[Graham](https://docs.computecanada.ca/wiki/Graham) or
[Niagara](https://docs.computecanada.ca/wiki/Niagara).

Symmetry consists of:

- 2 head nodes, which can be used for interactive work and to submit
  jobs. Each head node has 40 Intel Xeon Silver cores and 200
  GigaBytes of memory (RAM).

- 76 compute nodes, which are designed to run compute-intensive
  applications. Each compute node has 40 Intel Xeon Gold (Skylake)
  cores and 200 GigaBytes of memory (RAM).

- A file server hosting a GPFS file system offering 233 TeraBytes of
  space.

- A high-performance InfiniBand network connecting the nodes and the
  file server.

There are various additional bits and pieces, primarily for
administration, that are mostly invisible to general users.

## Access

All researchers at Perimeter have in prinple access to Symmetry.
Please contact the [help desk](mailto:help@perimeterinstitute.ca) to
enable this access. It is probably a good idea to enable
[VPN](https://portal.perimeterinstitute.ca/how-to/sonicwall-mobile-connection-vpn-installation-and-setup-instructions)
and [ssh](https://portal.perimeterinstitute.ca/how-to/ssh-perimeter)
access to Perimeter at the same time. Symmetry is located behind
Perimeter's firewall, and is not directly accessible from the outside.

There are two ways to access Symmetry, the traditional command-line
based way using `ssh`, and via a web browser and `JupyterHub`:

### Access via ssh

To log in, use `ssh USERNAME@symmetry`. (Replace `USERNAME` with your
user name.) This will ask for your Perimeter password. We recommend
generating ssh keys and using an ssh key chain to allow a
password-less access. (Question: Where is a good tutorial for this?)

On Linux and MacOS, ssh is pre-installed. On Windows, you
might need to install a client such as [PuTTY](https://www.putty.org).
(Question: Is there a better alternative to PuTTY?)

### Access via JupyterHub

(Dustin Lang to write this section.)

## Available software

Symmetry provides a wide range of software. If you need additional
software, you can request this via the [help
desk](mailto:help@perimeterinstitute.ca), or you can install it into
your home directory.

Pre-installed software:

- [Ubuntu](https://www.ubuntu.com) 16.04 LTS, with many scientific and
  development packages (e.g. FFTW, GCC compilers, GSL, LLVM compilers,
  OpenBLAS, OpenMPI, and many more)

- [Intel Parallel Studie
  XE](https://software.intel.com/en-us/parallel-studio-xe), including
  C, C++, Fortran compiler, OpenMP, MPI, Debugger, Profiler, etc.

- [Julia](https://julialang.org)

- [Maple](https://www.maplesoft.com/products/Maple/)

- [Mathematica](http://www.wolfram.com/mathematica/)

- [Matlab](https://www.mathworks.com/products/matlab.html)

- [Python](https://www.python.org)

- [Slurm](https://slurm.schedmd.com) scheduler

### Modules

Some software requires a module to be loaded before it is available.
Use `module avail` to see what modules are avilable, `module load` to
load a module.

## Building and installing software yourself

## Running jobs

As described above, you can use either Symmetry's head nodes for
interactive work, or can submit batch jobs via our queueing system.

### Running jobs interactively

You can run directly on Symmetry's head node. Be careful: This head
node is shared between all users on Symmetry, and this should only be
done for (ASDFASDFASDF)

### Running jobs on compute nodes

(Slurm)



## Software descriptions

#### "System" python

The "system" versions of python, `/usr/bin/python` and `/usr/bin/python3`, are provided by the Ubuntu distribution.
We will only update these versions as new versions are released by Ubuntu, and we will probably only install
python packages that also have an Ubuntu package (for example, `python-numpy`).

You can install your own python packages using `pip install --user`.  For example:

```
pip3 install --user astropy
```

This will install the `astropy` package inside my home directory, in `$HOME/.local/lib/python3.7/site-packages/astropy`.

#### Conda

We have installed the `anaconda` python distribution.  You can use it via:

```
module load anaconda2
```

or

```
module load anaconda3
```

If you prefer to have more control over your python environment (or you want to have multiple independent setups), 
you can use the `conda` package manager.  For example:

```
module load miniconda3
conda create -n myenvA astropy
source activate myenvA
python -c "import astropy; print(astropy.__file__)"
###> /home/me/.conda/envs/myenvA/lib/python3.7/site-packages/astropy/__init__.py
# when you are finished:
source deactivate
```





### (markdown cheat sheet)

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```
