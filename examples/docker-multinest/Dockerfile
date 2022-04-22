# Start from Ubuntu
FROM ubuntu:22.04

# Install OS packages
ENV DEBIAN_FRONTEND=noninteractive
RUN apt -y update && \
    apt install -y --no-install-recommends \
            cmake git g++ ca-certificates less make gfortran \
            libopenblas-dev mpi-default-dev mpi-default-bin ssh \
            python3-minimal python3-dev python3-pip wget && \
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

# Install github 'master' version of PyMultiNest
RUN cd /src && \
    git clone https://github.com/JohannesBuchner/PyMultiNest.git pymultinest && \
    cd pymultinest && \
    python3 setup.py install

RUN cd /src && wget http://www.feynarts.de/cuba/Cuba-4.2.2.tar.gz && \
    tar xzf Cuba-4.2.2.tar.gz && \
    mv Cuba-4.2.2 cuba && \
    cd cuba && ./configure && \
    wget https://raw.githubusercontent.com/JohannesBuchner/cuba/master/makesharedlib.sh && \
    chmod 755 ./makesharedlib.sh && ./makesharedlib.sh

# Per https://github.com/JohannesBuchner/cuba/issues/5,
# I instead grabbed the official Cuba tarball and the "makesharedlib.sh" script, above.
# Old version:
# # Install github 'master' version of Cuba
# RUN cd /src && \
#     git clone https://github.com/JohannesBuchner/cuba cuba && \
#     cd cuba && \
#     ./configure && \
#     # https://github.com/JohannesBuchner/cuba/issues/1
#     sed -i "s|-o libcuba.so|-lm -o libcuba.so|g" makesharedlib.sh && \
#     ./makesharedlib.sh && \
#     cp libcuba.so /usr/local/lib && \
#     # Apparently needed unless LD_LIBRARY_PATH is set to include /usr/local/lib
#     ln -s /usr/local/lib/libcuba.so /usr/lib/ && \
#     ln -s /usr/local/lib/libmultinest.so /usr/lib/ && \
#     ln -s /usr/local/lib/libmultinest_mpi.so /usr/lib/

RUN cd /src/cuba && \
    cp libcuba.so /usr/local/lib && \
    # Apparently needed unless LD_LIBRARY_PATH is set to include /usr/local/lib
    ln -s /usr/local/lib/libcuba.so /usr/lib/ && \
    ln -s /usr/local/lib/libmultinest.so /usr/lib/ && \
    ln -s /usr/local/lib/libmultinest_mpi.so /usr/lib/

# https://github.com/open-mpi/ompi/issues/7701#issuecomment-932587086
ENV HWLOC_COMPONENTS=-g

# These are a bunch of tests -- not really required for the Docker container, but useful as a check
# this the build worked.
RUN python3 -c "import pymultinest" && \
    python3 -c "import pycuba" && \
    cd src/pymultinest && \
    mpiexec --allow-run-as-root -np 4 python3 pymultinest_demo.py && \
    python3 pymultinest_demo.py && \
    python3 multinest_marginals.py chains/3- && \
    python3 pycuba_demo.py
