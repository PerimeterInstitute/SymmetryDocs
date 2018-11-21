# Welcome to Perimeter's HPC system "Symmetry"

Symmetries are important in physics. [Noether's
theorem](https://en.wikipedia.org/wiki/Noether%27s_theorem) states
that every local symmetry of a physical system generates a
conservation law. In honour of this principle, Perimeter's HPC system
is called *Symmetry*.

Symmetry is intended to serve the needs of Perimeter researchers,
filling a gap between personal devices such as laptops and desktops,
and large national sytems offered e.g. by [Compute
Canada](https://www.computecanada.ca). As such, each node of Symmetry
is significantly more powerful than a laptop, but cannot compete with
a national system such as
[Graham](https://docs.computecanada.ca/wiki/Graham) or
[Niagara](https://docs.computecanada.ca/wiki/Niagara).

(This documentation is still under construction, and should be
completed within the next days. Please report errors, omissions, and
suggestions for this documentation to our [help
desk](mailto:help@perimeterinstitute.ca).)



## Contact and help

As usual for all technical systems at Perimeter, the main channel to
report issues and ask for assistance is our [help
desk](mailto:help@perimeterinstitute.ca).

For online discussions, there is a Gitter chat room [Computing at
Perimeter](https://gitter.im/Computing-at-Perimeter/Lobby). This chat
room is not restricted to discussing Symmetry, but is for all topics
related to Computational Physics at Perimeter.



## System description

### Hardware

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

### Available Software

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

- [Slurm](https://slurm.schedmd.com) scheduler (our queueing system)

### Modules

Some of the software packages use [Environment
modules](http://modules.sourceforge.net). This means you need to load
a module before the package is available. Use `module avail` to see
what modules are avilable, `module load` to load a module. There are
also `module list`, `module unload`, and `module help`.

### Python

#### Local versions

We have installed Conda installations with some popular python
packages pre-installed.  If you require additional packages, please
contact the [help desk](mailto:help@perimeterinstitute.ca) and we will
either install the package, or advise how best to proceed.

To use these python environments, run one of these commands:
```
module load python
```
for the default Python 3.7 version, or

```
module load python/3.6
```

or

```
module load python/2.7
```

for these older versions.  Once you have loaded the module, you should see
that this is tha `python` command that will be run:

```
$ module load python
$ which python
/cm/shared/apps/conda-environments/python37/bin/python
$ python --version
Python 3.7.0
```

If you would like to install a new package that is available through
`pip`, you can install it within your home directory by running:

```
pip install --user [package-name]
```

Note that these python environments are the same ones used in the
Jupyterhub service, so if you install a local package this way, it
will be available to you in Jupyterhub.


#### "System" Python

The "system" versions of Python, `/usr/bin/python` and `/usr/bin/python3`, are provided by the Ubuntu distribution.
We will only update these versions as new versions are released by Ubuntu, and we will probably only install
Python packages that also have an Ubuntu package (for example, `python-numpy`).

You can install your own Python packages using `pip install --user`.  For example:

```
pip3 install --user astropy
```

This will install the `astropy` package inside your home directory, in `$HOME/.local/lib/python3.7/site-packages/astropy`.

#### Conda Python

We have also installed the `anaconda` Python distribution.  You can use it via:

```
module load anaconda2
```

or

```
module load anaconda3
```

If you prefer to have more control over your Python environment (or you want to have multiple independent setups), 
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



## Using Symmetry

### Access

All researchers at Perimeter have in prinple access to Symmetry.
Please contact the [help desk](mailto:help@perimeterinstitute.ca) to
enable this access. It is probably a good idea to enable
[VPN](https://portal.perimeterinstitute.ca/how-to/sonicwall-mobile-connection-vpn-installation-and-setup-instructions)
and [ssh](https://portal.perimeterinstitute.ca/how-to/ssh-perimeter)
access to Perimeter at the same time. Symmetry is located behind
Perimeter's firewall, and is not directly accessible from the outside.

There are two ways to access Symmetry, the traditional command-line
based way using `ssh`, and via a web browser and `JupyterHub`:

#### Access via ssh

To log in, use `ssh USERNAME@symmetry`. (Replace `USERNAME` with your
user name.) This will ask for your Perimeter password. We recommend
generating ssh keys and using an ssh key chain to allow a
password-less access. (Question: Where is a good tutorial for this?)

On Linux and MacOS, ssh is pre-installed. On Windows, you
might need to install a client such as [PuTTY](https://www.putty.org).
(Question: Is there a better alternative to PuTTY?)

#### Access via JupyterHub

(to be described)



### Running jobs

While you can run jobs interactively on the head nodes, you need to be
careful when doing so: Head nodes are shared between all users on
Symmetry. **Do this only for tasks that do not need many resources.**
For example, compiling code, or brief tests of a Julia or Mathematica
notebook are probably fine. If in doubt, use a compute node instead.

If you overload a head node by using too much memory or too many
threads, others might suffer, and an administrator might have to stop
in and abort your task. (Question: Is there a tutorial explaining how
to use `top` to monitor one's processes?)

Generally, we recommend running jobs on Symmetry's compute nodes, as
described below.

### Slurm queueing system

(to be described: basic principles, available queues)

### Running jobs interactively

(JupyterHub, srun, reservations, ...)

### Running batch jobs

(to be described, point to examples)
