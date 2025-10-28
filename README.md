# Project A7 - Automation: NASA STREAM Satellite Data Downloader

**Author:** Bhavin Shah\
**Email:** bshah@iastate.edu

------------------------------------------------------------------------

## 🛰️ Overview

This project demonstrates a **Model Launcher** and **Data Downloader**
workflow designed to automatically retrieve NASA STREAM satellite data.

The shell scripts (`run_stream_download.bat` and
`run_stream_download.sh`) are lightweight launchers that execute the
main automation script `run_project.py` using the system's default
Python.

------------------------------------------------------------------------

## ⚙️ Workflow Summary

### Main Steps in `run_project.py`

1.  **Environment Setup**
    -   Checks for a local folder named `stream_env`.\
    -   If not found, creates a new Python virtual environment.
2.  **Dependency Installation**
    -   Uses the virtual environment's `pip` to install all required
        libraries:
        -   `requests`, `geopandas`, `mgrs`, etc.\
    -   Ensures the environment is verified and isolated on every run.
3.  **Input Parsing**
    -   Reads download parameters from `Input/params.txt`.
4.  **Downloader Execution**
    -   Invokes `stream_downloader.py` using parameters from
        `params.txt`.
5.  **Data Download**
    -   Contacts the NASA STREAM API.\
    -   Finds and downloads satellite data that match the parameters.\
    -   Saves resulting `.TIF` files in the `Output` directory.

This entire setup is **portable** across **Windows** and **UNIX-based**
systems with Python 3.x.

------------------------------------------------------------------------

## 💻 How to Run

### Prerequisites

-   Python **3.x** (installed on the system).\
-   Internet connection (required on first run for dependency
    installation).

### On Windows

1.  Unzip the project folder.\
2.  Double-click **`run_stream_download.bat`**.\
3.  A command window opens.
    -   First run: 1--2 minutes (environment setup).\
    -   Subsequent runs: much faster.

### On UNIX / macOS / Linux

1.  Unzip the project folder.\

2.  Open a terminal and navigate into the folder.\

3.  Make the script executable:

    ``` bash
    chmod +x run_stream_download.sh
    ```

4.  Run the script:

    ``` bash
    ./run_stream_download.sh
    ```

------------------------------------------------------------------------

## 🧭 Input Configuration

The **only** file that needs modification for custom downloads is:\
`Input/params.txt`

Lines starting with `#` are comments.\
The default configuration downloads **Chlorophyll-a** data for **Lake
Pontchartrain**.

### Available Arguments

#### **Location (Choose at least one)**

-   `--site "Location Name"` --- Geocodes a human-readable name (e.g.,
    `"Ames Iowa"`, `"Lake Tahoe"`).\
-   `--latlon "lat,lon"` --- Uses an exact coordinate (e.g.,
    `"30.205,-90.096"`).\
-   `--bbox "minlat,minlon,maxlat,maxlon"` --- Finds tiles intersecting
    a bounding box.\
-   `--tile "TileID"` --- Downloads a specific tile (e.g., `"T15TVG"`,
    `"021039"`).

#### **Data (Choose at least one of each)**

-   `--product {chla|tss|secchi|tar}`
    -   `chla` --- Chlorophyll-a (water quality)\
    -   `tss` --- Total Suspended Solids (water quality)\
    -   `secchi` --- Secchi Disk Depth (water clarity)\
    -   `tar` --- TAR-zipped raw files (product dependent)\
-   `--satellite {Sentinel|Landsat|Sentinel2A|Sentinel2B|Sentinel2C|Landsat8|Landsat9}`
    -   `Sentinel` group includes 2A, 2B, and 2C.\
    -   `Landsat` group includes 8 and 9.

#### **Date (Choose one method)**

-   `--date YYYY-MM-DD` --- Specific date (can repeat for multiple).\
-   `--start YYYY-MM-DD --end YYYY-MM-DD` --- Date range.

#### **Download Options**

-   `--prefer-tif` --- *(Recommended)* Download full-resolution GeoTIF
    instead of low-res PNG.\
-   `--max-workers N` --- Optional parallel downloads (default: 6).\
-   `--wrs2-shp "path/to/shapefile.shp"` --- Optional Landsat WRS-2
    shapefile path.
    -   Default: `.\WRS2_descending_0\WRS2_descending.shp`

------------------------------------------------------------------------

## 📊 Expected Output

### Console Output

Verbose logging includes:\
- **Environment Setup:** pip installation messages.\
- **Preparing Download:** shows the full command being executed.\
- **Downloader Execution:** live progress including:\
- Geocoding results (e.g., `Geocoded 'Lake Pontchartrain' -> ...`)\
- Tile discovery (`Sentinel tiles: T16RCN`, `Landsat tiles: 021039`)\
- Summary of available files\
- File download or skip notifications (`✅ Downloaded -> ...`,
`↷ Skip ...`)\
- Final summary: `All Done ✅`

### File Output

All data saved under the `Output/` folder in this structure:

    Output/
    └── site_Lake_Pontchartrain/
        └── 021039_Landsat_chla/
            └── LC09_L1TP_021039_..._Chla_AQV202405.TIF

------------------------------------------------------------------------

## 🧩 Summary

This project provides a **self-contained**, **cross-platform**, and
**automated** workflow for downloading and managing NASA STREAM
satellite imagery with minimal setup.
