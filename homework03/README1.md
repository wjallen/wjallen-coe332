# Homework 03: Prelude to Containers
## Description
The purpose of this project is to explore and introduction to Docker containers and Dockerfiles. In this repository, I include a Dockerfile from which a Docker image is generated. While the user can pull the image from Docker Hub, I will include instructions on how to build the Docker image and run the program with specified data.  

The main program computes a number of interesting statistics from the meteorite landing dataset, including the closest meteorite to TACC, the year with the most meteorite landings, and the ratio of the masses of the largest to smallest meteorite.  

This application uses docker, so please ensure you have that library.  

## Building the Image from Dockerfile
In this repository, I include the Dockerfile as well as all of the source code. First, navigate into the homework03 directory. Then, to build the image, run the following:  
```docker build -t <username>/hw3:1.0 .```   
Make sure you specify your Docker Hub username in *<username>*. This might take a few moments, as it retrieves both my image and the base image (python 3.12).  

Alternatively, you could also pull the image from Docker Hub with the following, since my image is public:  
```docker pull USERNAME/hw3:1.0```

## Getting the Input Data
While we build the image with all of the necessary source code, we still need to mount the data at runtime, which is not included within the image directly. The raw CSV data can be found at the following link:  
https://data.nasa.gov/Space-Science/Meteorite-Landings/gh4g-9sfh/about_data

To ensure the image can find the Meteorite Landing dataset, go to the link above, click "Export", and export the file as a CSV. Move the downloaded CSV file into the directory homework03.  

The file will by default have the date of download in the file name. Rename the file to "Meteorite_Landings.csv". As long as the file is in the same directory as the source code, it will be able to run.  

## Mount the Data Into the Container at Runtime
To mount the data into the container at runtime, before each command you run, you must volume mount the file with your data as shown below:  
```docker run --rm -v $PWD/Meteorite_Landings.csv:/data/Meteorite_Landings.csv <username>/hw3:1.0 <command>```

The examples to run the code and unit tests are found below.  

## Run the Containerized Code
This code will run the main executable, found on the PATH, using the meteorite landing dataset.  
```docker run --rm -v $PWD/Meteorite_Landings.csv:/data/Meteorite_Landings.csv <username>/hw3:1.0 main.py /data/Meteorite_Landings.csv```

### Changing the logging level
This program uses an optional flag to allow the user to specify the logging level as DEBUG, INFO, WARNING, ERROR, or CRITICAL. By default, the level is set to ERROR. I recommend you leave the option blank, as to avoid repetitive debug statements, since the data is very large.  
```docker run --rm -v $PWD/Meteorite_Landings.csv:/data/Meteorite_Landings.csv <username>/hw3:1.0 main.py --loglevel <LEVEL> /data/Meteorite_Landings.csv```

There is also an option to use a short option to specify the log level:  
```docker run --rm -v $PWD/Meteorite_Landings.csv:/data/Meteorite_Landings.csv <username>/hw3:1.0 main.py -l <LEVEL> /data/Meteorite_Landings.csv```

## Run the Containerized Unit Tests
This command will run pytest on the unit tests found in the /code directory.

```docker run --rm -v $PWD/Meteorite_Landings.csv:/data/Meteorite_Landings.csv <username>/hw3:1.0 pytest /code```

## Interpreting the results
The results are designed to be succint, despite requiring quite a bit of code to perform the computations, as well as extensive unit testing. The main program provides 3 sentences to the command line for the user to read. This program reads through 45716 rows of data, and I didn't want to overwhelm the user with a bunch of log statements. Thus, the user can simply run the program and enjoy the fun facts.

## References
Great-circle distance formula: https://en.wikipedia.org/wiki/Great-circle_distance  
Decimal degrees: https://en.wikipedia.org/wiki/Decimal_degrees  
