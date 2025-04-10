# ERA52ARL Hysplit

  How to compile era52arl on gadi, which converts native ECMWF ERA5 file format (grib) to the format required for hysplit input (arl).
  This script requires (see era52arl MAIN PROGRAM DOCUMENTATION BLOCK - line 22-23):
  1. A 3D file - pressure level data (note: will not work for model levels)
  2. One or two 2D files - surace level data

### Locations

  - File locations to compile era52arl program: `hysplit.v5.2.3/data2arl/era52arl`
  - Compiled era52arl program: `hysplit.v5.2.3/exec`
  
### Step 1: Modifying the Makefiles to compile era52arl.f

  There are two Makefiles that need modification to compile era52arl. <br/>
  The first is found here: `hysplit.v5.2.3/data2arl/era52arl` called `Makefile` 
  1. Edit this Makefile to point to the locations of the eccodes library in gadi: <br/>
    *Change* <br/>
    `# Dependencies
    api2arl_DEPENDENCIES = \` <br/>
      `$(ECCODES_TOPDIR)/lib/libeccodes_f90.so \` <br/>
      `$(ECCODES_TOPDIR)/lib/libeccodes.so` <br/>
    *To:* <br/>
    `# Dependencies
    api2arl_DEPENDENCIES = \` <br/>
      `$(ECCODES_TOPDIR)/lib64/libeccodes_f90.so \` <br/>
      `$(ECCODES_TOPDIR)/lib64/libeccodes.so` <br/>
    - This Makefile points to another Makefile in the first line: `include ../../Makefile.inc
     
  2.  Edit the second Makefile found in the parent directory: `/g/data/q90/ac9768/hysplit.v5.2.3/Makefile.inc` <br/>
    - The edits here change the locations of $(ECCODES_TOPDIR), $ (ECCODESINC) and $(ECCODESLIBS) <br/>
    *Change (the original paths are default and do not correspond to the eccodes library on gadi)* <br/>
      `ECCODES_TOPDIR= /usr` <br/>
      `ECCODESINC= -I/opt/eccodes/include` <br/>
      `ECCODESLIBS= -L/usr/lib64 -leccodes_f90 -leccodes` <br/>
    *To:* <br/>
      `ECCODES_TOPDIR= /apps/eccodes/2.34.1` <br/>
      `ECCODESINC= -I/apps/eccodes/2.34.1/include` <br/>
      `ECCODESLIBS= -L/apps/eccodes/2.34.1/lib64 -leccodes_f90 -leccodes` <br/>

  ### Step 2: Compile era52arl.f
  
  Load eccodes library: `module load eccodes` (I'm not sure if this is necessary but just in case) <br/>
  Within `hysplit.v5.2.3/data2arl/era52arl` run `make` <br/>
  This should create an executable in `/g/data/q90/ac9768/hysplit.v5.2.3/exec` called `era52arl`  <br/>

  ### Step 3: Running era52arl

    ./era52arl -i<path_to_3d_era5_grib_file> -a<path_to_2d_era5_grib_file> -v
    *Note: remove <>, also make sure no space is between -i, -a and the file paths - otherwise era52arl cannot see the grib files*
    
    
