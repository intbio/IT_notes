Instuctions to compile VMD for Mac

0) don't forget to install XQuartz and check opt/include/X11
0) check that gcc is os x native one
0) install cuda driver and cuda toolkit
1) download vmd-1.9.1, unpack
2) cd vmd-1.9.1/libs
3) cd fltk, download fltk-1.3.2, follow instuctions in README provided with VMD, while compiling fltk, you can use -arch i386 and x86_64 to compile for both platforms simultaneously
4) in vmd-1.9.1/libs, mkdir tcltk, cd, download tcl8.5.15 and tk8.5.15.
5) in macosx subdirs, run ./configure --enable-framework --enable-64 
6)
cd ~/tmp/vmdsrc/vmd-1.9.1/plugins
export TCLINC=”-I/Users/jbardhan/tmp/vmdsrc/tcltk/include”
export TCLLIB=”-L/Users/jbardhan/tmp/vmdsrc/tcltk/lib”
make MACOSXX86_64 TCLINC=$TCLINC TCLLIB=$TCLLIB
export PLUGINDIR=/Users/jbardhan/tmp/vmdsrc/vmd-1.9.1/plugins
make distrib

7) edit configure.options to read
MACOSXX86_64 FLTKOPENGL FLTK TK TDCONNEXION LIBTACHYON NETCDF TCL PYTHON PTHREADS NUMPY ACTC GCC

CUDA - is optional


edit configure for the final destination of your VMD scripts and files:
$install_name = “vmdpy”;
$install_bin_dir=”/Users/jbardhan/tmp/bin”;
$install_library_dir=”/Users/jbardhan/tmp/lib/$install_name”;

./configure

8) cd to vmd-1.9.1/libs
check this http://www.ks.uiuc.edu/Research/vmd/doxygen/extprogs.html#stride
just copy stride, tachyon, surf from binary distrib

9) get vmdmac.r from forum site to src dir. somehow it is absent there

9) cd src, open Makefile and edit
delete -m32 option
add lib paths

change framework to Cocoa from Carbon


===========

INCDIRS     =  -fpascal-strings -I../lib/tachyon/include -F/System/Library/Frameworks   -I../lib/tcl/include -I../lib/tk/lib_MACOSXX86_64/Tk.framework/Versions/8.5/Headers -I../plugins/include -I../plugins/MACOSXX86_64/molfile -I../lib/netcdf/include -I../lib/fltk/fltk -I. -F/opt/local/Library/Frameworks/Python.framework -I/opt/local/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages/numpy/core/include -I/opt/X11/include

LIBS        = -lfltk_gl -framework OpenGL -framework AGL  -ltachyon -framework Python -lpthread -lpthread  -framework Tk -framework Tcl  -lmolfile_plugin -lnetcdf -lfltk  -framework Cocoa  $(VMDEXTRALIBS)

LIBDIRS     =  -L/opt/local/lib -weak_framework 3DconnexionClient -L../lib/tachyon/lib_MACOSXX86_64    -F../lib/tcl/lib_MACOSXX86_64 -F../lib/tk/lib_MACOSXX86_64 -Wl,-executable_path . -lmx  -L../plugins/MACOSXX86_64/molfile -L../lib/netcdf/lib_MACOSXX86_64 -L../lib/fltk/MACOSXX86_64 

DEFINES     = -DVMDOPENGL -DVMDFLTKOPENGL -DVMDTDCONNEXION -DVMDLIBTACHYON -DVMDPYTHON -DVMDTHREADS -DWKFTHREADS -DUSEPOSIXTHREADS -D_REENTRANT -DVMDNUMPY -DVMDQUICKSURF -DVMDWITHCARBS -DVMDPOLYHEDRA -DVMDSURF -DVMDMSMS -DVMDPBCSMOOTH -DVMDTCL -DVMDTK  -DVMDSTATICPLUGINS  -DVMDGUI -DVMDFLTK 

# compiler and compiler directives 
CC          = cc
CFLAGS      = -fPIC -Os -ffast-math -DARCH_MACOSXX86_64 $(DEFINES) $(INCDIRS) 

CCPP        = c++
CPPFLAGS    = -fPIC -Os -ffast-math  -DARCH_MACOSXX86_64 $(DEFINES) $(INCDIRS) 

NVCC        = /usr/local/encap/cuda-4.0/bin/nvcc
NVCCFLAGS   = --ptxas-options=-v -gencode arch=compute_10,code=sm_10 -gencode arch=compute_13,code=sm_13 -gencode arch=compute_20,code=sm_20 --ftz=true  -DARCH_MACOSXX86_64 $(DEFINES) $(INCDIRS)

COMPILERC   = /usr/bin/Rez
RCFLAGS     = -t APPL -o ../MACOSXX86_64/vmd_MACOSXX86_64 vmdmac.r
===========

=====Alternative with CUDA ==
# Turn things into objects
VMD_OBJS    =   $(VMD_CCPP:.C=.o) $(VMD_CC:.c=.o) $(VMD_CU:.cu=.o)

INCDIRS     =  -I../lib/actc/include  -I../lib/tachyon/include -L/System/Library/Frameworks   -I../lib/tcl/include -I../lib/tk/lib_MACOSXX86_64/Tk.framework/Versions/8.5/Headers -I../plugins/include -I../plugins/MACOSXX86_64/molfile -I../lib/netcdf/include -I../lib/fltk/fltk -I. -L/opt/local/Library/Frameworks/Python.framework -I/opt/local/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages/numpy/core/include -I/opt/X11/include

LIBS        = -lfltk_gl -framework OpenGL -framework AGL -lactc -Wl,-rpath -Wl,$$ORIGIN/ -lcudart  -ltachyon -framework Python -lpthread -lpthread  -framework Tk -framework Tcl  -lmolfile_plugin -lnetcdf -lfltk  -framework Cocoa  $(VMDEXTRALIBS)

