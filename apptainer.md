# Singularity

[Singularity](https://www.sylabs.io/singularity/) is a program for running *containers*.
It is similar to [Docker](https://www.docker.com/), but is specially designed for High-Performance Computing environments.
For security reasons, we cannot provide Docker on the Symmetry cluster.  However, we do provide Singularity, and Singularity
can be used to run Docker containers.

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
Useful starting points include:
 * [Overview of the "singularity" command](https://www.sylabs.io/guides/3.0/user-guide/quick_start.html#overview-of-the-singularity-interface)
 

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
bash: module: command not found            ### this is printed because I have "module load" statements in my bash startup files
                                           ### and the container does not have the "module" command
bash-4.2$ hostname                         ### The container can have its own hostname
centos
bash-4.2$ cat /etc/redhat-release          ### The container looks like a RedHat distribution (Symmetry uses Ubuntu)
CentOS Linux release 7.6.1810 (Core)       ### You could install RPM packages in this container
```

Within the container (unlike Docker), you keep your username, and you can create files within your home directory and your `/gpfs` directory.  The `/cm` shared directory is also available.  *However*, you don't have `root` power within your container (also unlike Docker).

```
dlang@mn001:~$ singularity run --hostname centos centos-7.sif
bash: module: command not found
bash-4.2$ whoami
dlang
bash-4.2$ pwd
/home/dlang
bash-4.2$ touch hello.txt               ### I can create files in my home directory
bash-4.2$ ls -d /cm/shared
/cm/shared
bash-4.2$ ls -d /gpfs/`whoami`          ### My /gpfs directory is visible (and writable)
/gpfs/dlang
bash-4.2$ touch /gpfs/`whoami`/hello.txt
bash-4.2$ yum install python            ### I don't have root power within the container!
Loaded plugins: fastestmirror, ovl
ovl: Error while doing RPMdb copy-up:
[Errno 30] Read-only file system: '/var/lib/rpm/.dbenv.lock'
You need to be root to perform this command.
```

## Workflow

Because of the security limitations on running Singularity on Symmetry, it's actually easier to developer your *container*
on your own computer using *Docker*, and then use the *Docker Hub* to copy the container's files from your computer to
Symmetry.

To start, go to <https://hub.docker.com/> and create an account.

Install Docker Desktop on your local computer <https://www.docker.com/products/docker-desktop>

Log in to your Docker Hub account:
```
> docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username:
Password:
```

Next, start developing your Dockerfile.  Here is an [example Dockerfile](examples/docker-multinest/Dockerfile)
that builds the *MultiNest* Bayesian sampling library.  The first few commands are shown here:

```
# Start from a modern Ubuntu
FROM ubuntu:18.04

# Install OS packages
RUN apt -y update && \
    apt install -y --no-install-recommends \
            cmake git g++ ca-certificates make gfortran \
            libopenblas-dev mpi-default-dev mpi-default-bin ssh \
            python3-minimal python3-dev python3-pip && \
    apt clean

# Install Python packages for pymultinest
RUN pip3 install setuptools wheel && \
    pip3 install numpy scipy matplotlib mpi4py

# Install github 'master' version of MultiNest
RUN mkdir -p /src && \
    cd /src && \
    git clone https://github.com/JohannesBuchner/MultiNest.git multinest && \
    cd multinest/build && \
    cmake .. && \
    make && \
    make install && \
    make clean
```

Build this Dockerfile using the `docker build` command:

```
mkdir docker-multinest
cd docker-multinest
# Put Dockerfile here...
docker build .
```

You can also *tag* the build with a *repository* name.  My Docker Hub username is *dstndstn*; replace the commands below with
your own user id.

```
docker build -t dstndstn/pymultinest
```

This build will take a while!  When it completes, you can *push* it to Docker Hub.  This will upload the container files, so
can take a while also.

```
docker push dstndstn/pymultinest
```

There, now the Docker image is on Docker Hub.  We can finally return to Symmetry and load it into Singularity:

```
module load singularity
singularity build pymultinest.sif docker://dstndstn/pymultinest
singularity run --hostname sing pymultinest.sif
```

This will pop you into the container (you can tell because your prompt will show the hostname as *sing*):

```
dlang@mn001:~$ singularity run --hostname sing pymultinest.sif
bash: module: command not found
dlang@sing:~$ cd /tmp
dlang@sing:/tmp$ mpiexec -np 4 python3 /src/pymultinest/pymultinest_demo.py
 *****************************************************
 MultiNest v3.10
.....
```


