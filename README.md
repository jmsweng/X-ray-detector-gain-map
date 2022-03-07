# X-ray-detector-gain-map

# Documentation is currently a work in progress

Code to calculate a gain map for an x-ray area detector at an arbitrary beam energy using measurements of an amorphous scatterer.
May be used to correct for non-uniformity in detector response at energies where it is not possible collect a flat field.
May also be used to quickly measure a gain map to monitor and correct for detector degradation from exposure to radiation.

With an incorrect gain map, experimental measurements will likely be erroneous.  Even when the experimental data is spatially averaged such as in a radially averaged 1D pattern from a 2D detector using the original factory gain map shows peak shifts and an apparent isosbestic point around 2θ ≈ 3.7 when the detector is simply translated, as shown below. Data shown below is collected from a Pilatus 2M CdTe detector.

![Uncorrected 1D scattering pattern](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/Uncorrected%201D.png)

Applying a more recently collected gain map remedies this issue

![Corrected 1D scattering pattern](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/Corrected%201D.png)

These gain maps will drift with time to varying degrees depending on the detector. This code provides an alternative measure to measure the gain map of an x-ray area detector at the energy which it is used. Rather than collecting multiple flat fields at energies which correspond to x-ray fluorescence lines, a gain map is calculated from a set of measurements at one energy.

# Measurements required
A series of scattering patterns should be collected using an amorphous scatterer with the detector placed fairly far away from the sample. The position of the beam stop for each measurement should not overlap with the position of the beam stop of a previous measurement. The series of measurements should look like the following:

![3 scattering positions](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/3%20measurement%20positions.png)

Code provided uses 3 or 5 scattering positions. More than 5 scattering positions does not appear to improve the collected gain map, though the code may be easily modified for that purpose. If the front of the beam stop is not normal to the incident beam asymmetric scattering from the beam stop may be present, creating regions of erroneous results in the final gain map if only 3 scattering positions are used (use 5 positions if you are unsure if this is an issue).

Examples shown use 3 scattering positions to save space on the page, not because 3 scattering positions is in some way better.

An example of the experimental setup is shown below using a stack of 10 glass microscope slides:

![Gain map experiment set up](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/Glass%20microscope%20slides.png)


A stack of glass microscope slides works well as the amorphous scatterer, though other scatterers may also be used. Some nanoparticle materials tested have some asymmetric small angle scattering which can cause problems with the calculation, so ensure this is not the case for whatever material is used. The asymmetric scattering wil appear as very obvious roughly circular artifacts around beamstop positions in the final gain map if present. 

Ensure that the detector counts over the pattern stay within the linear regime of the detector for whatever collection time is used, a good rule of thumb is to stay between 10-90% of the max counting depth of the detector. If a stack of microscope slides is used as the scatterer the number of slides may be varied to change the scattering intensity. 

Sample distance to detector does not appear to be critical beyond altering the detector counts due to air scattering attenuation.

Examples of experiment conditions at APS beamlines:
  - 17-BM-B
    - Scatterer: 4 Microscope slides
    - Energy: 27 and 51 keV
    - Collection time: 30 minutes per position
    - Detector: Varex 4343 CT
  - 11-ID-C
    - Scatterer: 10 microscope slides
    - Energy: 105.7 keV
    - Collection time: 150 seconds per position
    - Detector: Pilatus 2M CdTe and Perkin Elmer XRD1621
   
  (Not yet collected)
  - 11-ID-B
    - Scatterer:
    - Energy:
    - Collection Time:
    - Detector: Perkin Elmer XRD1621
  - 6-ID
    - Scatterer:
    - Energy:
    - Collection Time:
    - Detector: Pilatus 2M CdTe
   

At each position a calibrant should also be measured, CeO2 is used in the examples. A black mark will appear in the glass slides at the beam position, this does not appear to effect the measurement.

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

# Example of resulting gain map

The function gain_map_all_pos() returns a calculated gain map for the input measurement positions, shown below along with the amorphous scattering pattern taken at each position.

![Example output](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/3%20measurement%20positions%2C%20with%20gain%20maps.png)

The corners of the calculated gain maps will likely have artifacts, like in the example shown, due to the extrapolation performed for the higher Q regions not measured in all of the scattering patterns. This is normal and does not effect the final output. If it does show up in the final output perform the measurements with less separation between the beam stop positions.

The beam stop and some asymmetric scattering around the beam stop results in the dark spot and surrounding ring in the gain map at each position. There may also be a larger ring visible at a greater distance from the beam stop if the beam stop is particularly crooked. If this is visible in the final output the alignment of the beam stop should be corrected and the measurements retaken.

The final calculated gain map for the example is shown below:

![Final calculated gain map](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/Final%20gain%20map.PNG)

Bright yellow spots for this detector (pixels with large gain corrections applied) are pixels which are known to have radiation damage and may not respond correctly even after correction. If a large number of these are present and unexpected in the final gain map there may be some problem with the detector or experimental setup. 
