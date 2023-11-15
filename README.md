# Resample-raster-with-GDAL-Python
"In this script, you simply need to specify the input_file representing the raster you wish to resample and the output_file to designate where the resampled file will be saved. Additionally, you have the flexibility to adjust px and py to define the desired resolution of the output."


import os
from osgeo import gdal

# Specify the input and output folders
input_folder = 'D:/BayernAtlas/Würzburg'
output_folder = 'D:/BayernAtlas/Würzburg_resampled'

# Define the resolution
px = 0.5
py = 0.5

# Specify the desired output data type and compression
output_data_type = gdal.GDT_Byte
output_format = 'GTiff'
output_options = ['COMPRESS=LZW', 'TILED=YES']  # Use LZW compression

# Iterate through folders and files
for root, dirs, files in os.walk(input_folder):
    for file in files:
        if file.endswith('.tif') or file.endswith('.TIF'):
            # Form the full path for input and output files
            input_file = os.path.join(root, file)
            output_file = os.path.join(output_folder, file)

            # Open the input dataset
            ds = gdal.Open(input_file)

            # Resample the image
            gdal.Warp(output_file, ds, format=output_format, outputType=output_data_type,
                      xRes=px, yRes=py, resampleAlg='bilinear', srcNodata=None, dstNodata=None,
                      creationOptions=output_options)

            print(f"Original File Size ({file}): {os.path.getsize(input_file) / (1024 * 1024):.2f} MB")
            print(f"Resampled File Size ({file}): {os.path.getsize(output_file) / (1024 * 1024):.2f} MB")

            ds = None
