### Working way to compile gromacs with plumed on LomonosovII

#### Define folders
```
prefix_plumed_dir=/home/kniazevaanastasiia2015_1609/_scratch/_software/plumed/plumed_2.5
prefix_gromacs_dir=/home/kniazevaanastasiia2015_1609/_scratch/_software/gromacs/2018.4_plumed
module_dir=/home/kniazevaanastasiia2015_1609/_scratch/_software/modules

module load openmpi/1.8.4-gcc cuda/8.0 gcc/5.5.0
```
#### Plumed compilation
```
./configure --prefix=$prefix_plumed_dir CXX=mpicxx CC=mpicc
make -j4
make install
```
##### Add Plumed module
`cp $prefix_plumed_dir/lib/plumed/modulefile $module_dir/plumed_2.5_gcc`

#### Compile gromacs 
```
module load plumed_2.5_gcc cmake/3.12.3 openmpi/1.8.4-gcc cuda/8.0 gcc/5.5.0

plumed patch -p
# Select gromacs 2018.4

mkdir build_plumed
cd build_plumed
cmake .. -DGMX_BUILD_OWN_FFTW=ON -DCMAKE_C_COMPILER=mpicc -DCMAKE_CXX_COMPILER=mpicxx -DGMX_MPI=on -DGMX_GPU=on \
         -DCMAKE_INSTALL_PREFIX=$prefix_gromacs_dir -DGMX_DEFAULT_SUFFIX=OFF -DGMX_BINARY_SUFFIX=_plumed \
         -DGMX_LIBS_SUFFIX=_plumed
         
make -j 4
make install
```
##### Add gromacs module
Add file to `$module_dir`
```
#%ModuleTcl####################################################################
##
##  gromacs-2018 modulefile
##
##

proc ModulesHelp { } {
        global version

        puts stderr "\tThis module will set up environment for gromacs-2018"
        puts stderr "\n\tVersion $version\n"
}

# for Tcl script use only
set     version      2018.4

module add gcc/5.5.0
module add openmpi/1.8.4-gcc
module add cuda/8.0
module add plumed_2.5_gcc
append-path PATH /home/kniazevaanastasiia2015_1609/_scratch/_software/gromacs/2018.4_plumed/bin
``` 
