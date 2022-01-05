# GDAL+ OTB + QGIS in a virtual environment

The [Orfeo](https://www.orfeo-toolbox.org/tag/machine-learning/) machine learning library and the translator library for raster and vector geospatial data formats [GDAL](https://gdal.org/) do not play well with the open source geographic information system [QGIS](https://qgis.org) outside of the resource intensive GUI environment.
The systems have differing and partially conflicting dependencies, and the GUI environment makes rapid testing across the various tool boxes cumbersome.
This fact becomes apparent, for example, when developing a pipeline to perform vector based satellite image classification with a neural network under OTB.
This repository demonstrates how to setup a virtual computer that can support GDAL, OTB and QGIS functionality seamlessly. It also includes examples of how to use the setup to apply various classifiers including vector support machines, random forests and neural networks across GDAL, OTB and QGIS.

1 - VM <br>
Create a virtual computer. Choose Ubuntu 18.04 LTS, with at least 16GB of RAM. <br>

2- Get the repo files from this Github site. <br>
You will have all the files in the correct directory structure on your computer after the download. <br>

3 - Install QGIS. <br>
Make the file basics+qgis.sh executable.<br>

  	chmod +x basics+qgis.sh
	
Run that file. <br>

  	sh basics+qgis.sh

4 - Install OTB in a virtual environment with conda. <br>
Make the file conda-packages.sh executable. <br>

  	chmod +x conda-packages.sh
	
Run that file. <br>

  	sh conda-packages.sh
	
5 - Create a conda environment with defined dependencies.<br>
(OTB 7.2 requires python 3.7, for example. Also check the path to the environment.yml file)

	conda env create -f environment.yml
	

6 - Customize <br>
Add other libraries to the OTB environment as needed <br>

	conda install -c conda-forge pillow
	conda install -c conda-forge geopandas
(Enable the OTB environment to install geopandas… the install may take some time…)<br>
	
7 – Setup the directory structure <br>
The directories code, data, results should be ready made in your repository directory.
Put the rasterimages and vectorfiles directories into the data directory. Place image assets (.tif) in the
rasterimages directory and vector assets (.shp) into the vectorfiles directory. Add the
corresponding .dbf, .prj, .shx files for each .shp file.

  
8 - Test<br>
a) Test QGIS

Run the qgis test in the base environment <br>

  	python3 qgis_test.py
	

b) Test OTB

Activate the OTB environment <br>

  	conda activate OTB
	
Run the otb test <br>

  	python3 otb_test.py
  
You can now use QGIS in the base installation and enable the conda environment to access OTB functionality. 
You can also move across environments to access libraries from either environment with python scripts as outlined below (see vector_classify_top.sh): <br>

  	echo "Starting OTB-QGIS pipeline...\n\n" 
  	conda run -n OTB python3 /home/code/otb_code_A.py 
  	python3 /home/code/qgis_code_A.py 
  	conda run -n OTB python3 /home/code/otb_code_B.py 
  	python3 /home/code/qgis_code_B.py 
	 
  
Since the directory structure you setup will be identical in both the QGIS and OTB environments, intermediate filed produced will be available to processes running in either environment, allowing for data to be shared. All settings across the script modules are stored in the 'settings.txt' resource file and imported to the individual modules.
