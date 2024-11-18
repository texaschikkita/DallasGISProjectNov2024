```python
import pandas as pd

# Define the data
data = [
    {"Street": "Seaton Dr", "House Number": 4337, "Fence Presence": "Yes", "Fence Type": "Standard", "Nearby Amenity": "Green Spaces", "Address": "4337 Seaton Dr, Dallas, TX 75216"},
    {"Street": "Kyser St", "House Number": 4323, "Fence Presence": "Yes", "Fence Type": "Standard", "Nearby Amenity": "None", "Address": "43323 Kyser St, Dallas, TX 75216"},
    {"Street": "Kyser St", "House Number": 4325, "Fence Presence": "Yes", "Fence Type": "Standard", "Nearby Amenity": "None", "Address": "4325 Kyser St, Dallas, TX 75216"},
    {"Street": "Kyser St", "House Number": 4326, "Fence Presence": "Yes", "Fence Type": "Standard", "Nearby Amenity": "None", "Address": "4326 Kyser St, Dallas, TX 75216"},
    {"Street": "Kyser St", "House Number": 4328, "Fence Presence": "Yes", "Fence Type": "Standard", "Nearby Amenity": "None", "Address": "4328 Kyser St, Dallas, TX 75216"},
    {"Street": "Kyser St", "House Number": 4329, "Fence Presence": "Yes", "Fence Type": "Head Height", "Nearby Amenity": "None", "Address": "4329 Kyser St, Dallas, TX 75216"},
    {"Street": "Kyser St", "House Number": 4331, "Fence Presence": "Yes", "Fence Type": "Standard", "Nearby Amenity": "None", "Address": "4331 Kyser St, Dallas, TX 75216"},
    {"Street": "Kyser St", "House Number": 4335, "Fence Presence": "Yes", "Fence Type": "Standard", "Nearby Amenity": "None", "Address": "4335 Kyser St, Dallas, TX 75216"},
    {"Street": "Kyser St", "House Number": 4337, "Fence Presence": "Yes", "Fence Type": "Standard", "Nearby Amenity": "None", "Address": "4337 Kyser St, Dallas, TX 75216"},
    {"Street": "Vandervort Dr", "House Number": 4324, "Fence Presence": "Yes", "Fence Type": "Standard", "Nearby Amenity": "None", "Address": "4324 Vandervort Dr, Dallas TX 75216"},
    {"Street": "Vandervort Dr", "House Number": 4326, "Fence Presence": "Yes", "Fence Type": "Standard", "Nearby Amenity": "None", "Address": "4326 Vandervort Dr, Dallas TX 75216"},
    {"Street": "Vandervort Dr", "House Number": 4328, "Fence Presence": "Yes", "Fence Type": "Standard", "Nearby Amenity": "None", "Address": "4328 Vandervort Dr, Dallas TX 75216"},
    {"Street": "Vandervort Dr", "House Number": 4332, "Fence Presence": "Yes", "Fence Type": "Standard", "Nearby Amenity": "None", "Address": "4332 Vandervort Dr, Dallas TX 75216"},
    {"Street": "Vandervort Dr", "House Number": 4336, "Fence Presence": "Yes", "Fence Type": "Standard", "Nearby Amenity": "None", "Address": "4336 Vandervort Dr, Dallas TX 75216"},
    {"Street": "Vandervort Dr", "House Number": 4340, "Fence Presence": "Yes", "Fence Type": "Standard", "Nearby Amenity": "None", "Address": "4340 Vandervort Dr, Dallas TX 75216"},
    {"Street": "Vandervort Dr", "House Number": 4342, "Fence Presence": "Yes", "Fence Type": "Standard", "Nearby Amenity": "None", "Address": "4342 Vandervort Dr, Dallas TX 75216"},
    {"Street": "Linfield Rd", "House Number": 3506, "Fence Presence": "No", "Fence Type": "None", "Nearby Amenity": "Bus Stop", "Address": "3506 Linfield Rd, Dallas, TX 75216"},
    {"Street": "Linfield Rd", "House Number": 3516, "Fence Presence": "Yes", "Fence Type": "Standard", "Nearby Amenity": "None", "Address": "3516 Linfield Rd, Dallas, TX 75216"},
    {"Street": "Linfield Rd", "House Number": 3520, "Fence Presence": "Yes", "Fence Type": "Standard", "Nearby Amenity": "None", "Address": "3520 Linfield Rd, Dallas, TX 75216"},
    {"Street": "Linfield Rd", "House Number": 3522, "Fence Presence": "Yes", "Fence Type": "Standard", "Nearby Amenity": "None", "Address": "3522 Linfield Rd, Dallas, TX 75216"},
    {"Street": "Kelloch Rd", "House Number": 3506, "Fence Presence": "No", "Fence Type": "None", "Nearby Amenity": "Bus Stop", "Address": "3506 Kellogg Ave, Dallas, TX 75216"},
    {"Street": "Kelloch Rd", "House Number": 3516, "Fence Presence": "Yes", "Fence Type": "Standard", "Nearby Amenity": "None", "Address": "3516 Kellogg Ave, Dallas, TX 75216"},
    {"Street": "Royal Crest", "House Number": "Apartments", "Fence Presence": "No", "Fence Type": "None", "Nearby Amenity": "Bus Stop, Green Space", "Address": "3558 Wilhurt Ave, Dallas, TX 75216"},
    {"Street": "Seaton Park", "House Number": "N/A", "Fence Presence": "No", "Fence Type": "None", "Nearby Amenity": "Green Spaces", "Address": "3200 Seaton Dr, Dallas, TX 75216"},
    {"Street": "Fruitdale WIC", "House Number": "N/A", "Fence Presence": "No", "Fence Type": "None", "Nearby Amenity": "Green Spaces", "Address": "4408 Vandervort Dr, Dallas, TX 75216"}
]

# Convert the data to a DataFrame
df = pd.DataFrame(data)

# Save the DataFrame to a CSV file
output_path = "dallas_walk.csv"
df.to_csv(output_path, index=False)

output_path

```




    'dallas_walk.csv'




