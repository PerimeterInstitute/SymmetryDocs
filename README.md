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

- [Slurm](https://slurm.schedmd.com) resource manager (our queueing
  system)

### Modules

Some of the software packages use [Environment
modules](http://modules.sourceforge.net). This means you need to load
a module before the package is available. Use `module avail` to see
what modules are avilable, `module load` to load a module. There are
also `module list`, `module unload`, and `module help`.

### Python

[Several Python versions are available](python)


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

To log in, visit <https://symmetry.pi.local>.  As you might guess from
the name, this is currently only available within the Perimeter
network.  (If you want to access it from outside, please use the PI
VPN.)

Currently, you will get a warning that *Your connection is not
secure*, because we don't have a proper security certificate yet.
Please tell your web browser that this is okay (in Firefox: *Advanced*
→ *Add Exception* → *Confirm Security Exception*).

![screenshot of https://symmetry.pi.local, with security warning]({{ site.url }}/assets/jupyterhub-1.png)

![Jupyterhub login screen]({{ site.url }}/assets/jupyterhub-2.png)

Log in using your normal PI username and password, without the
`@perimeterinstitute.ca`.

You will arrive at a `Spawner Options` page with a drop-down menu,
asking you where you would like to run your Jupyter notebook.  You can
run on the head node---a computer shared with many other users---or on
one of the compute nodes.  The head node will be available
immediately, while the compute nodes may take a while to become
available, because we have to wait for a batch job to be scheduled.
The compute nodes are more powerful and are more suitable for
expensive computations or memory-hungry applications.

![Jupyterhub Spawn Options screen]({{ site.url }}/assets/jupyterhub-3.png)

Once you have selected where to run your notebook, you will arrive at
the main Jupyter notebook page, which by default will show you your
home directory.  From here, you can *Upload* files (using the *Upload*
button at the top-right), and download files (by clicking on them).
At the top-right there is a *New* drop-down menu to start a new
*Jupyter kernel*, an interactive programming environment.

![Jupyterhub Spawn Options screen]({{ site.url }}/assets/jupyterhub-4.png)

If you started running on the head node but would now like to run on a
compute node, use the *Control Panel* button at the very top-right,
then hit the *Stop My Server* button, then the *Start My Server*
button will take you back to the *Spawner Options* page.

Currently, we have several *Python* versions, a *Bash* (shell)
environment, and a recent *Julia* version.  If you want to use a
different programming language, please write a note to
help@perimeterinstitute.ca with the details!

#### Custom Kernels 

It is possible to add your own *kernels* to the list of kernels.  When it
starts, Jupyterhub will search for kernel files in these directories:

```
/cm/shared/apps/conda-environments/jupyterhub/share/jupyter/kernels/
$HOME/.local/share/jupyter/kernels
```

To create a new kernel for yourself (say, using a special Conda
environment you have created), you can copy one of the existing kernel
files and modify it for your needs.  For example:

```
mkdir -p ~/.local/share/jupyter/kernels
cp -r /cm/shared/apps/conda-environments/jupyterhub/share/jupyter/kernels/python37-conda ~/.local/share/jupyter/kernels/mykernel
# Edit ~/.local/share/jupyter/kernels/mykernel/kernel.json
# to point to the "python" executable inside your environment.
```



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

#### Slurm resource manager

To avoid conflict when accessing the compute nodes, we use the
[Slurm](https://slurm.schedmd.com) resource manager (aka "scheduler"
or "queueing system"). Slurm keeps track of which compute nodes are
currently used by who. If you want to use a certain number of compute
nodes, you have to ask Slurm, and you might have to wait until the
nodes are available before you can run your job.

The basic work flow is thus as follows:

1. You write a *batch script* (shell script) for your job. (Below are
   some examples.) This script defines which resources you want (e.g.
   "4 nodes for 7 days"), and also how to run your job.

1. You submit this script to Slurm via `sbatch` (see below for
   examples).

1. If the system is busy, your job might have to wait in the queue for
   some time. Slurm will try to be "fair" to all users (whatever that
   means). Your job's priority is determined by several factors,
   including how much you have used Symmetry recently, and how many
   resources your job requests.

1. Slurm will run your job automatically (that's what *batch* means).
   This does generally not work with notebooks (e.g. Mathematica,
   Jupyter). Instead, you need to write a script with a text editor
   (see below for examples).

1. After the job has finished, you examine its output that was
   presumably written to a file.

#### Using Slurm

After `module load slurm`, you can get an overall view of the system
with `sinfo`. This might output a description like this:

```
$ sinfo
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
defq*        up 7-00:00:00      6 drain* cn[017,019-020,040,045,056]
defq*        up 7-00:00:00      1  drain cn002
defq*        up 7-00:00:00      1  alloc cn001
defq*        up 7-00:00:00     68   idle cn[003-016,018,021-039,041-044,046-055,057-076]
debugq       up    1:00:00      6 drain* cn[017,019-020,040,045,056]
debugq       up    1:00:00      1  drain cn002
debugq       up    1:00:00      1  alloc cn001
debugq       up    1:00:00     68   idle cn[003-016,018,021-039,041-044,046-055,057-076]
```

This means there are two *partitions* (queues) available, called
`defq` (the default queue) and `debugq` (for debugging and short
interactive jobs). `defq` allows jobs to run for up to 7 days,
`debugq` for up to 1 hour. Not shown here is the fact that jobs in
`debugq` have a much higher priority and will usually start before any
jobs waiting in `defq`.

The description of the compute nodes is (unfortunately) repeated for
both queues. Nodes in the `drain` state are not available; they are
either reserved for administrative use (here `cn002`), or are
unresponsive (here `cn[017,019-020,040,045,056]`; these nodes are
presumably either being updated, or might be reporting a hardware
issue). Nodes in the `alloc` state are currently in use, and nodes in
the `idle` state are currently free.

The command `squeue` shows all jobs that are currently either waiting
or running. `squeue -u USERNAME` (replace `USERNAME` with your user
name) shows only your jobs.

#### Running jobs interactively

(JupyterHub, srun, reservations, ...)

#### Running batch jobs

Slurm comes with extensive documentation and tutorials.

When running a job on Symmetry, you need to describe how many *nodes*
and *cores* your job is requesting. Determining this correctly is not
always straightforward:

- First, you need to know whether your application can run across
  multiple nodes. Many applications cannot, because it is difficult to
  implement this. Usually you will know whether your application
  supports this. For exampe, in Mathematica you need to use *remote
  kernels* to enable this. In Fortran, C, or C++, you need to use
  `MPI` or a similar mechanism. In Julia or Python programs, you also
  need to explicitly support using multiple processes.

- You also need to know whether your application uses multiple
  threads. Even if your application does not support this explicitly,
  it might use a library that uses multi-threading. For example,
  Mathematica, Julia, or Python are not multi-threaded by default, but
  if you use linear algebra (e.g. systems with large matrices for
  floating-point numbers), then they might use multiple threads. In
  Fortran, C, or C++, you can use OpenMP for multi-threading. (Note
  that `OpenMP` and `MPI`/`OpenMPI` are very different, despite the
  very similar names.)

- If your application uses multiple nodes, then it most likely will
  use all the cores on each node efficiently. This gives the highest
  performance, but is the most difficult to implement.

- If your application use a single node but is multi-threaded, then
  you should probably run only a single program on each compute node.
  This is the easiest case.

- If your application is not multi-threaded, then it iuses only a
  single core. Each node of Symmetry has many cores (up to 40). It
  thus makes sense to run multiple copies of your application at the
  same time on the same node, if there is enough memory available.

Here are some examples that might be useful for a quick start:

**Note:** The Slurm scripts contain path names pointing into my
(`eschnetter`'s) home directory. You need to change this to point into
a directory of yours, otherwise you will not see the output.

- Running Mathematica on 1 node: A multi-threaded code (using linear
  algebra) [Mathematica script](examples/mathematica-manythreads.m)
  [Slurm script](examples/mathematica-manythreads.sbatch) [example
  output](examples/mathematica-manythreads.out)

- Running Mathematica on 1 node: A single-threaded code, running
  several independent Mathematica scripts simultaneously (e.g. a
  parameter scan) [Mathematica script](examples/mathematica-onetask.m)
  [Slurm script](examples/mathematica-manytasks.sbatch) [example
  output](examples/mathematica-manytasks.out)

- Running Julia on several nodes: A multi-processing code (using
  Julia's built-in multi-processing capabilities) [Julia
  script](examples/julia-manytasks.jl) [Slurm
  script](examples/julia-manytasks.sbatch) [example
  output](examples/julia-manytasks.out)

- Running Julia on 1 node: A multi-threaded code (using linear
  algebra) [Julia script](examples/julia-manythreads.jl) [Slurm
  script](examples/julia-manythreads.sbatch) [example
  output](examples/julia-manythreads.out)

- Running a C program on several nodes: A multi-processing MPI code [C
  code](examples/hello-mpi.c) [build
  instructions](examples/hello-mpi.build) [Slurm
  script](examples/hello-mpi.sbatch) [example
  output](examples/hello-mpi.out)

- Running a C program on several nodes: Ahybrid multi-processing
  multi-threaded MPI/OpenMP code [C code](examples/hello-hybrid.c)
  [build instructions](examples/hello-hybrid.build) [Slurm
  script](examples/hello-hybrid.sbatch) [example
  output](examples/hello-hybrid.out)

- Running a C program on 1 node: A multi-threaded OpenMP code [C
  code](examples/hello-openmp.c) [build
  instructions](examples/hello-openmp.build) [Slurm
  script](examples/hello-openmp.sbatch) [example
  output](examples/hello-openmp.out)



### File systems

(home directory, GPFS)
