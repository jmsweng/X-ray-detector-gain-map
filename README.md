# X ray detector gain map
Code is provided in the form of Jupyter Notebooks, installation instructions for Jupyter are found [here](https://jupyter.org/install), though it is already included in the default install configuration of [Anaconda Python](https://www.anaconda.com/products/individual) which is the recommended Python install for this code.
Code is written for Python 3 (Python 3.9.7 was used to run the code to generate example images).

### Documentation is currently a work in progress and may (though is not likely to significantly) change without notice

Code to calculate a gain map for an x-ray area detector at an arbitrary beam energy using measurements of an amorphous scatterer.
May be used to correct for non-uniformity in detector response at energies where it is not possible collect a flat field.
May also be used to quickly measure a gain map to monitor and correct for detector degradation from exposure to radiation.

With an incorrect gain map, experimental measurements will likely be erroneous.  Even when the experimental data is spatially averaged such as in a radially averaged 1D pattern from a 2D detector using the original factory gain map shows peak shifts and an apparent isosbestic point around 2θ ≈ 3.7 when the detector is simply translated, as shown below. Data shown below is collected from a Pilatus 2M CdTe detector.

![Uncorrected 1D scattering pattern](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/Uncorrected%201D.png)

Applying a recently collected gain map remedies this issue

![Corrected 1D scattering pattern](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/Corrected%201D.png)

Due to the slight drift in the detector gain over time the detector's gain map will change with time and should be periodically updated. Data corrected with gain maps collected roughly one month apart will result in subtly different scattering patterns, even when radially integrated to 1D as shown below. This is most noticable around 2θ ≈ [0.5, 3, 5]. It is not clear what factors influence this drift and it is likely detector specific. When correcting data with a gain map, the gain map used should thus be one which was measured as recently as possible to minimize errors induced in detector drift.

![gain map, 1 month apart](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/Gain%20correction%201%20month%20apart.png)

This code provides an alternative measure to measure the gain map of an x-ray area detector at the energy which it is used. Rather than collecting multiple flat fields at energies which correspond to x-ray fluorescence lines, a gain map is calculated from a set of measurements at one energy. For measurements particularly sensitive to peak intensities, such as liquid pair distribution function analysis, it is critical to have a proper gain map.

Examples shown are with data taken on a Pilatus 2M CdTe, this is done for presentation consistency and should **not** be interpreted as an indication that the problems, solutions, or code presented are unique to this detector. 

# Measurements required
A series of scattering patterns should be collected using an amorphous scatterer with the detector placed fairly far away from the sample. The position of the beam stop for each measurement should not overlap with the position of the beam stop of a previous measurement. The series of measurements should look like the following:

![3 scattering positions](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/3%20measurement%20positions.png)

Code provided uses 5 scattering positions. More than 5 scattering positions does not appear to improve the collected gain map, and 3 scattering positions appears to be sufficient in most cases. The code may be easily modified more or less than 5 scattering positions. If the front of the beam stop is not normal to the incident beam asymmetric scattering from the beam stop may be present, creating regions of erroneous results in the final gain map if only 3 scattering positions are used (use 5 positions if you are unsure if this is an issue).

Examples shown may use 3 scattering positions to save space on the page, not because 3 scattering positions is in some way better.

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

# Usage of gain map calculation script
Place 'Gain map calculation.ipynb' in a directory containing measurements of amorphous scatterer and standards at each position as shown below.
Measurements taken at Position 1 should be placed in a directory named 'Pos 1', Position 2 in a directory named 'Pos 2', and so on. Positions do not need to be in any particular order. Maps directory exported from GSAS-II in previous step should be in a directory named 'Standard/maps/'. Position numbers should match between amorphous scatterer and calibration measurements. Amorphous scatterer measurements are expected to be .tif files.
Different directories may be used if the script is changed to correspond to the file locations if desired. 

![gain map calculation directory](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/gain%20map%20calculation%20directory.png)

Open the jupyter notebook 'Gain map calculation.ipynb' and run the first cell by clicking in it and hitting [Shift] + [Enter]. This imports the python libraries used by this script.

![Gain map calculation, import libraries](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/gain%20map%20calculation%2C%20import%20libraries.PNG)

Check to make sure the directories in the next cell correspond to the locations of the amorphous scattering measurements and then hit [Shift] + [Enter] to run the cell. This may take a few moments depending on the number of measurements taken, as they are all imported and averaged for each position.

![Gain map calculation, directories](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/gain%20map%20calculation%2C%20import%20files.png)

Click inside the next cell containing function definitions and hit [Shift] + [Enter] to run cell in order to initialize functions. Only the beginning of the cell is pictured below.

![Gain map calculation, initialize functions](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/gain%20map%20calculation%2C%20initialize%20functions.png)

Click inside the next cell and hit [Shift] + [Enter] to calculate the radial average (actually the median) of the averaged patterns of the amorphous scatterer. The bins parameter may be changed if desired. Radial averages will be plotted. This may take a few moments. An error may appear due to complaining about an All-NaN slice due to how dead pixels are handled, this may be safely ignored. In the radial averages, select a region that is reasonably flat towards the high 2θ values that is in all of the radial averages. A linear fit will be perfored to this section to extrapolate high 2θ values that are not in all the 1D diffraction patterns. In the example below the region from 2θ = 8 to 2θ = 9.5 is selected.

![Gain map calculation, radial average](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/gain%20map%20calculation%2C%20radial%20averages.png)

To change the start and stop of the x-axis to zoom in, add the following line and adjust the two values to the desired value. Rerun the cell by hitting [Shift] + [Enter] to recalculate the averages and re-plot the graph of the 1D patterns. The dimensions of the plot may also be changed by changing the two values in the section in parentheses after 'plt.rcParams["figure.figsize'] = '. Change these two values (15, 5) to adjust (width, height) of the plot.