```python
from geopy.geocoders import Nominatim
from geopy.exc import GeocoderTimedOut

# Initialize geolocator
geolocator = Nominatim(user_agent="geo_locator")

# Function to get latitude and longitude
def geocode_address(address):
    try:
        location = geolocator.geocode(address, timeout=10)
        if location:
            return location.latitude, location.longitude
        else:
            return None, None
    except GeocoderTimedOut:
        return None, None

# Add coordinates to the DataFrame
df['Latitude'], df['Longitude'] = zip(*df['Address'].apply(geocode_address))

# Save the updated DataFrame with coordinates
output_with_coords_path = "dallas_walk_w_coord.csv"
df.to_csv(output_with_coords_path, index=False)

output_with_coords_path

```




    'dallas_walk_w_coord.csv'



Added the rest of the coordinate enrtries and saved as dallas_walk_w_coord.csv.


```python
import geopandas as gpd
from shapely.geometry import Point

# Load the updated CSV with coordinates
csv_file_path = "dallas_walk_w_coord.csv"
df = pd.read_csv(csv_file_path)

# Ensure Latitude and Longitude are numeric
df['Latitude'] = pd.to_numeric(df['Latitude'], errors='coerce')
df['Longitude'] = pd.to_numeric(df['Longitude'], errors='coerce')

# Create geometry column using Latitude and Longitude
df['geometry'] = df.apply(lambda row: Point(row['Longitude'], row['Latitude']), axis=1)

# Convert to GeoDataFrame
gdf = gpd.GeoDataFrame(df, geometry='geometry', crs="EPSG:4326")

# Save to shapefile
shapefile_path = "dallas_walk1.shp"
gdf.to_file(shapefile_path)

shapefile_path

```

    C:\Users\jessi\AppData\Local\Temp\ipykernel_32936\3360649122.py:20: UserWarning: Column names longer than 10 characters will be truncated when saved to ESRI Shapefile.
      gdf.to_file(shapefile_path)
    c:\ProgramData\miniforge3\envs\arcgis\Lib\site-packages\pyogrio\raw.py:723: RuntimeWarning: Normalized/laundered field name: 'House Number' to 'House Numb'
      ogr_write(
    c:\ProgramData\miniforge3\envs\arcgis\Lib\site-packages\pyogrio\raw.py:723: RuntimeWarning: Normalized/laundered field name: 'Fence Presence' to 'Fence Pres'
      ogr_write(
    c:\ProgramData\miniforge3\envs\arcgis\Lib\site-packages\pyogrio\raw.py:723: RuntimeWarning: Normalized/laundered field name: 'Nearby Amenity' to 'Nearby Ame'
      ogr_write(
    




    'dallas_walk1.shp'




```python
import geopandas as gpd
import pandas as pd
from shapely.geometry import Point

# Data for the shapefile
data = {
    "Address": [
        "4506 Hedgdon Dr, Dallas, TX 75216",
        "4510 Hedgdon Dr, Dallas, TX 75216",
        "4512 Hedgdon Dr, Dallas, TX 75216",
        "4516 Hedgdon Dr, Dallas, TX 75216",
        "4519 Hedgdon Dr, Dallas, TX 75216",
        "4520 Hedgdon Dr, Dallas, TX 75216",
        "4523 Hedgdon Dr, Dallas, TX 75216",
        "4527 Hedgdon Dr, Dallas, TX 75216",
        "4530 Hedgdon Dr, Dallas, TX 75216",
        "4531 Hedgdon Dr, Dallas, TX 75216",
        "4533 Hedgdon Dr, Dallas, TX 75216",
        "4534 Hedgdon Dr, Dallas, TX 75216",
        "4535 Hedgdon Dr, Dallas, TX 75216",
        "4612 Hedgdon Dr, Dallas, TX 75216",
        "4615 Hedgdon Dr, Dallas, TX 75216",
        "4618 Hedgdon Dr, Dallas, TX 75216",
        "4622 Hedgdon Dr, Dallas, TX 75216",
        "3401 Fifty-First St, Dallas, TX 75216",
        "3500 Fifty-First St, Dallas, TX 75216",
        "4534 Humphrey Dr, Dallas, TX 75216",
        "4538 Humphrey Dr, Dallas, TX 75216",
        "3227 Mallory Dr, Dallas, TX 75216",
        "3502 Mallory Dr, Dallas, TX 75216",
    ],
    "Latitude": [
        32.70669, 32.706475, 32.706238, 32.70599, 32.705816, 32.705779,
        32.705648, 32.705337, 32.705394, 32.705084, 32.704844, 32.705137,
        32.704629, 32.704235, 32.704356, 32.703861, 32.703576, 32.703386,
        32.703776, 32.704827, 32.704592, 32.707176, 32.707412
    ],
    "Longitude": [
        -96.761231, -96.761163, -96.761041, -96.760947, -96.761526, -96.760847,
        -96.761496, -96.761349, -96.760712, -96.761256, -96.76113, -96.760587,
        -96.761047, -96.76021, -96.760938, -96.760037, -96.759974, -96.763752,
        -96.761748, -96.76226, -96.76216, -96.76203, -96.758619
    ],
    "Neighborhood": ["Cedar Crest"] * 23,
    "Elementary": ["Pease Elementary"] * 23,
    "Middle_Sch": ["Zumwalt Middle"] * 23,
    "High_Schoo": ["Roosevelt High"] * 23,
    "Dollar_Val": [
        155000, 165340, 179110, 205900, 171000, 192110, 154000, 207900, 146500,
        180800, 253700, 179800, 205300, 175300, 206000, 169000, 248500, 138100,
        277300, 124800, 145600, 134600, 221000
    ],
    "SqFt": [
        1224, 1428, 1140, 1315, 1100, 1401, 1397, 1518, 1183, 1221, 2664, 1126,
        1911, 840, 1622, 1092, 1835, 1014, 2359, 899, 932, 1036, 1962
    ],
    "Image_Path": [
        f"C:\\Users\\jessi\\Desktop\\Dallas Walkability\\dallas project images GIS-selected\\{addr.split()[0]}"
        for addr in [
            "4506", "4510", "4512", "4516", "4519", "4520", "4523", "4527", "4530",
            "4531", "4533", "4534", "4535", "4612", "4615", "4618", "4622", "3401",
            "3500", "4534", "4538", "3227", "3502"
        ]
    ],
}

# Convert data to a GeoDataFrame
df = pd.DataFrame(data)
geometry = [Point(xy) for xy in zip(df["Longitude"], df["Latitude"])]
gdf = gpd.GeoDataFrame(df, geometry=geometry, crs="EPSG:4326")  # WGS84 CRS

# Save to a shapefile
output_path = r"C:\Users\jessi\Desktop\Dallas Walkability\Dallas_Addresses.shp"
gdf.to_file(output_path)

print(f"Shapefile created at {output_path}")

```

    Shapefile created at C:\Users\jessi\Desktop\Dallas Walkability\Dallas_Addresses.shp
    

    C:\Users\jessi\AppData\Local\Temp\ipykernel_24296\3584045192.py:74: UserWarning: Column names longer than 10 characters will be truncated when saved to ESRI Shapefile.
      gdf.to_file(output_path)
    c:\ProgramData\miniforge3\envs\arcgis\Lib\site-packages\pyogrio\raw.py:723: RuntimeWarning: Normalized/laundered field name: 'Neighborhood' to 'Neighborho'
      ogr_write(
    


```python
import zipfile
import os

```


```python
import zipfile
import os

# Define the path to the uploaded file and extraction directory
uploaded_file_path = 'C:/Users/jessi/Desktop/3227 Mallory.zip'
extraction_dir = 'C:/Users/jessi/Desktop/images_extracted/'

# Extract the zip file
os.makedirs(extraction_dir, exist_ok=True)
with zipfile.ZipFile(uploaded_file_path, 'r') as zip_ref:
    zip_ref.extractall(extraction_dir)

# List the extracted files to verify contents
extracted_files = []
for root, dirs, files in os.walk(extraction_dir):
    for file in files:
        extracted_files.append(os.path.join(root, file))

extracted_files

```




    ['C:/Users/jessi/Desktop/images_extracted/3227 Mallory.JPG',
     'C:/Users/jessi/Desktop/images_extracted/3400 Fifty-First.JPG',
     'C:/Users/jessi/Desktop/images_extracted/3401 Fifty-First.jpg',
     'C:/Users/jessi/Desktop/images_extracted/3500 Fifty-First.JPG',
     'C:/Users/jessi/Desktop/images_extracted/3502 Mallory.JPG',
     'C:/Users/jessi/Desktop/images_extracted/4506 Hedgdon.JPG',
     'C:/Users/jessi/Desktop/images_extracted/4510 Hedgdon.JPG',
     'C:/Users/jessi/Desktop/images_extracted/4512 Hedgdon.JPG',
     'C:/Users/jessi/Desktop/images_extracted/4519 Hedgdon.JPG',
     'C:/Users/jessi/Desktop/images_extracted/4520 Hedgdon.JPG',
     'C:/Users/jessi/Desktop/images_extracted/4523 Hedgdon.JPG',
     'C:/Users/jessi/Desktop/images_extracted/4527 Hedgdron.JPG',
     'C:/Users/jessi/Desktop/images_extracted/4530 Hedgdon.JPG',
     'C:/Users/jessi/Desktop/images_extracted/4530 Humphrey.JPG',
     'C:/Users/jessi/Desktop/images_extracted/4531 Hedgdon.JPG',
     'C:/Users/jessi/Desktop/images_extracted/4531 Humphrey.JPG',
     'C:/Users/jessi/Desktop/images_extracted/4533 Hedgdon.JPG',
     'C:/Users/jessi/Desktop/images_extracted/4534 Hedgdon.JPG',
     'C:/Users/jessi/Desktop/images_extracted/4534 Humphrey.JPG',
     'C:/Users/jessi/Desktop/images_extracted/4535 Hedgdon.JPG',
     'C:/Users/jessi/Desktop/images_extracted/4538 Humphrey.JPG',
     'C:/Users/jessi/Desktop/images_extracted/4612 Hedgdon.JPG',
     'C:/Users/jessi/Desktop/images_extracted/4615 Hedgdon.JPG',
     'C:/Users/jessi/Desktop/images_extracted/4618 Hedgdon.JPG',
     'C:/Users/jessi/Desktop/images_extracted/4622 Hedgdon.JPG']



extraction_dir = 'C:/Users/jessi/Desktop/images_extracted/'



```python
# import exifread
# import pandas as pd

# def extract_gps_data(image_path):
#     """Extract GPS coordinates from an image's EXIF metadata."""
#     with open(image_path, 'rb') as img_file:
#         tags = exifread.process_file(img_file)
        
#         # Extract GPS metadata
#         gps_latitude = tags.get('GPS GPSLatitude')
#         gps_latitude_ref = tags.get('GPS GPSLatitudeRef')
#         gps_longitude = tags.get('GPS GPSLongitude')
#         gps_longitude_ref = tags.get('GPS GPSLongitudeRef')
        
#         if gps_latitude and gps_latitude_ref and gps_longitude and gps_longitude_ref:
#             # Convert to degrees
#             lat = convert_to_degrees(gps_latitude, gps_latitude_ref.values[0])
#             lon = convert_to_degrees(gps_longitude, gps_longitude_ref.values[0])
#             return lat, lon
#         return None, None

# def convert_to_degrees(value, ref):
#     """Convert GPS coordinates from degrees, minutes, and seconds to decimal degrees."""
#     d, m, s = [float(x.num) / float(x.den) for x in value.values]
#     decimal = d + (m / 60.0) + (s / 3600.0)
#     if ref in ['S', 'W']:
#         decimal = -decimal
#     return decimal

# # Initialize list to hold metadata
# metadata_list = []

# # Process each image file
# for image_file in extracted_files:
#     if image_file.lower().endswith(('.jpg', '.jpeg', '.png')):
#         lat, lon = extract_gps_data(image_file)
#         metadata_list.append({
#             'FileName': os.path.basename(image_file),
#             'Latitude': lat,
#             'Longitude': lon
#         })

# # Convert metadata to a DataFrame
# metadata_df = pd.DataFrame(metadata_list)

# # Save metadata to a CSV file
# output_csv_path = '/mnt/data/image_metadata.csv'
# metadata_df.to_csv(output_csv_path, index=False)

# import ace_tools as tools; tools.display_dataframe_to_user(name="Extracted Image Metadata", dataframe=metadata_df)

```


```python
from PIL import Image
from PIL.ExifTags import TAGS, GPSTAGS
import csv
import os

def extract_gps_info(exif_data):
    """Extract GPS information from EXIF data."""
    gps_info = {}
    for key in exif_data.keys():
        name = TAGS.get(key)
        if name == "GPSInfo":
            for gps_key in exif_data[key].keys():
                gps_name = GPSTAGS.get(gps_key)
                gps_info[gps_name] = exif_data[key][gps_key]
    return gps_info

def convert_to_degrees(value):
    """Convert GPS coordinates to decimal degrees."""
    d = value[0].numerator / value[0].denominator
    m = value[1].numerator / value[1].denominator
    s = value[2].numerator / value[2].denominator
    return d + (m / 60.0) + (s / 3600.0)

def get_coordinates(gps_info):
    """Get latitude and longitude from GPS info."""
    if "GPSLatitude" in gps_info and "GPSLongitude" in gps_info:
        lat = convert_to_degrees(gps_info["GPSLatitude"])
        lon = convert_to_degrees(gps_info["GPSLongitude"])
        if gps_info["GPSLatitudeRef"] == "S":
            lat = -lat
        if gps_info["GPSLongitudeRef"] == "W":
            lon = -lon
        return lat, lon
    return None, None

# Define the directory containing the extracted images
image_directory = 'C:/Users/jessi/Desktop/images_extracted/'

# Prepare to save metadata to a CSV
output_csv = 'C:/Users/jessi/Desktop/image_metadata.csv'
metadata_list = []

for image_file in os.listdir(image_directory):
    if image_file.lower().endswith(('.jpg', '.jpeg', '.png')):
        image_path = os.path.join(image_directory, image_file)
        with Image.open(image_path) as img:
            exif_data = img._getexif()
            if exif_data:
                gps_info = extract_gps_info(exif_data)
                lat, lon = get_coordinates(gps_info)
                metadata_list.append({
                    'FileName': image_file,
                    'Latitude': lat,
                    'Longitude': lon
                })

# Write metadata to CSV
with open(output_csv, 'w', newline='') as csvfile:
    fieldnames = ['FileName', 'Latitude', 'Longitude']
    writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
    writer.writeheader()
    writer.writerows(metadata_list)

print(f"Metadata has been saved to {output_csv}")

```

    Metadata has been saved to C:/Users/jessi/Desktop/image_metadata.csv
    


```python
import os  

csv_path = 'C:/Users/jessi/Desktop/image_metadata.csv'  

if os.path.exists(csv_path):  
    print("File exists.")  
else:  
    print("File does not exist.")
```

    File does not exist.
    


```python
import os  

csv_path = r'C:\Users\jessi\Desktop\dp\image_metadata.csv'  

if os.path.exists(csv_path):  
    print("File exists.")  
else:  
    print("File does not exist.")
```

    File does not exist.
    


```python
import pandas as pd  
import os  

# Correct path to your CSV file and image folder  
csv_path = r'C:\Users\jessi\Desktop\dp\image_metadata.csv'  
image_folder = r'C:\Users\jessi\Desktop\images_extracted'  

# Load the CSV  
metadata_df = pd.read_csv(csv_path)  

# Add the ImagePath column  
metadata_df['ImagePath'] = metadata_df['FileName'].apply(lambda x: os.path.join(image_folder, x))  

# Save the updated CSV  
updated_csv_path = r'C:\Users\jessi\Desktop\image_metadata_with_paths.csv'  
metadata_df.to_csv(updated_csv_path, index=False)  

print(f"Updated CSV saved to {updated_csv_path}")
```


    ---------------------------------------------------------------------------

    PermissionError                           Traceback (most recent call last)

    Cell In[8], line 16
         14 # Save the updated CSV  
         15 updated_csv_path = r'C:\Users\jessi\Desktop\image_metadata_with_paths.csv'  
    ---> 16 metadata_df.to_csv(updated_csv_path, index=False)  
         18 print(f"Updated CSV saved to {updated_csv_path}")
    

    File c:\ProgramData\miniforge3\envs\arcgis\Lib\site-packages\pandas\core\generic.py:3772, in NDFrame.to_csv(self, path_or_buf, sep, na_rep, float_format, columns, header, index, index_label, mode, encoding, compression, quoting, quotechar, lineterminator, chunksize, date_format, doublequote, escapechar, decimal, errors, storage_options)
       3761 df = self if isinstance(self, ABCDataFrame) else self.to_frame()
       3763 formatter = DataFrameFormatter(
       3764     frame=df,
       3765     header=header,
       (...)
       3769     decimal=decimal,
       3770 )
    -> 3772 return DataFrameRenderer(formatter).to_csv(
       3773     path_or_buf,
       3774     lineterminator=lineterminator,
       3775     sep=sep,
       3776     encoding=encoding,
       3777     errors=errors,
       3778     compression=compression,
       3779     quoting=quoting,
       3780     columns=columns,
       3781     index_label=index_label,
       3782     mode=mode,
       3783     chunksize=chunksize,
       3784     quotechar=quotechar,
       3785     date_format=date_format,
       3786     doublequote=doublequote,
       3787     escapechar=escapechar,
       3788     storage_options=storage_options,
       3789 )
    

    File c:\ProgramData\miniforge3\envs\arcgis\Lib\site-packages\pandas\io\formats\format.py:1186, in DataFrameRenderer.to_csv(self, path_or_buf, encoding, sep, columns, index_label, mode, compression, quoting, quotechar, lineterminator, chunksize, date_format, doublequote, escapechar, errors, storage_options)
       1165     created_buffer = False
       1167 csv_formatter = CSVFormatter(
       1168     path_or_buf=path_or_buf,
       1169     lineterminator=lineterminator,
       (...)
       1184     formatter=self.fmt,
       1185 )
    -> 1186 csv_formatter.save()
       1188 if created_buffer:
       1189     assert isinstance(path_or_buf, StringIO)
    

    File c:\ProgramData\miniforge3\envs\arcgis\Lib\site-packages\pandas\io\formats\csvs.py:240, in CSVFormatter.save(self)
        236 """
        237 Create the writer & save.
        238 """
        239 # apply compression and byte/text conversion
    --> 240 with get_handle(
        241     self.filepath_or_buffer,
        242     self.mode,
        243     encoding=self.encoding,
        244     errors=self.errors,
        245     compression=self.compression,
        246     storage_options=self.storage_options,
        247 ) as handles:
        248     # Note: self.encoding is irrelevant here
        249     self.writer = csvlib.writer(
        250         handles.handle,
        251         lineterminator=self.lineterminator,
       (...)
        256         quotechar=self.quotechar,
        257     )
        259     self._save()
    

    File c:\ProgramData\miniforge3\envs\arcgis\Lib\site-packages\pandas\io\common.py:859, in get_handle(path_or_buf, mode, encoding, compression, memory_map, is_text, errors, storage_options)
        854 elif isinstance(handle, str):
        855     # Check whether the filename is to be opened in binary mode.
        856     # Binary mode does not support 'encoding' and 'newline'.
        857     if ioargs.encoding and "b" not in ioargs.mode:
        858         # Encoding
    --> 859         handle = open(
        860             handle,
        861             ioargs.mode,
        862             encoding=ioargs.encoding,
        863             errors=errors,
        864             newline="",
        865         )
        866     else:
        867         # Binary mode
        868         handle = open(handle, ioargs.mode)
    

    PermissionError: [Errno 13] Permission denied: 'C:\\Users\\jessi\\Desktop\\image_metadata_with_paths.csv'

