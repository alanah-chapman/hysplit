# Hysplit

  How to compile era52arl on gadi, which converts native ECMWF ERA5 file format (grib) to the format required for hysplit input (arl).
  This script requires (see era52arl MAIN PROGRAM DOCUMENTATION BLOCK - line 22-23):
  1. A 3D file - pressure level data (note: will not work for model levels)
  2. One or two 2D files - surace level data

### Locations

  I have a copy of hysplit.v5.2.3 in my directory in q90 (copied from Rob Ryan). 
  
### Modifying the Makefiles

  There are two Makefiles that need modification to compile era52arl. 
  
  The first is found here `hysplit.v5.2.3/data2arl/era52arl` called `Makefile` 
  - Edit this Makefile to point to the locations of the eccodes library in gadi:
    *Change*
    `# Dependencies
    api2arl_DEPENDENCIES = \
    $(ECCODES_TOPDIR)/lib/libeccodes_f90.so \
    $(ECCODES_TOPDIR)/lib/libeccodes.so`
  