LIBDIRS     =  -L/opt/local/lib -L../lib/actc/lib_MACOSXX86_64 -L/usr/local/cuda/lib -weak_framework 3DconnexionClient -L../lib/tachyon/lib_MACOSXX86_64    -L../lib/tcl/lib_MACOSXX86_64 -L../lib/tk/lib_MACOSXX86_64 -Wl,-executable_path . -lmx  -L../plugins/MACOSXX86_64/molfile -L../lib/netcdf/lib_MACOSXX86_64 -L../lib/fltk/MACOSXX86_64 -L/opt/local/Library/Frameworks/Python.framework

DEFINES     = -DVMDOPENGL -DVMDFLTKOPENGL -DVMDACTC -DVMDCUDA -DMSMPOT_CUDA -DVMDTDCONNEXION -DVMDLIBTACHYON -DVMDPYTHON -DVMDTHREADS -DWKFTHREADS -DUSEPOSIXTHREADS -D_REENTRANT -DVMDNUMPY -DVMDQUICKSURF -DVMDWITHCARBS -DVMDPOLYHEDRA -DVMDSURF -DVMDMSMS -DVMDPBCSMOOTH -DVMDTCL -DVMDTK  -DVMDSTATICPLUGINS  -DVMDGUI -DVMDFLTK 

# compiler and compiler directives 
CC          = cc
CFLAGS      =  -fPIC -Os -ffast-math -DARCH_MACOSXX86_64 $(DEFINES) $(INCDIRS) 

CCPP        = c++
CPPFLAGS    =  -fPIC -Os -ffast-math  -DARCH_MACOSXX86_64 $(DEFINES) $(INCDIRS) 

NVCC        = /usr/local/cuda/bin/nvcc
NVCCFLAGS   = --ptxas-options=-v -gencode arch=compute_10,code=sm_10 -gencode arch=compute_13,code=sm_13 -gencode arch=compute_20,code=sm_20 --ftz=true  --machine 64 -O3  -DARCH_MACOSXX86_64 $(DEFINES) $(INCDIRS)

COMPILERC   = /usr/bin/Rez
RCFLAGS     = -t APPL -o ../MACOSXX86_64/vmd_MACOSXX86_64 vmdmac.r


=============this is the best version that linked against macport python2.7

INCDIRS     =  -I../lib/actc/include -fpascal-strings -I../lib/tachyon/include -F/opt/Local/Library/Frameworks   -I../lib/tcl/include -I../lib/tk/lib_MACOSXX86_64/Tk.framework/Versions/8.5/Headers -I../plugins/include -I../plugins/MACOSXX86_64/molfile -I../lib/netcdf/include -I../lib/fltk/fltk -I. -F/opt/local -I/opt/local/Library/Frameworks/Python.framework/Versions/2.7/Headers -I/opt/local/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages/numpy/core/include -I/opt/X11/include

LIBS        = -lfltk_gl  -lactc -framework OpenGL -framework AGL  -ltachyon -lpython2.7 -lpthread -lpthread  -framework Tk -framework Tcl  -lmolfile_plugin -lnetcdf -lfltk  -framework Cocoa  $(VMDEXTRALIBS)

LIBDIRS     =  -L/opt/local/lib -L../lib/actc/lib_MACOSXX86_64 -weak_framework 3DconnexionClient -L../lib/tachyon/lib_MACOSXX86_64    -F../lib/tcl/lib_MACOSXX86_64 -F../lib/tk/lib_MACOSXX86_64 -Wl,-executable_path . -lmx  -L../plugins/MACOSXX86_64/molfile -L../lib/netcdf/lib_MACOSXX86_64 -L../lib/fltk/MACOSXX86_64 -F/opt/local/Library/Frameworks -F/opt/local/Library/Frameworks/Python.framework/Versions/2.7/ -F/opt/local/Library/Frameworks/Python.framework/Versions/2.7/lib/

DEFINES     = -DVMDOPENGL -DVMDFLTKOPENGL -DVMDTDCONNEXION -DVMDLIBTACHYON -DVMDPYTHON -DVMDTHREADS -DWKFTHREADS -DUSEPOSIXTHREADS -D_REENTRANT -DVMDNUMPY -DVMDQUICKSURF -DVMDWITHCARBS -DVMDPOLYHEDRA -DVMDSURF -DVMDMSMS -DVMDPBCSMOOTH -DVMDTCL -DVMDTK  -DVMDSTATICPLUGINS  -DVMDGUI -DVMDFLTK 

# compiler and compiler directives 
CC          = cc
CFLAGS      = -fPIC -Os -ffast-math -DARCH_MACOSXX86_64 $(DEFINES) $(INCDIRS) 

CCPP        = c++
CPPFLAGS    = -fPIC -Os -ffast-math  -DARCH_MACOSXX86_64 $(DEFINES) $(INCDIRS) 

NVCC        = /usr/local/encap/cuda-4.0/bin/nvcc
NVCCFLAGS   = --ptxas-options=-v -gencode arch=compute_10,code=sm_10 -gencode arch=compute_13,code=sm_13 -gencode arch=compute_20,code=sm_20 --ftz=true  -DARCH_MACOSXX86_64 $(DEFINES) $(INCDIRS)

COMPILERC   = /usr/bin/Rez
RCFLAGS     = -t APPL -o ../MACOSXX86_64/vmd_MACOSXX86_64 vmdmac.r



==========

10) make install
11)
Edit the script install_name (here vmdpy) which has been copied to your
install_bin directory. Change MACOSX to MACOSXX86_64.




