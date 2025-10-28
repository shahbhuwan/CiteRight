Project: NASA STREAM Satellite Data Downloader

Author: Bhuwan Shah

Email: bshah@iastate.edu

**======================================================================**

**WHAT THE SHELL SCRIPT DOES**

**======================================================================**

This project demonstrates a "Model Launcher" and "Data Downloader" workflow.

The shell scripts (run\_stream\_download.bat and run\_stream\_download.sh) are simple, portable launchers. Their only job is to execute the main automation script, run\_project.py, using the system's default Python.

The core automation is handled by run\_project.py. When executed, it performs the following steps:

&nbsp;  1. Sets Up Environment: It checks for a local folder named stream\_env. If it doesn't exist, it creates a new Python virtual environment in that folder.

&nbsp;  2. Installs Dependencies: It uses the virtual environment's pip to install all required Python libraries (requests, geopandas, mgrs, etc.) from the internet. This ensures the host machine is not affected. This step runs every time to verify the environment is correct.

&nbsp;  3. Parses Input: It reads the download parameters from the Input/params.txt file.

&nbsp;  4. Executes Downloader: It calls the main application, stream\_downloader.py, using the virtual environment's Python and the parameters from params.txt.

&nbsp;  5. Downloads Data: The stream\_downloader.py script contacts the NASA STREAM API, finds available satellite data matching the parameters, and downloads the resulting .TIF files into the Output folder.

This entire workflow is portable and can be run on any Windows or UNIX-based machine that has a base installation of Python 3.



**======================================================================** 

**2. HOW TO RUN**

**======================================================================**

**Prerequisites:**

&nbsp;  A base installation of Python 3.x (for running run\_project.py).

&nbsp;  An internet connection (for the first run, to install dependencies).

**Windows:**

Unzip the folder.

Double-click run\_stream\_download.bat.

A command window will open. The first run will take 1-2 minutes to install the environment. Subsequent runs will be much faster.

**UNIX (Linux/macOS):**

Unzip the folder.

Open a terminal and navigate into the folder.

Make the script executable: chmod +x run\_stream\_download.sh

Run the script: ./run\_stream\_download.sh



**======================================================================** 

**3. INPUT**

**======================================================================**

The only file you need to edit to change the download query is:

Input/params.txt

This file contains all the command-line arguments for the downloader. Lines starting with # are comments. The default file is set to find Chlorophyll-a data for Lake Pontchartrain.

Available Arguments for params.txt:



**Location (Choose at least one)**

--site "Location Name": Geocodes a human-readable name (e.g., "Ames Iowa", "Lake Tahoe").

--latlon "lat,lon": Uses an exact coordinate (e.g., "30.205,-90.096").

--bbox "minlat,minlon,maxlat,maxlon":Finds all tiles that intersect a bounding box.

--tile "TileID": Downloads data for a specific tile (e.g., "T15TVG" or "021039").



**Data (Choose at least one of each)**

--product {chla|tss|secchi}: The data product to download.

&nbsp; chla: Chlorophyll-a (water quality)

&nbsp; tss: Total Suspended Solids (water quality)

&nbsp; secchi: Secchi Disk Depth (water clarity)

--satellite {Sentinel|Landsat|Sentinel2A|Sentinel2B|Sentinel2C|Landsat8|Landsat9}: The satellite(s) to query.

&nbsp; Sentinel (group) includes 2A, 2B, and 2C.

&nbsp; Landsat (group) includes 8 and 9.



**Date (Choose one method)**

--date YYYY-MM-DD: Downloads data for one specific date. Can be repeated for multiple dates.

--start YYYY-MM-DD and --end YYYY-MM-DD: Downloads all available data within a date range.



**Download Options**

--prefer-tif: (Recommended) Downloads the full-resolution GeoTIF file. If omitted, downloads a low-resolution PNG quick-look.

--max-workers N: (Optional) Sets the number of parallel downloads. Default is 6.

--wrs2-shp "path/to/shapefile.shp": (Optional) Path to the Landsat WRS-2 shapefile. Default: .\\WRS2\_descending\_0\\WRS2\_descending.shp



**======================================================================** 

**4. EXPECTED OUTPUT**

**======================================================================**

**Console Output:** The script is very verbose and will print its progress in stages:

&nbsp; Environment Setup: You will see pip install messages as it builds the stream\_env.

&nbsp; Preparing Download Request: It will show the final command it is about to run.

&nbsp; Running Downloader: It will show the live log from the downloader, including:

&nbsp;   Geocoding results (e.g., "Geocoded 'Lake Pontchartrain' -> ...")

&nbsp;   Tile discovery (e.g., "Sentinel tiles: T16RCN", "Landsat tiles: 021039")

&nbsp;   A summary of files found (e.g., "Found 2 file(s)... Starting download...")

&nbsp;   A line for every file downloaded ("✅ Downloaded -> ...") or skipped ("↷ Skip ...").

&nbsp; All Done ✅: A final summary message.



**File Output:**

&nbsp; All downloaded satellite images will be placed in the Output/ folder.

&nbsp; A subfolder is created for each location (e.g., Output/site\_Lake\_Pontchartrain/).

&nbsp; A sub-subfolder is created for each tile/satellite/product (e.g., .../021039\_Landsat\_chla/).

&nbsp; The final .TIF files are placed inside (e.g., .../LC09\_L1TP\_021039\_...\_Chla\_AQV202405.TIF).



