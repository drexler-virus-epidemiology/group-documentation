# Beagle & MrBayes

## What and why?

* MrBayes

## Install Beagle

First install all dependencies. These include:

* NVIDIA Drivers: `nvidia`
* CUDA: `cuda`, `cuda-tools`
* Build tools: `gcc-11`, `cmake`, `autoconf`, `automake`, `libtool`,
  `subversion`, `pkg-config`
* Java Development Kit: `jre11-openjdk`

Follow the instructions from the [official README](https://github.com/beagle-dev/beagle-lib/wiki/LinuxInstallInstructions).

```bash
# Find a path to where to install the software
export INSTALL_DIR='<PATH/TO/INSTALL/LOCATION>'

# Compilation process
cd $INSTALL_DIR
git clone --depth=1 https://github.com/beagle-dev/beagle-lib.git
cd beagle-lib
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX:PATH=$INSTALL_DIR/beagle-lib/ \
    -DBUILD_JNI=ON \
    -DBUILD_CUDA=ON \
    -DBUILD_OPENCL=OFF \
    -DCMAKE_C_COMPILER=gcc-11 \
    -DCMAKE_CXX_COMPILER=g++-11 \
    ..
make install
```

Since Beagle was installed in a non-default location, two environment variables need to be persistently set for it to be recognized by other programs. Add this to your `~/.bashrc`.

```bash
export LD_LIBRARY_PATH=$INSTALL_DIR/beagle-lib/lib:$LD_LIBRARY_PATH
export PKG_CONFIG_PATH=$INSTALL_DIR/beagle-lib/lib/pkg-config:$PKG_CONFIG_PATH
```

## Install MrBayes

Follow the instructions from the [official
README](https://github.com/NBISweden/MrBayes/blob/develop/INSTALL). When
running MrBayes on the HPC cluster, it might be useful to create two binaries,
one with mpi support and one without. The dependencies are similar to Beagle,
so no additional libraries need to be installed. However, it is advisable to
check whether everything is installed.

```
# Check dependencies
nvcc --version
gcc-11 --version
export | grep CUDA_VISIBLE_DEVICES # should return '0' for one GPU

# Go to the installation directory
cd $INSTALL_DIR

# Set CC variable to local GCC at
export CC=/opt/local/gcc-7.2.0-0/bin/

# Compilation process
git clone --depth=1 https://github.com/NBISweden/MrBayes.git
cd MrBayes
mkdir build
cd build
../configure \
    --with-beagle \
    --with-mpi \ # REMOVE this flag when 
    --prefix=$INSTALL_DIR/MrBayes
make
make install
```

This will create a binary `mb` in `$INSTALL_DIR/MrBayes/bin` that can be executed
directly (without MPI support) or using SLURM (with MPI support). In order to
compile two versions of MrBayes, rename the original binary (e. g. `mb-mpi`)
remove the `--with-mpi` flag and reconfigure and repeat `make install` to
create a new `mb` binary in the same directory.

## Installation on the BIH HPC cluster

Beagle and MrBayes should be installed on the HPC cluster in the group's work
directory at `/fast/scratch/groups/ag-drexler/02_software`. It can be installed
the same way as described above, but most dependencies should be installed
using a `conda` environment. The installation process is much more fragile due
user's inability to install libraries into standard locations. So the current
way to make it work is to install cuda and all dependencies, Java as well as
libtool and subversion.

* libtool, subversion 
* cuda, cuda-tools (`-c nvidia`)
* openjdk=11

```
# Start a shell on GPU partition 
srun --time=01-01 --mem=32G --ntasks=1 -p gpu --gres=gpu:tesla:1  --pty bash -i

# Install dependencies
conda create -n mrbayes -c nvidia cuda-nvcc cuda-toolkit 
conda activate mrbayes
conda install openjdk=11 libtool subversion
module load cmake gcc
```

First install Beagle in a location set using 

```
export INSTALL_DIR='<PATH/TO/INSTALL/LOCATION>'
cd $INSTALL_DIR
git clone --depth=1 https://github.com/beagle-dev/beagle-lib.git
cd beagle-lib
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX:PATH=$INSTALL_DIR/beagle-lib/build \
     -DBUILD_JNI=ON \
     -DBUILD_CUDA=ON \
     -DBUILD_OPENCL=OFF \
     -DCMAKE_C_COMPILER=/opt/local/gcc-7.2.0-0/bin/gcc \
     -DCMAKE_CXX_COMPILER=/opt/local/gcc-7.2.0-0/bin/g++ \
    ..
make install
```

To install MrBayes...

```
export CC=/opt/local/gcc-7.2.0-0/bin/
git clone --depth=1 https://github.com/NBISweden/MrBayes.git
cd $INSTALL_DIR
cd MrBayes
mkdir build
cd build
../configure \
    --with-beagle \
    --enable-doc=no \
    --prefix=$INSTALL_DIR/MrBayes/build
make
make install

```

## Check Installation 

```
nvidia-smi

# In MrBayes 
$INSTALL_DIR/MrBayes/bin/mb
> showbeagle
> help set
```

## References

* The official MrBayes manual can be found [here](https://mrbayes.sourceforge.net/mb3.2_manual.pdf)
* HPC documentation about GPU nodes can be found [here](https://bihealth.github.io/bih-cluster/how-to/connect/gpu-nodes/)
