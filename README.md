# Galaxy Mergers Detection with Machine Learning  
Author: Alon Haviv  
  
## Intro:  
------  
This Python (Jupyter) notebook is part of My (Alon Haviv) thesis project in Galaxy Mergers Detection, for my MSc in Astrophysics, at the Hebrew University (HUJI).  
The code trains a Deep-Learning Neural Network (DNN) with a Convolutional Neural Network architecture (CNN), to analyze images of galaxies. The images (128x128) are taken from the Hubble Space Telescope (HST) project of "Cosmic Assembly Near-infrared Deep Extragalactic Legacy Survey" (CANDELS), and the program classifies the galaxies according to their merging stages (Pre-Merger, In-Merger, Post-Merger, No-Merger etc.). The training though is done over simulated images.  
  
## Training over simulated images:  
-------------------------------  
The program trains the model over a catalog of simulated images, extracted from the hydrodynamical cosmological simulation, the "Horizon-AGN". In this way we know the ground truth of the images. The catalog was prepared in advance by "Huertas-Company", and it contains snapshots of galaxy merger events and isolated galaxies, along with a data base for the galaxies (mass, redshift, time before/after merging etc.). In order to mimic the camera instruments of the HST (WFC3 and ACS-WFC), they performed the following steps (more details in my thesis):  
  
1. Each snapshot appears in 7 HST-CANDELS filters: The 3 NIR bands of the WFC3 (F160W, F125W, F105W) and the 4 visible bands of ACS-WFC (F850LP, F775W, F606W, F435W). And in 3 axes: X, Y, Z. Resulting in 21 images per snapshot.  
  
2. The magnitude of the flux of ewach image is recalibrated to match the HST-CANDELS system.  
  
3. Each image is convolved with the proper Point-Spread-Function (PSF) of the relevant instrument and band filter from CANDELS, and cropped to a size of 128×128 pixels.  
  
* Backgrounds (HST noise or real CANDELS background stamps) are added by our program.  
  
This is just a summary of the data processing. For more details please read the included thesis under the chapter of "Data". Both the Horizon-AGN simulation and the CANDELS project are available for everyone, so by following our recipe you can replicate our steps.  
  
## Requirements:  
-------------  
* The following libraries: keras (tensorflow), numpy, matplotlib, scipy, sklearn, astropy.  
* In addition, you better have a Cuda-able GPU and Cuda and Cudnn installed in your system.  
  
## Usage:  
------  
1. You should have 2 catalogs of simulated images and 2 image snapshots directories, as following:  
   	a. Major mergers snapshots image directory:  
   		MM-root-dir/  
		List of merger-event-id dirs/  
		Directories called "fits_of_flux" and "fits_of_flux_psf"/  
		List of merger-snapshots dirs/  
		21 fits files with the images (name patterns: \*_F105_x/y/z\*.fits)  
  
	b. Major mergers data catalog: A .fits file catalog with a data-table containing a row for every snapshot, and at least the following columns (with the names):  
		* "id_mer" (= merger-event-id dir's name),  
		* "n_gal" (= merger-snapshot dir's name),  
		* "phase" ("pre"/"mer"/"post"),  
		* "id_gal" (= \<n_gal\>_\<unique galaxy-id\>),  
		* "m_stellar_gal" (10^10 M_sol), "z_gal",  
		* "time_from_time_merge_init_mer" (Gyr).  
  
	c. No-Mergers snapshots image directory:  
		NM-root-dir/  
		List of galaxy-id dirs/  
		List of step/snapshot dirs/  
		Directories called "fits_of_flux" and "fits_of_flux_psf"/  
		21 fits files with the images (name patterns: \*_F105_x/y/z\*.fits)  
  
	d. No-Mergers data catalog: A .fits file catalog with a data-table containing a row for every snapshot, and at least the following columns:  
		* "gal_id_str" (= \<step\>_\<galaxy-id\>),  
		* "gal_m" (10^11 M_sol),  
		* "gal_z",  
  
3. If use choose to add real CANDELS stamps as backgrounds, you should also have the CANDELS fields tensors. They should be stored in .fits files. Update the following parameters (in the next box) with their respective values, and make sure to maintain the same filter-band orders as in "filterdict":  
	"candels_gds_paths", "candels_gdn_paths"  
  
4. In the code, set the "Setup" block with the desired parameter values (such as the files and directories locations, load pre-trained models etc.). The comments explain the meaning of each parameter. We recommend keeping the default values for most of them, except:  
	a. The paths: major_mergers_dir, major_mergers_catalogIndexFile, no_mergers_dir, no_mergers_catalogIndexFile, dataStructureSavedDir.  
	b. The "case" - Which classification do you want? A list of cases and their numbers appear in the next block. You can add new cases if you want.  
  
5. Run all the blocks one by one in order.  
  
6. At the end, if you allowed, a model will be trained and saved under "dataStructureSavedDir/case.../trained_model.../trained_model/" (it's the keras save)  
  
* You can the load the model and use it to classify real CANDELS images as you wish.  
  
## Files:  
------  
1. README:  
   This file.  
2. merger_no_merger_classifier.ipynb:  
   The Jupyter notebook for training a classifier.  
3. Galaxy Mergers with Machine Learning - Alon Haviv - Thesis.pdf  
   My Thesis. Contains theoretical material, all the results and more detailed explanation about the data and the work.  
  
