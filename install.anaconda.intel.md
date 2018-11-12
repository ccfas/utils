How to Install Intel Python libs on Anaconda Enviroment
========================================================

Author: Carlos Felipe C. Alves
email: carlos.fas@gmail.com


Brief Description
------------------

Describe stepts to install and manage intel enviorements in anaconda.
The choice here after hours of problems due to conflits was install:

 1. Install Anaconda
 2. Install intelpython3_core lib as basic and pure intel env (idp)
 3. Disable automatic updates
 4. Create (idp3) with intel packages
 5. Create (idp3_bmap) by cloning (idp3)
	  Install all other packages in this env using as many  intel libs as possible using conda
 6. Config channels



Installation
=============


## 1.  Install Anaconda

Download 

> Anaconda3-5.2.0-Linux-x86_64.sh 
> https://repo.anaconda.com/archive/


 Install 

~~~
    bash ~/Downloads/Anaconda3-5.2.0-Linux-x86_64.sh
~~~

To take effect 

~~~
    source ~/.bashrc.
~~~

After this you will have base enviorement as default like this

~~~

    active environment : None
    user config file : /home/linuxize/.condarc
populated config files : 
        conda version : 4.5.4
    conda-build version : 3.10.5
        python version : 3.6.5.final.0
    base environment : /home/linuxize/anaconda3  (writable)
        channel URLs : https://repo.anaconda.com/pkgs/main/linux-64
                        https://repo.anaconda.com/pkgs/main/noarch
                        https://repo.anaconda.com/pkgs/free/linux-64
                        https://repo.anaconda.com/pkgs/free/noarch
                        https://repo.anaconda.com/pkgs/r/linux-64
                        https://repo.anaconda.com/pkgs/r/noarch
                        https://repo.anaconda.com/pkgs/pro/linux-64
                        https://repo.anaconda.com/pkgs/pro/noarch
        package cache : /home/linuxize/anaconda3/pkgs
                        /home/linuxize/.conda/pkgs
    envs directories : /home/linuxize/anaconda3/envs
                        /home/linuxize/.conda/envs
            platform : linux-64
            user-agent : conda/4.5.4 requests/2.18.4 CPython/3.6.5 Linux/4.15.0-22-generic ubuntu/18.04 glibc/2.27
                UID:GID : 1000:1000
            netrc file : None
        offline mode : False

~~~


## 2. Install intelpython3_core lib as basic and pure intel env (idp)

~~~
conda create -n idp3_core intelpython3_core python=3.6.5 -c intel
conda create -n idp2_core intelpython3_core python=2 -c intel
~~~

## 3. Disable automatic updates

Edit ` .condarc` of the installation directory and your home dir

~~~
    conda config --set auto_update_conda false

    $ which conda
    /home/carlos/anaconda3/bin/conda

    $ nano /home/carlos/anaconda3/bin/.condarc 
~~~
 
Add lines

