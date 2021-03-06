# Lidar Visualization
Lidar Data Visualization in Browser

# References

## https://pdal.io/
PDAL is a C++ BSD library for translating and manipulating point cloud data. It is very much like the GDAL library which handles raster and vector data. 

    - tutorial: https://pdal.io/tutorial/iowa-entwine.html

## https://greyhound.io/
Greyhound is an open source software from Hobu, Inc. that allows clients to query and stream progressive point cloud data over the network.

## https://entwine.io/
Entwine is a data organization library for massive point clouds, designed to conquer datasets of trillions of points as well as desktop-scale point clouds.

    - entwine demo: https://usgs.entwine.io/
    - tutorial: https://github.com/connormanning/entwine


## https://github.com/potree/potree
Potree is a free open-source WebGL based point cloud renderer for large point clouds, developed at the Institute of Computer Graphics and Algorithms, TU Wien.

## http://plas.io/
plas.io is a WebGL HTML5 point cloud renderer that speaks ASPRS LAS and LASzip compressed LAS. You can find the software for it at plasiojs.io and https://github.com/hobu/plasio-ui


# Datasets

## OCM Lidar Data
     https://coast.noaa.gov/htdata/lidar1_z/
     https://coast.noaa.gov/htdata/lidar2_z/geoid12b/data/8688/

## Download Instructions
```sh
    wget -np -r -nH -L --cut-dirs=2 https://coast.noaa.gov/htdata/lidar2_z/geoid12b/data/8688/ 
```

## Build Entwine Format
Demo Dataset
```sh
docker run -it -v $(pwd)/data:/data \
    connormanning/entwine build \
    -i https://entwine.io/data/red-rocks.laz \
    -o /data/entwine/red-rocks
```
OCM Dataset
```sh
rm all_files.csv

docker run -it -v $(pwd)/data:/data \
    -v /Volumes/shaunak_2tb/OCM_Lidar_data:/ocm_data \
    connormanning/entwine build \
    -i /ocm_data/geoid12b/data/4937/ \
    -o /ocm_data/entwine_output/4937 \
    -r EPSG:4269 EPSG:3857 \
    --hammer \
    --scale 0.01 \
    -f
```

docker run -it -v $(pwd)/data:/data \
    -v /Volumes/shaunak_2tb/OCM_Lidar_data:/ocm_data \
    connormanning/entwine scan \
    -i '/ocm_data/geoid12b/data/1/*.laz' \
    -r EPSG:3857


## Locally Serve Entwine

```sh
    docker run -it -v /Volumes/shaunak_2tb/OCM_Lidar_data/entwine_output:/var/www -p 8080:8080 connormanning/http-server
```

## Launch PDAL Container and Bash into it

```sh
    docker run -it -v $(pwd)/data:/data -v /Volumes/shaunak_2tb/OCM_Lidar_data:/ocm_data pdal/pdal bash
```

## Alternatively Launch miniconda environment in docker

```sh
    docker run -it -v $(pwd)/data:/data -v /Volumes/shaunak_2tb/OCM_Lidar_data:/ocm_data continuumio/miniconda3 bash
```


## AWS Lambda image for PDAL
    - https://github.com/PDAL/lambda