
# How to install CDO (Climate Data Operator) in Intel Anaconda enviroment

Author: Carlos Felipe C. Alves
email: carlos.fas@gmail.com


## Brief Description

This guide shows how to install CDO (Climate Data Operator) in Intel Anaconda enviroment.

1. Check if the channels precedence (intel,conda-forge,deafults )
2. Install the CDO without impose conda-forge

## Note

If you use the standar install on intel anaconda env one package will be downgraded and CDO will not work. As indicated bellow.

~~~
(idp_bmap) carlos@carlos-300E5M-300E5L:~$ conda install cdo -c conda-forge

The following packages will be DOWNGRADED:

    proj4:       5.2.0-h470a237_0    --> 4.9.3-h470a237_8

...

(idp_bmap) $ conda install python-cdo -c conda-forge

(idp_bmap) $ cdo -V
cdo: error while loading shared libraries: libiconv.so.2: cannot open shared object file: No such file or directory
~~~

### Roll Back if something goes wrong

~~~
conda list --revisions

2018-11-05 16:05:38  (rev 4)
    +bokeh-0.13.0 (conda-forge)
    +click-7.0 (conda-forge)
    +cytoolz-0.9.0.1 (conda-forge)
    +dask-0.20.0 (conda-forge)
    +dask-core-0.20.0 (conda-forge)
    +distributed-1.24.0 (conda-forge)
    +h5netcdf-0.6.2 (conda-forge)
    +h5py-2.8.0 (conda-forge)
    +heapdict-1.0.0 (conda-forge)
    +locket-0.2.0 (conda-forge)
    +msgpack-python-0.5.6 (conda-forge)
    +partd-0.3.9 (conda-forge)
    +sortedcontainers-2.0.5 (conda-forge)
    +tblib-1.3.2 (conda-forge)
    +toolz-0.9.0 (conda-forge)
    +xarray-0.10.9 (conda-forge)
    +zict-0.1.3 (conda-forge)

2018-11-07 05:14:07  (rev 5)
     proj4  {5.2.0 (conda-forge) -> 4.9.3 (conda-forge)}
    +cdo-1.9.4 (conda-forge)
    +eccodes-2.8.2 (conda-forge)
    +fftw-3.3.8 (conda-forge)
    +jasper-1.900.1 (conda-forge)
    +libgfortran-3.0.0 (conda-forge)
    +udunits2-2.2.27.6 (conda-forge)
    +util-linux-2.21

2018-11-07 05:15:46  (rev 6)
    +python-cdo-1.4.0 (conda-forge)

~~~

## 1. Check if the channels precedence (intel,conda-forge,deafults )


~~~
(idp_bmap) $ conda install --revision 4

conda config --set show_channel_urls yes

conda create -n idp3 intelpython3_core pandas jupyter notebook scikit-learn scipy python=3.6.5 -c intel
conda create --name idp3_cdo --clone idp3
source activate idp3_cdo
conda config --show

channels:
  - conda-forge
  - defaults
~~~
In this installation we don't have intel, channel so, if you have the same result, please add intel channel ==> only <==  your anaconda intel enviroment using `--env`.

~~~
conda config --env --add channels intel

conda config --show

channels:
  - intel
  - defaults
  - conda-forge
~~~

The intel env here is `(idp3_cdo)` so, to check the channel source precedence we use `conda config --show-sources` with this env activated.

~~~
(idp3_cdo) $ conda config --show-sources
==> /home/carlos/.condarc <==
auto_update_conda: False
ssl_verify: True
channels:
  - conda-forge
  - defaults
show_channel_urls: True
update_dependencies: False

==> /home/carlos/anaconda3/envs/idp3_cdo/.condarc <==
channels:
  - intel
  - defaults
~~~

We can see here that defaults have precedence in this env over conda-forge that is "globally" setted, so we remove the `defaults` ==>only<== in intel env using  `--env`. 

~~~
conda config --env --remove channels defaults

(idp3_cdo) $ conda config --get channels
--add channels 'defaults'   # lowest priority
--add channels 'conda-forge'   # highest priority
(idp3_cdo)  $ conda config --env --get channels
--add channels 'intel'   # lowest priority
~~~

## 2. Install the CDO without impose conda-forge

And finally install and test CDO. 

~~~
conda install cdo python-cdo 

cdo -V
Climate Data Operators version 1.9.4 (http://mpimet.mpg.de/cdo)
System: x86_64-pc-linux-gnu
CXX Compiler: g++ -fPIC -DPIC -g -O2 -std=c++11 -fopenmp -fPIC -DPIC  -m64 -fPIC -fopenmp 
CXX version : g++ (GCC) 4.8.2 20140120 (Red Hat 4.8.2-15)
C Compiler: gcc -std=gnu99 -fPIC -DPIC  -m64 -fPIC -fopenmp  
C version : gcc (GCC) 4.8.2 20140120 (Red Hat 4.8.2-15)
F77 Compiler: gfortran -g -O2
F77 version : GNU Fortran (GCC) 4.8.2 20140120 (Red Hat 4.8.2-15)
Features: 15GB C++11 Fortran DATA PTHREADS OpenMP3 HDF5 NC4/HDF5/threadsafe OPeNDAP UDUNITS2 PROJ.4 XML2 CURL FFTW3 SSE2
Libraries: HDF5/1.10.1(1.10.2) proj/4.93 xml2/2.9.8 curl/7.61.0(h7.60.0)
Filetypes: srv ext ieg grb1 grb2 nc1 nc2 nc4 nc4c nc5 
     CDI library version : 1.9.4
 CGRIBEX library version : 1.9.0
GRIB_API library version : 2.8.2
  NetCDF library version : 4.6.1 of Oct 22 2018 00:13:28 $
    HDF5 library version : 1.10.2 threadsafe
    EXSE library version : 1.4.0
    FILE library version : 1.8.3
~~~

#### References

https://code.mpimet.mpg.de/projects/cdo/wiki/Anaconda
https://code.mpimet.mpg.de/projects/cdo/files
https://code.mpimet.mpg.de/boards/2/topics/6803?r=6808



