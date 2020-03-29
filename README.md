# SREMPyLandsat
Open python realization of SREM (A Simplified and Robust Surface Reflectance Estimation Method) for Landsat 4-5 TM, 7 ETM+, 8 OLI datasets (Level 1C).

Original algorithm paper:
Bilal, M.; Nazeer, M.; Nichol, J.E.; Bleiweiss, M.P.; Qiu, Z.; Jäkel, E.; Campbell, J.R.; Atique, L.; Huang, X.; Lolli, S. A Simplified and Robust Surface Reflectance Estimation Method (SREM) for Use over Diverse Land Surfaces Using Multi-Sensor Data. Remote Sens. 2019, 11, 1344.

https://www.mdpi.com/2072-4292/11/11/1344/htm

Module landsatangles (part of 'python-fmask') by Neil Flood is used as part of SREMPy-landsat.

### Important information

For most correct work you must download and compile angles util by USGS. Information and download links: https://www.usgs.gov/land-resources/nli/landsat/solar-illumination-and-sensor-viewing-angle-coefficient-files

Angles generated by these utils are important input for algorithm.

### Prerequisites

python3, gdal, numpy


### Important note for Windows users who wants to use USGS Utils for angles

I recommend to use cygwin compiler. Download it [here](https://cygwin.com/install.html). Compile USGS angles util with ''make'' command.

''cygwin_bash_exe_path'' option is specially for you. It must always be None for Unix users! For Windows users set there full path to bash.exe cygwin executable (e.g. C:/cygwin64/bin/bash.exe)


### How to use

You can use SREMPy-landsat in four cases. Input is always Landsat L1C imagery. Landsat 4-5 (TM), Landsat 7 (ETM+) and Landsat 8 (OLI) supported. Always use full path for all files!

1. Surface reflectance estimation for single landsat band with automated generation of angles with compiled USGS util

```python   
from SREMPyLandsat.SREMPyLandsat import SREMPyLandsat

srem = SREMPyLandsat(mode='landsat-usgs-utils')
data = {'band':'../LC08_L1TP_183021_20180530_20180614_01_T1_B3.TIF',
        'metadata':'../LC08_L1TP_183021_20180530_20180614_01_T1_MTL.txt',
        'angles_file':'../LC08_L1TP_183021_20180530_20180614_01_T1_ANG.txt',
        'usgs_util_path':'../software/l8_angles/l8_angles',
        'temp_dir':'../temp_dir',
        'cygwin_bash_exe_path':None}

srem.set_data(data)
sr = srem.get_srem_surface_reflectance_as_array()
srem.save_array_as_gtiff(sr, '../LC08_L1TP_183021_20180530_20180614_01_T1/B3_SREM_USGS.TIF')
```

2. Surface reflectance estimation for single landsat band with direct paths of files with angles, prepared manually

```python    
from SREMPyLandsat.SREMPyLandsat import SREMPyLandsat

srem = SREMPyLandsat(mode='landsat-raw')
data = {'band':'../LC08_L1TP_183021_20180530_20180614_01_T1_B3.TIF',
        'metadata':'../LC08_L1TP_183021_20180530_20180614_01_T1_MTL.txt',
        'solar_azimuth':'../sun_azimuth.tif',
        'sensor_azimuth':'../sat_azimuth.tif',
        'solar_zenith':'../sun_zenith.tif',
        'sensor_zenith':'../sat_zenith.tif',
        'angles_coef':1.0}

srem.set_data(data)
``` 

3. Surface reflectance estimation for single landsat band without angles, with simplified model

```python         
from SREMPyLandsat.SREMPyLandsat import SREMPyLandsat

srem = SREMPyLandsat(mode='landsat-auto')
data = {'band':'../LC08_L1TP_027037_20190613_20190619_01_T1_B3.TIF',
        'metadata':'../LC08_L1TP_027037_20190613_20190619_01_T1_MTL.txt',
        'temp_dir':'../temp_dir'}

srem.set_data(data)
a = srem.get_srem_surface_reflectance_as_array()
srem.save_array_as_gtiff(a,'../B3_SREM_Auto.TIF')
``` 

4. Surface reflectance for all suitable bands from landsat L1C dataset

```python
from SREMPyLandsat.utils import process_full_landsat_dataset_with_usgs_util

process_full_landsat_dataset_with_usgs_util(metadata_file='../LC08_L1TP_121027_20180731_20180814_01_T1_MTL.txt',
                                            angles_file='../LC08_L1TP_121027_20180731_20180814_01_T1_ANG.txt',
                                            usgs_util_path='../software/l8_angles/l8_angles.exe',
                                            temp_dir='../temp_dir',
                                            output_dir='../output_dir',
                                            cygwin_bash_exe_path='C:/cygwin64/bin/bash.exe')
```
## Contacts

Feel free to contact me: ee.kazakov@gmail.com
