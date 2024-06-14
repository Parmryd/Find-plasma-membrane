**Find_plasma_membrane.ijm**


The macro was first used in https://doi.org/10.1016/j.bbamem.2022.184094 and is thoroughly described in https://doi.org/10.1016/j.softx.2023.101570.

Requires the Plugins StackReg and Translation alignment that can be found at https://github.com/fiji-BIG/StackReg and http://punias.free.fr/ImageJ/Translation_Alignment.java

1) Open and load an image from the first channel. If the input is a stack of images the macro defaults to the central slice; the user confirms/changes; the selected image is copied. The slice selection is saved in the ROI manager as Slicenumber.

2) The image/image stack from the second channel is automatically loaded; similar names need to be used. This is settable, e.g. 483/442, ch00/ch01, NameLongerR/NameShorterR. If stack; the matching slice from the first stack is copied.

3) Option to crop images, e.g. to include only a single cell. The cropped image is saved in the ROI manager as CropROI.

4) The single images from each channel are combined into a stack that is used to create the ROI.

5) Registration of the images performed and the user is asked to check the results visually. The offset is saved as point selection.

6) Creation of a weighted smoothed (default Gau 0.8, settable) combination of the two images. The weighting gives the two input images equal prominence. The weighted image is added to the stack providing a choice between 3 images, fluorophore 1, 2 or the combined image.

7) Manual selection of an initial ROI using a few sequential points that are connected by a line, saved in the ROI manager as Initial.

8) The initial ROI is used to find the centre of the cell, which is saved as a point selection in the ROI manager as Centre.

9) A search for the maximal intensity pixel around the manually selected points. There are three search options: circular, a perpendicular line or a line towards the cell centre. New points are marked at the local intensity peaks. The search parameters used (type of search and range) for the small number of manually marked points and the more numerous interpolated points are set separately.

10) The updated ROI is saved in the ROI manager as Initial Max.

11) The user either approves or changes the new positions of the point. If changed a new ROI is saved in the ROI manager as InitialMax_Edt creating a record of any user intervention.

12) New points are interpolated between the existing ones at a spacing selected by the user and are saved in the ROI manager as Memb interp.

13) A search for the maximal intensity pixel around the interpolated points as in 9. The resulting ROI is saved in the ROI manager as Mmb Intp Mx.

14) Option to manually delete or move points. If changed a new ROI is saved in the ROI manager as Mmb Intp Mx_Edt

15) The aligned stack images are saved with \'94Algnd\'94 included in the name e.g. Test_ch00Stack_Algnd-Cell3.tif. Since the original image might contain more than one cell and each cell may have been previously analyzed the name includes a reference number, 3 in the example name, based on a check of the file names already used.

16) The complete ROI manager is automatically saved.

17) If the original images were cropped, a CropROI overlay will be added to original images, showing which cell or image area have already been analyzed.




**Measure_plasma_membrane.ijm**

The macro was first used in https://doi.org/10.1016/j.bbamem.2022.184094 and is thoroughly described in https://doi.org/10.1016/j.softx.2023.101570.

A stack of three images generated by Find_plasma_membrane.ijm is expected as input.

1) Images to be analysed are opened.

2) The macro automatically looks in the folder where the images were and loads the accompanying ROIs that were created in Find_plasma_membrane.ijm.

3) The user is prompted to mark a representative background area. This could be based on a band from the PM but the presence of nearby cells makes if preferable to manually select the area. The area is saved.

4) The centre coordinates are read.

5) The final plasma membrane ROI or, if the ROI was manually edited, the edited version is read.

6) The gap between the two end points of the plasma membrane is filled by interpolation using a weighted combination of the radii of the two endpoints. This is effective for gaps of up to half of the cell.

7) A three-point polygon covering the parts of the distance map that will originate from the gap is created. The polygon is based on the centre point plus an extension from the centre point through the two end points. The polygon is added to the ROI manager as DTzap.

8) A one-pixel-wide binary image with the gap filled is created and inverted.

9) A 32-bit distance map is made from the gap-filled plasma membrane ROI. Note all distances have positive values.

10) The whole cell ROI is selected and pixels inside the cell multiplied with -1, producing negative distance values inwards from the PM.

11) The gap related areas of the distance map are voided using the three-point polygon.

12) Intensity thresholding is used to select one-pixel-wide ROIs at any distance-range inwards or outwards from the plasma membrane and measurements are made from each. Summary data for the whole thresholded area is added to the Results.

13) Sequential measurements are made for each pixel in the ROI. This requires finding the start pixel. To this end the thresholded Distance Map is reduced to a binary skeleton, this is then convolved leaving each pixel showing that it is part of the ROI and adding the number of binary neighbours - the end points have one neighbour and a search for an end point is made.

14) Measurements of fluorescence intensity from the two channels are made for every pixel and, after a background intensity subtraction, are used to calculate GP-values.
 
15) All measurements and all relevant information are added to the Results.

16) The user is prompted to add a text note the Results Table.

17) All the new ROIs are added to the ROI Manager, which can be saved, preserving a complete record the analysis.
