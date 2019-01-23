FROM ubuntu:18.04

RUN apt -y update && \
    apt install -y --no-install-recommends \
            cmake git g++ ca-certificates make gfortran \
            libopenblas-dev mpi-default-dev mpi-default-bin ssh \
            python3-minimal python3-dev python3-pip && \
    apt clean

RUN pip3 install setuptools wheel && \
    pip3 install numpy scipy matplotlib mpi4py

RUN mkdir -p /src && \
    cd /src && \
    git clone https://github.com/JohannesBuchner/MultiNest.git multinest && \
    cd multinest/build && \
    cmake .. && \
    make && \
    make install && \
    make clean

RUN cd /src && \
    git clone https://github.com/JohannesBuchner/PyMultiNest.git pymultinest && \
    cd pymultinest && \
    python3 setup.py install

RUN cd /src && \
    git clone https://github.com/JohannesBuchner/cuba cuba && \
    cd cuba && \
    ./configure && \
    # https://github.com/JohannesBuchner/cuba/issues/1
    sed -i "s|-o libcuba.so|-lm -o libcuba.so|g" makesharedlib.sh && \
    ./makesharedlib.sh && \
    cp libcuba.so /usr/local/lib && \
    # Apparently needed unless LD_LIBRARY_PATH is set to include /usr/local/lib
    ln -s /usr/local/lib/libcuba.so /usr/lib/ && \
    ln -s /usr/local/lib/libmultinest.so /usr/lib/ && \
    ln -s /usr/local/lib/libmultinest_mpi.so /usr/lib/

RUN python3 -c "import pymultinest" && \
    python3 -c "import pycuba" && \
    cd src/pymultinest && \
    mpiexec --allow-run-as-root -np 4 python3 pymultinest_demo.py && \
    python3 pymultinest_demo.py && \
    python3 multinest_marginals.py chains/3- && \
    python3 pycuba_demo.py