![Gain map calculation, radial averages change plot region](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/gain%20map%20calculation%2C%20radial%20averages%2C%20change%20plot%20region.png)

Change the following two numbers in the next cell to the extrapolation bounds chosen in the previous step. The number of bins used in the calculation of the radial average may be changed on the next line, this may raise an error identical to the previous step which may be safely ignored. 

If the number of bins is too large and results in an empty bin with no values, the following error will appear indicating a mismatch between the number of 2θ bins and scattering count values. Reduce the number of bins if this occurs. A value around the smaller of the width or height of the detector generally works.

ValueError: x and y must have same first dimension, but have shapes ...

![Gain map calculation, extrapolation bounds](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/gain%20map%20calculation%2C%20extrapolation%20bounds.png)

Running the next cell plots the averaged scattering pattern at each position and the corresponding gain map calculated at each position.
![Gain map calculation, all positions](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/Gain%20map%20calculation%2C%20all%20positions.png)

The next cell applies a median filter to all pixels which are not radiation damaged to remove noise resulting from amplifier conversion errors (this appears as a speckle pattern). For the window size parameter use some value less than or equal to the width of point spread function of the detector. To obtain this experimentally, cut down the beam to smaller than the size of a single pixel and shine the attenuated beam on the detector and look at the width of the resulting peak. For a an a-Si detector such as the Varex 4343CT this is generally around a 10x10 spread, for the Pilatus 2M CdTe this is a 3x3 spread. A width parameter for the median filter window should be an odd integer close to the width of the spread (9 for a 10x10, 3 for a 3x3). This spread may vary slightly based on beam energy, but a parameter of 5 should work reasonably well if this is unknown. 
Pixels which are damaged, defined as pixels with a gain more than 4 standard deviations away from the average of the whole detector, are ignored by the median filter.
The calculated gain map will then be plotted. 

![Gain map calculation, correction](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/gain%20map%20calculation%2C%20median%20filter.png)

Running the next cell will export the gain map as a 32 bit .tif file with the desired name, followed by the current date in the format [filename]\_YYYY-MM-DD.tif
Changing the g_form parameter will select the format of the gain map, g_form = 1 results in a gain map that should be multiplied by the measurement from the detector, g_form = 0 results in a gain map that should be divided by the measurement from the detector. 

![Gain map calculation, save map](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/gain%20map%20calculation%2C%20save%20gain%20map.png)

# Example of a calculated gain map

The function gain_map_all_pos() returns a calculated gain map for the input measurement positions, shown below along with the amorphous scattering pattern taken at each position.

![Example output](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/3%20measurement%20positions%2C%20with%20gain%20maps.png)

The corners of the calculated gain maps will likely have artifacts, like in the example shown, due to the extrapolation performed for the higher Q regions not measured in all of the scattering patterns. This is normal and does not effect the final output. If it does show up in the final output perform the measurements with less separation between the beam stop positions.

The beam stop and some asymmetric scattering around the beam stop results in the dark spot and surrounding ring in the gain map at each position. There may also be a larger ring visible at a greater distance from the beam stop if the beam stop is particularly crooked. If this is visible in the final output the alignment of the beam stop should be corrected and the measurements retaken.

The final calculated gain map for the example is shown below:

![Final calculated gain map](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Images/Final%20gain%20map.PNG)

Bright yellow spots for this detector (pixels with large gain corrections applied) are pixels which are known to have radiation damage and may not respond correctly even after correction. If a large number of these are present and unexpected in the final gain map there may be some problem with the detector or experimental setup. 

# General description of calculation method

# Comparison to gain maps calculated from x-ray fluorescence flat fields
Work in progress