>   auto_update_conda: False
>   update_dependencies: False

 Note, `conda config --set update_dependencies False`  do not work due to a bug (https://github.com/conda/conda/issues/6393)

The  `.condarc` file may also be located in the root environment, in which case it overrides any in the home directory. By default, conda install updates the given package to the latest version, and installs any dependencies necessary for that package. If you would prefer that conda update all dependencies to the latest version that is compatible with the environment, set update_dependencies to `True`:

When `auto_update_conda` is `True`, conda updates itself any time a user updates or installs a package in the root environment. 


https://conda.io/docs/user-guide/configuration/use-condarc.html#disable-updating-of-dependencies-update-dependencies
https://conda.io/docs/user-guide/configuration/use-condarc.html#channel-locations-channels


## 4. Create (idp3) with intel packages

~~~
    conda create -n idp3 intelpython3_core pandas jupyter notebook scikit-learn scipy python=3.6.5 -c intel
    source activate idp3
    conda install spyder
    source deactivate
~~~


https://anaconda.org/intel/repo
https://anaconda.org/intel/pandas
https://anaconda.org/intel/scikit-learn
https://anaconda.org/intel/scipy


## 5. Create (idp3_bmap) by cloning (idp3)

~~~
    conda create -n idp3_bmap --clone idp3
    source activate idp3_bmap
    conda install basemap proj4 netCDF4 -c conda-forge 
~~~

https://anaconda.org/conda-forge/netcdf4


## 6. Config channels

## Install Intel and Additional Conda Channels

Install specific channels in .condarc of the specific enviroment 
Sorts packages from highest to lowest channel priority.
channels from .condarc enviorements has precedence (higher piority) over /home/.condarc

~~~
    source activate base
    conda config --add channels conda-forge
    source deactivate

    source activate idp3_bmap
    conda config --env --add channels intel
~~~

Checking 

~~~

    (idp3_bmap) $ conda config --get channels
    --add channels 'defaults'   # lowest priority
    --add channels 'conda-forge'   # highest priority

    (idp3_bmap) $ conda config --env --get channels
    --add channels 'intel'   # lowest priority

    conda config --show-sources
~~~


https://software.intel.com/en-us/articles/using-intel-distribution-for-python-with-anaconda
https://conda.io/docs/user-guide/tasks/manage-channels.html
https://conda.io/docs/user-guide/configuration/use-condarc.html#channel-locations-channels
https://stackoverflow.com/questions/39558316/how-can-i-remove-a-url-channel-from-anaconda






#### References

https://anaconda.org/intel/repo
https://conda.io/docs/user-guide/configuration/use-condarc.html#channel-locations-channels
https://conda.io/docs/user-guide/configuration/admin-multi-user-install.html#admin-inst
https://conda.io/docs/user-guide/tasks/manage-environments.html
https://stackoverflow.com/questions/40616381/can-i-add-a-channel-to-a-specific-conda-environment
https://medium.freecodecamp.org/why-you-need-python-environments-and-how-to-manage-them-with-conda-85f155f4353c




Aditional Notes
=================

This tutorial used Anaconda3-5.2.0-Linux-x86_64.sh avaiable at https://repo.anaconda.com/archive/ .

Intel python3 core libs were used due to some compatibility issues during packges installation, 
you can see that following this steps, you will have almost the same intel packages available in 
intelpython3_full, but doing that way you will just install intel packages as you needed and is available 
in intel channel using conda to handle dependencies proporly.

A comparsion table between intelpython3_core and intelpython3_core is avaiable at:

https://software.intel.com/en-us/articles/complete-list-of-packages-for-the-intel-distribution-for-python 

Note that up to now intel just support python 2.7.14, 3.6.5 so we will use this version to create intel env. 
The installation of new packages will use anaconda intel channel as main source of packages  
 
Anaconda3-5.2.0-Linux-x86_64.sh

https://docs.anaconda.com/anaconda/packages/oldpkglists/
https://linuxize.com/post/how-to-install-anaconda-on-ubuntu-18-04/

Conda
4.5.11 (2018-08-21)
Conda is an open source package management system and environment management system
https://conda.io/docs/release-notes.html#id1
https://conda.io/docs/index.html


distribution
Anaconda 5.2.0 (May 30, 2018)
https://docs.anaconda.com/anaconda/reference/release-notes/
https://www.anaconda.com/blog/developer-blog/anaconda-distribution-5-2/


## Conda Env Roll Back 

Use  `conda list --revisions` to see available revisions and  `conda install --revision N`  (where N is the revision number) to rollback to revision N.

~~~
    conda install -c anaconda basemap

    conda list --revisions

    2018-10-29 16:48:53  (rev 1)
        +basemap-1.2.0 (conda-forge)
        +geos-3.6.2 (conda-forge)
        +proj4-4.9.3 (conda-forge)
        +pyproj-1.9.5.1 (conda-forge)
        +pyshp-1.2.12 (conda-forge)


    conda install --revision 0 
~~~

https://anaconda.org/intel/repo?sort=_name&sort_order=asc
http://blog.rtwilson.com/conda-revisions-letting-you-rollback-to-a-previous-version-of-your-environment/
https://github.com/conda-forge/basemap-feedstock/issues/30


## Update Note


I assume you have intel channel in your .condarc file, so when packages are updated, intel channel takes precedence over defaults and there could be certain packages not updated in intel channel unlike the defaults.

When you are using Intel Distribution for Python, I would advise to have the latest versions available only in intel channel, although the package versions are older than that ones available in defaults to avoid any conflicts in dependencies . try to update your conda with "conda update --all -c intel"

So for the issue that I see here, this might resolve your problem

step 1. conda update -n base conda

updates root conda package manager based on defaults channel

step 2. conda update --all -c intel

upgrades/downgrades packages in IDP environment based on latest versions available in intel channel

https://software.intel.com/en-us/forums/intel-distribution-for-python/topic/754712


## CONDA Enviroments

conda env list
conda info
conda config --show

auto_update_conda: True
channel_priority: True
channels:
  - intel
  - conda-forge
  - defaults


conda config --show-sources

conda config --set auto_update_conda False
conda config --set update_dependencies False
* do not work due to a bug (https://github.com/conda/conda/issues/6393)


## Status
~~~
(idp_bmap) $ conda info

     active environment : idp_bmap
    active env location : /home/carlos/anaconda3/envs/idp_bmap
            shell level : 1
       user config file : /home/carlos/.condarc
 populated config files : /home/carlos/.condarc
                          /home/carlos/anaconda3/envs/idp_bmap/.condarc
          conda version : 4.5.11
    conda-build version : 3.10.5
         python version : 3.6.5.final.0
       base environment : /home/carlos/anaconda3  (writable)
           channel URLs : https://conda.anaconda.org/intel/linux-64
                          https://conda.anaconda.org/intel/noarch
                          https://conda.anaconda.org/conda-forge/linux-64
                          https://conda.anaconda.org/conda-forge/noarch
                          https://repo.anaconda.com/pkgs/main/linux-64
                          https://repo.anaconda.com/pkgs/main/noarch
                          https://repo.anaconda.com/pkgs/free/linux-64
                          https://repo.anaconda.com/pkgs/free/noarch
                          https://repo.anaconda.com/pkgs/r/linux-64
                          https://repo.anaconda.com/pkgs/r/noarch
                          https://repo.anaconda.com/pkgs/pro/linux-64
                          https://repo.anaconda.com/pkgs/pro/noarch
          package cache : /home/carlos/anaconda3/pkgs
                          /home/carlos/.conda/pkgs
       envs directories : /home/carlos/anaconda3/envs
                          /home/carlos/.conda/envs
               platform : linux-64
             user-agent : conda/4.5.11 requests/2.18.4 CPython/3.6.5 Linux/4.15.0-38-generic linuxmint/19 glibc/2.27
                UID:GID : 1000:1000
             netrc file : None
           offline mode : False

(idp_bmap) $ cat /home/carlos/anaconda3/envs/idp_bmap/.condarc
channels:
  - intel
(idp_bmap) $ cat /home/carlos/.condarc
channels:
  - conda-forge
  - defaults
ssl_verify: true

(idp_bmap) $ conda config --env --get channels
--add channels 'intel'   # lowest priority
(idp_bmap) $ conda config --get channels
--add channels 'defaults'   # lowest priority
--add channels 'conda-forge'   # highest priority

(idp_bmap) $ conda list | grep conda-forge
basemap                   1.2.0            py36h50ae964_0    conda-forge
bokeh                     0.13.0                py36_1000    conda-forge
click                     7.0                        py_0    conda-forge
cytoolz                   0.9.0.1          py36h470a237_1    conda-forge
dask                      0.20.0                     py_0    conda-forge
dask-core                 0.20.0                     py_0    conda-forge
distributed               1.24.0                py36_1000    conda-forge
geos                      3.6.2                hfc679d8_3    conda-forge
h5netcdf                  0.6.2                      py_0    conda-forge
h5py                      2.8.0            py36h7eb728f_3    conda-forge
heapdict                  1.0.0                 py36_1000    conda-forge
locket                    0.2.0                      py_2    conda-forge
msgpack-python            0.5.6            py36h2d50403_3    conda-forge
partd                     0.3.9                      py_0    conda-forge
proj4                     5.2.0                h470a237_0    conda-forge
pyshp                     1.2.12                     py_0    conda-forge
sortedcontainers          2.0.5                      py_0    conda-forge
tblib                     1.3.2                      py_1    conda-forge
toolz                     0.9.0                      py_1    conda-forge
xarray                    0.10.9                   py36_0    conda-forge
zict                      0.1.3                      py_0    conda-forge
~~~

Primary install 

> basemap                   1.2.0            py36h50ae964_0    conda-forge
> geos                      3.6.2                hfc679d8_3    conda-forge
> proj4                     5.2.0                h470a237_0    conda-forge
> xarray                    0.10.9                   py36_0    conda-forge



~~~
$ conda env list
# conda environments:
#
base                  *  /home/carlos/anaconda3
geo                      /home/carlos/anaconda3/envs/geo
idp                      /home/carlos/anaconda3/envs/idp
idp_bmap                 /home/carlos/anaconda3/envs/idp_bmap
ncl_stable               /home/carlos/anaconda3/envs/ncl_stable
orange                   /home/carlos/anaconda3/envs/orange
py2                      /home/carlos/anaconda3/envs/py2


(base) $ conda info

     active environment : base
    active env location : /home/carlos/anaconda3
            shell level : 1
       user config file : /home/carlos/.condarc
 populated config files : /home/carlos/.condarc
          conda version : 4.5.11
    conda-build version : 3.10.5
         python version : 3.6.5.final.0

(idp) $ conda info

     active environment : idp
    active env location : /home/carlos/anaconda3/envs/idp
            shell level : 1
       user config file : /home/carlos/.condarc
 populated config files : /home/carlos/.condarc
                          /home/carlos/anaconda3/envs/idp/.condarc
          conda version : 4.5.11
    conda-build version : 3.10.5
         python version : 3.6.5.final.0
       base environment : /home/carlos/anaconda3  (writable)
           channel URLs : https://conda.anaconda.org/intel/linux-64
                          https://conda.anaconda.org/intel/noarch
                          https://conda.anaconda.org/conda-forge/linux-64
                          https://conda.anaconda.org/conda-forge/noarch
                          https://repo.anaconda.com/pkgs/main/linux-64
                          https://repo.anaconda.com/pkgs/main/noarch
                          https://repo.anaconda.com/pkgs/free/linux-64
                          https://repo.anaconda.com/pkgs/free/noarch
                          https://repo.anaconda.com/pkgs/r/linux-64
                          https://repo.anaconda.com/pkgs/r/noarch
                          https://repo.anaconda.com/pkgs/pro/linux-64
                          https://repo.anaconda.com/pkgs/pro/noarch
          package cache : /home/carlos/anaconda3/pkgs
                          /home/carlos/.conda/pkgs
       envs directories : /home/carlos/anaconda3/envs
                          /home/carlos/.conda/envs
               platform : linux-64
             user-agent : conda/4.5.11 requests/2.18.4 CPython/3.6.5 Linux/4.15.0-38-generic linuxmint/19 glibc/2.27
                UID:GID : 1000:1000
             netrc file : None
           offline mode : False
~~~













