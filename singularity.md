# Singularity

[Singularity](https://www.sylabs.io/singularity/) is a program for running *containers*.
It is similar to [Docker](https://www.docker.com/), but is specially designed for High-Performance Computing environments.
For security reasons, we cannot provide Docker on the Symmetry cluster.  However, we do provide Singularity.

*Containers* are a way of creating a self-contained filesystem that includes all the dependencies, code, and data files
needed by your application.  This allows it to run in different environments (eg, on your laptop, or on Symmetry head nodes
or compute nodes), without having to worry about installing a bunch of libraries or building the code from scratch.
When you run a program inside a container, that program will see a "fake" root filesystem, different than the real filesystem
on the Symmetry cluster.  You can have root permissions within that "fake" filesystem, which means you can install whatever
packages you want.  You can even use a different Linux distribution (for example, CentOS) than the Ubuntu we have installed on
Symmetry.  This is one of the main use cases for using containers for HPC work -- you can install all the dependencies you
need, compile it once, and have it work on different systems.

## Getting Started

The [Singularity Documentation](https://www.sylabs.io/guides/3.0/user-guide/index.html) is quite good, though generally it
assumes you have *root* powers (on Symmetry, you will not!).

To use Singularity on Symmetry, first load the module:
```
module load singularity
```
This will add the `singularity` program to your path.

Singularity uses its own format for container files (.sif).  These can be built from Docker-hub repositories:
```
singularity build centos-7.sif docker://centos:7
```
will create the file `centos-7.sif` in your current directory, containing a basic CentOS 7 install.

To run (a shell) within this container, first ensure that your home directory is readable by all:
```
chmod 755 $HOME
```
then:
```
singularity run --hostname centos centos-7.sif
```

You should see your shell prompt change, showing that you are running within the container:
```
dlang@mn001:~$ singularity run --hostname centos centos-7.sif
bash: module: command not found
bash-4.2$ hostname
centos
bash-4.2$ cat /etc/redhat-release
CentOS Linux release 7.6.1810 (Core)
```





dlang@mn001:~$ singularity run --hostname container ./ubuntu-18.04.sif
bash: module: command not found
dlang@container:~$
