<p align="center">
 <img src="https://github.com/FedeYK/ImpactProject/blob/main/app/olin_group.png" title="Dashboard Example" style="background-color: white" width="200" height="200">
</p>

# Impact Project - Olin Group Dashboard

This project is a data transformation and visualization application for address data, using Python, Streamlit, pandas and other tools. It provides functions for cleaning address data, validating and saving new addresses, and visualizing the locations of addresses on an interactive map.

:boom:<a href='https://streamlit-2c6cd3bsva-ew.a.run.app/'>Olin Group Address Cleaning app</a>

## Table of Contents

* [Key Features](#key)
* [Architecture](#arch)
* [How it Works?](#hiw)
  * [Clean Data](#cdat)
  * [New Address](#nad)
  * [Navigation](#nav)
* [Setup & Installation](#set)
  * [Installation](#ins)
* [Deployment on Cloud](#dep)
  * [Google Maps API Key](#key)
  * [Steps to Deploy](#step)


## <a name='key'></a>**Key Features**

- Cleans address data from CSV files, transforming it into a more usable format and providing download links for the cleaned data.
- Validates and saves new addresses entered through a form interface, and provides map visualization of the location.
- Filters and displays addresses from a data set on an interactive map, with additional data available in a detailed report.

## <a name='arch'></a>**Architecture**

<img src="https://github.com/FedeYK/ImpactProject/blob/main/Architecture-01.jpg" title="Dashboard Example" style="background-color: white">

## <a name='hiw'></a>**How it works?**

To begin with, the app is divided in three diferent sectors.
This are: 

* Clean Data
* New Address
* Navigation

<img src="https://github.com/FedeYK/ImpactProject/blob/main/pics/1.png" title="Dashboard Example">

### <a name='cdat'></a>**Clean Data**

In the Clean Data area, the user is able to upload a CSV file with messy addresses to be converted into well written addresses containing the geolocation data.

<img src="https://github.com/FedeYK/ImpactProject/blob/main/pics/2.png" title="Upload CSV">

Once the user has uploaded a CSV file, the cleaning algorithm begins. First of all, it separates the diferent characteristics of each address into different colums such as:

* Type of Street
* Street Name
* Street Number
* Region
* Etc...

Then it begins with the most important step of the Clean Data tab. It connects with two different Google Maps API's and request information from each one of those to get the perfect address.
First of all, the app calls the Google Places API, and after it has the results, it injects those results into the Google Geocoding API which returns a very specific address with the geocoding information.
The combination of this two API's have a great result in accuracy!

Once that process is finished, the app will show the algorithmicaly cleaned addresses with the final status obtained from the API's.

<img src="https://github.com/FedeYK/ImpactProject/blob/main/pics/3.png" title="Clean Data Example">

At this precise moment, the user can decide if it wants to save the addresses into a database or download the final CSV file.
The CSV file will be named with the actual date of the request.

### <a name='nad'></a>**New Address**

In the New Address area, the user can insert an address manually. The objective of this area is to improve Olin Group way of obtaining data.
The New Address area must verify with the API's the correct spelling and format of the address.

<img src="https://github.com/FedeYK/ImpactProject/blob/main/pics/4-0.png" title="New Address Form">

The New Address area must verify with the API's the correct spelling and format of the address.
Once the address inserted and verified, the full, well written address will be shown with the correct location plotted on a map.

<img src="https://github.com/FedeYK/ImpactProject/blob/main/pics/4.png" title="Plotted address">

### <a name='nav'></a>**Navigation**

The Navigation area is the next step into Olin Group data analysis.

<img src="https://github.com/FedeYK/ImpactProject/blob/main/pics/5.png" title="Plotted map">

This tab will allow Olin Group to see all their clients, filter them by province and zip code to make the best marketing analysis they could ever do.

<img src="https://github.com/FedeYK/ImpactProject/blob/main/pics/6.png" title="Filters" width="200" height="400">

With this tool, many things such as adding the zip code medium square meter value can be done and will allow Olin Group to excell in the future.

<img src="https://github.com/FedeYK/ImpactProject/blob/main/pics/7.png" title="Filters">

## <a name='set'></a>**Setup & Installation**

### <a name='ins'></a>**Installation**

1. Clone the repository:
```
$ git clone https://github.com/your-github-username/your-repo-name.git
```
2. Go into the repository
```
$ cd your-repo-name
```
3. Install the required packages
```
$ pip install -r requirements.txt
```
4. Run the Streamlit application:
```
$ streamlit run app.py
```
5. Open your browser and go to http://localhost:8501.


## <a name='dep'></a>**Deployment on Cloud**

To easily deploy this application, you can do it through Google Cloud Run.
Using Google Cloud Run, you can deploy in an easy serverless environment, it is flexible and portable, scalable, pay per use with 180.000 free seconds per month and 1 million calls.

### <a name='key'></a>**Create an Google Maps API Key**

This step is pretty stepforward, you just have to search for Google Maps Platform in your Google Cloud Console search bar and it will prompt you to generate a key.
In case it does not, just click on the left panel the "Keys" option, Create Key and last, API Key.

This API Key you will need it to deploy so your app is able to connect to the Google Maps API's.

### <a name='step'></a>**Steps to deploy**

1. Create a Dockerfile
```
# Use the official Python base image
FROM python:3.9-slim

# Set the working directory in the container
WORKDIR /app

# Copy the requirements.txt file to the container
COPY requirements.txt .

# Install the required packages
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application code to the container
COPY . .

# Expose the port on which the Streamlit app will run
EXPOSE 8080

# Set the command to run the Streamlit app when the container starts
CMD ["streamlit", "run", "--server.port", "8080", "main.py"]
```

2. You can use a script to deploy. For this, create a deploy.sh file and paste the following. 
```
#!/bin/bash

# Set variables for API
API_REGION="YOUR_REGION"
API_PROJECT_ID="YOUR_PROJECT_ID"
API_FOLDER_NAME="YOUR_FOLDER_NAME"
APP_NAME="YOUR_APP_NAME"
API_IMAGE_NAME="$API_REGION-docker.pkg.dev/$API_PROJECT_ID/$API_FOLDER_NAME/$APP_NAME"


# Deploy API
echo "Deploying API..."

# Change to the 'app' directory or your own directory
cd app

# Build API container image
docker build -t "$API_IMAGE_NAME" ./

# Enable Artifact Registry API
gcloud services enable artifactregistry.googleapis.com

# Create Repository for API
gcloud artifacts repositories create "$API_FOLDER_NAME" --location="$API_REGION" --repository-format=docker

# Authorize Docker client in the API region
gcloud auth configure-docker "$API_REGION-docker.pkg.dev"
echo "y"

# Push API container image to Artifact Registry
docker push "$API_IMAGE_NAME"

# Deploy API container to Cloud Run
gcloud run deploy streamlit --image "$API_IMAGE_NAME" --platform managed --region "$API_REGION" --allow-unauthenticated --set-env-vars="apikey=YOUR_API_KEY",DEBUG="False",ALLOWED_HOSTS="*"

echo "API deployment completed."

```

3. Upload everything to your Google Cloud console and then write the following

```
chmod +x deploy.sh
./deploy.sh
```

**Congratulations! Your app was deployed!**

You can also continue improving the CI/CD by connecting your GitHub to Cloud Build.


