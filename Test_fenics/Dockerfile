# System testing preCICE bindings

# Building on top of the local preCICE-build with FEniCS
ARG from=precice_fenics
FROM $from

USER root
ENV USER=root

# Installing necessary dependecies
RUN apt-get -qq update && apt-get -qq install \
    python-pip python-enum34 python3-pip
    
# Installing necessary python dependecies
RUN pip2 install --upgrade pip
RUN pip2 install Cython mpi4py
RUN pip3 install --upgrade pip
RUN pip3 install Cython mpi4py numpy

# Rebuild image if force_rebuild after that command
ARG CACHEBUST

# pulls the last version of the source code
WORKDIR $PRECICE_ROOT
RUN git pull

# Builds the precice python binding for python2
WORKDIR $PRECICE_ROOT/src/precice/bindings/python
ENV PRECICE_MPI_IMPLEMENTATION=openmpi
RUN python setup.py install

# Builds the precice python binding for python3
WORKDIR $PRECICE_ROOT/src/precice/bindings/python
ENV PRECICE_MPI_IMPLEMENTATION=openmpi
RUN python3 setup.py install

# clone fenics adapter repository
# using $HOME instead of /home/fenics would be better, but does not work. Why?
WORKDIR /home/fenics
RUN git clone https://github.com/precice/fenics-adapter.git

# install fenics adapter
WORKDIR fenics-adapter
RUN python3 setup.py install
RUN python2 setup.py install
