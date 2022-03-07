# X-ray-detector-gain-map
Calculated gain maps for area detectors at APS Sector 11

# Documentation is a work in progress
# Obtaining GSAS-II calibration maps
![GSAS-II Calibration](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/gsas%20II%20calibration.PNG)

Import calibration measurements into GSAS-II and run calibration on all measurements. Shown here are CeO2 calibration patterns taken at three positions for a Pilatus 2M CdTe detector. Make sure calibrations are reasonable. All three measurements should have the same calibration distance. 

![Save .gpx file as 'calibrations.gpx'](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/save%20as.png)

Select File -> Save project as and save file as 'calibrations.gpx' (Other file names may be used if desired)
Make sure to save calibration file in the same directory as the CeO2 calibration patterns.

![Save .imgctrl files](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/save%20image%20controls.png)

Select Params -> Save multiple controls

![Save multiple controls menu](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/save%20controls.PNG)

In the menu that pops up click "select all" (or select all images manually) and click 'Ok'

![Select folder to save controls](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/save%20controls%20menu.png)

Navigate to folder where you saved the calibration file and click 'Select folder'

![Directory containing .imgctrl and .gpx files](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/directory.png) 

Folder should now contain three files with the extension .imgctrl with the same filename as the CeO2 calibration patterns.
Close GSAS-II. Following python script will not work if GSAS-II is already open.
Place downloaded 'Save maps.ipynb' file in same directory with files as pictured

![Save maps script, GSAS-II directory](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/gsas%20install%20directory.PNG)

Change this to the install directory of GSAS-II

![Save maps script, calibration file name](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/gsas%20calibration%20file%20name.PNG)

Make sure this is the same name as the .gpx file in the directory as the .ipynb script. Leave this alone if your calibration file is already named 'calibrations.gpx'

![Run scrpt](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/run%20script.PNG)

Make sure GSAS-II is not already open or the script will not work
Select Cell -> Run all
Script will take a few seconds to run

![maps folder](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/directory%20with%20maps.png)

Folder should now contain a new folder named 'maps' containing .tif files
