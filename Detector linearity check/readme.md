# Script used to check linearity of x-ray detectors

# Measurements required
A series of scattering patterns should be taken, ideally with something which covers a large area of the detector (such as an amorphous scatterer), with different exposure times. The scatterer used is not critical, but should have a strong enough scattering pattern to actually be measured on the detector at both the shortest and longest exposure times.

Ensure that the exposure time is actually being changed, rather than increasing the number of summed frames; a 0.5s exposure should not be taken as the sum of 5 0.1s exposures. Make sure that the longest exposure time being used does not saturate the detector at any point.

## An example series of measurements:
  -Scatterer: CeO2
  -Exposure times: 0.1, 0.2, 0.3, 0.5 s
  
# Script usage:
After taking a series of measurements with varying exposure times, place the images in the same folder. In the script, change the following line to the location of the .tif images (* denotes a wildcard, so the line just looks for all files with the extension .tif in the directory). Files that are to be read in have their locations printed out below the cell.
![File location](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Detector%20linearity%20check/images/File%20location.png)

Change the following line to the exposure times corresponding to the images. The order of the images will be read in is printed below the cell, so ensure that the exposure times input on this line correspond to the exposure times of the images. 
![Exposure times](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Detector%20linearity%20check/images/exposure%20times.png)

The order of the images will be read in is printed below the cell, so ensure that the exposure times input on this line correspond to the exposure times of the images. In the example the following files are read in:

'multiple exposure times\\d450_CeO2_atten2_100ms-000000.tif', 
'multiple exposure times\\d450_CeO2_atten2_200ms-000000.tif', 
'multiple exposure times\\d450_CeO2_atten2_300ms-000000.tif', 
'multiple exposure times\\d450_CeO2_atten2_500ms-000000.tif'

Their corresponding exposure times are 0.1, 0.2, 0.3, and 0.5 seconds, so the exposure times on the specified line must be in that order. If the file names were instead in the following order:

'multiple exposure times\\d450_CeO2_atten2_100ms-000000.tif', 
'multiple exposure times\\d450_CeO2_atten2_200ms-000000.tif', 
'multiple exposure times\\d450_CeO2_atten2_500ms-000000.tif', 
'multiple exposure times\\d450_CeO2_atten2_300ms-000000.tif'

Then the line with the exposure times should be:
xp = np.array((0.1, 0.2, 0.5, 0.3))

Running the next cell takes the average (median) of the entire detector readout and plots it vs the exposure times. If this line is not straight then the detector response is nonlinear over the measured intensity range. Stop here and determine the issue with the detector. 
![Average detector intensity](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Detector%20linearity%20check/images/detector%20average.PNG)

The cell below it will plot the intensity of a single pixel over the measured series. Change the coordinate values for the x-y pixel position to change the pixel being checked (in the example shown it is checking pixel 900, 87).
![Single pixel intensity](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Detector%20linearity%20check/images/single%20pixel.PNG)

The next cell downsamples the detector images by taking the average (median) of blocks of pixels. Changing the two circled numbers changes the dimensions of the block of pixels which is averaged. For example, a 6x6 image that is downsampled with 2x2 blocks will return a 3x3 image with a new pixel corresponding to each colored block.

![Block reduction illustration](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Detector%20linearity%20check/images/block%20reduce%2C%20illustration.png)

Each downsampled block across the series is fit to a to a linear model and returns an image composed of the R<sup>2</sup> value of each fit at each position. 
![Detector r^2](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Detector%20linearity%20check/images/block%20reduce.png)

If the detector is responding linearly the image it produces will be fairly boring and contain an image of values near 1.  
![r^2 image](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Detector%20linearity%20check/images/r%5E2%20image.PNG)

The next cell plots a histogram of all the fit R<sup>2</sup> values. This is again not very interesting if the detector is functioning properly.
![r^2 histogram](https://github.com/jmsweng/X-ray-detector-gain-map/blob/main/Detector%20linearity%20check/images/r%5E2%20histogram.PNG)
