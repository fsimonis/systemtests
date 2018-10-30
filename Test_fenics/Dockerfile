# System testing preCICE fenics adapter

# Building on top of the local preCICE-build with FEniCS
ARG from=precice_bindings
FROM $from

USER root
ENV USER=root

# Installing necessary dependecies
RUN apt-get -qq update && apt-get -qq install \
    python3-pip
    
# Installing necessary python dependecies
RUN pip3 install --upgrade pip
RUN pip3 install Cython mpi4py numpy

# pulls the last version of the source code
WORKDIR $PRECICE_ROOT
RUN git pull

# clone fenics adapter repository
# using $HOME instead of /home/fenics would be better, but does not work. Why?
WORKDIR /home/fenics
RUN git clone https://github.com/precice/fenics-adapter.git

# install fenics adapter
WORKDIR fenics-adapter
RUN python3 setup.py install

# install fenics
RUN apt-get install software-properties-common
RUN add-apt-repository ppa:fenics-packages/fenics
RUN apt-get update
RUN apt-get install --no-install-recommends fenics

# run test case fenics-fenics
WORKDIR /fenics-adapter/tutorials/HT/partitioned-heat/fenics-fenics
RUN python3 heat.py precice-config.xml -d & python3 heat.py precice-config.xml -n
