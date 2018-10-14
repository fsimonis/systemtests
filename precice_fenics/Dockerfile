# Dockerfile for building preCICE on ubuntu 18.04

# Use fenicsproject as baseimage
FROM quay.io/fenicsproject/stable:latest

# Installing necessary dependacies for preCICE
RUN apt-get -qq update && apt-get -qq install \
    build-essential \
    scons \
    libeigen3-dev \
    libxml2-dev \
    petsc-dev \
    libboost-all-dev \
    python-dev \
    python-numpy \
    git \       
    wget \
    bzip2

# Rebuild image if force_rebuild after that command
ARG CACHEBUST

# Setting some environment variables for installing preCICE
# Eigen
ENV CPLUS_INCLUDE_PATH="$CPLUS_INCLUDE_PATH:/usr/include/eigen3"
ENV CPATH="/usr/include/eigen3:${CPATH}"
# PETSc
ENV PETSC_DIR="/usr/lib/petscdir/3.7.7/"
ENV LD_LIBRARY_PATH="$PETSC_DIR/lib:${LD_LIBRARY_PATH}"
ENV CPATH="$PETSC_DIR/include:${CPATH}"
ENV LIBRARY_PATH="$PETSC_DIR/lib:${LIBRARY_PATH}"
ENV PYTHONPATH="$PETSC_DIR/lib:${PYTHONPATH}"
ENV PETSC_ARCH="x86_64-linux-gnu-real"

# Building preCICE
ARG branch=develop
RUN git clone --branch $branch https://github.com/precice/precice.git
WORKDIR precice
# Some parameters for the build, you can set them in the build command e.g.
# sudo docker build Dockerfile.precice --build-arg petsc_para=yes --build-arg mpi_para=yes .
# this will result in
# scons petsc=yes mpi=yes python=no compiler="mpicxx" -j2
ARG petsc_para=no
ARG mpi_para=yes
ARG python_para=no
RUN scons petsc=$petsc_para mpi=$mpi_para python=$python_para -j$(nproc)

# Setting preCICE environment variables
RUN pwd
# using $HOME instead of /home/fenics would be better, but does not work. Why?
ENV PRECICE_ROOT /home/fenics/precice
ENV LD_LIBRARY_PATH $PRECICE_ROOT/build/last:${LD_LIBRARY_PATH}
ENV LIBRARY_PATH $PRECICE_ROOT/build/last:${LIBRARY_PATH}
