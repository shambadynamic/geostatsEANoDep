This external adapter gets descriptive statistics from the Shamba geospatial API. It facilitates smart contracts to use satellite data.
Does NOT use any third-party Chainlink dependancy.

# Input Parameters

    "agg_x": The descriptive statistics required (aggregated) - string
    "dataset_code": The code of the geospatial dataset - string
    "selected_band": The specific band within the dataset - string
    "image_scale": The preferred spatial resolution - string
    "start_date": Start of the period of interest - string
    "end_date": End of the period of interest - string
    "geometry": Geojson defining the area of interest - object

# Example Request
   --data '{ "id": 0, "data": {"agg_x":"agg_mean", "dataset_code":"COPERNICUS/S2_SR", "selected_band":"NDVI", "image_scale":"250", "start_date":"2021-09-01", "end_date":"2021-09-10", "geometry":{"type":"FeatureCollection","features":[{"type":"Feature","properties":{},"geometry":{"type":"Polygon","coordinates":[[[19.51171875,4.214943141390651],[18.28125,-4.740675384778361],[26.894531249999996,-4.565473550710278],[27.24609375,1.2303741774326145],[19.51171875,4.214943141390651]]]}}]}} }'


# Response

For example when mean ("agg_mean") is requested, response is:


{"jobRunID":0,"data":{"agg_mean":0.624091649911697,"result":0.624091649911697},"result":0.624091649911697,"statusCode":200}


# Available descriptive statistics
 min, mean, median, max, stdDev, variance

# Where to get the GeoJson for the geometry parameter
 define it here:  https://contracts.shamba.app

# Learn more about the Shamba Geospatial Oracle and APIs

 https://docs.shamba.app


## Quick Start

See [Install Locally](#install-locally) for a quickstart

## Install Locally

Install dependencies:

```bash
yarn
```

### Test

Run the local tests:

```bash
yarn test
```

Natively run the application (defaults to port 8080):

### Run

```bash
yarn start
```

## Call the external adapter/API server

```bash
curl -X POST -H "content-type:application/json" "http://localhost:8080/" --data '{ "id": 0, ***enter request example above***'
```

## Docker

If you wish to use Docker to run the adapter, you can build the image by running the following command:

```bash
docker build . -t external-adapter
```

Then run it with:

```bash
docker run -p 8080:8080 -it external-adapter:latest
```

## Serverless hosts

After [installing locally](#install-locally):

### Create the zip

```bash
zip -r external-adapter.zip .
```

### Install to AWS Lambda

- In Lambda Functions, create function
- On the Create function page:
  - Give the function a name
  - Use Node.js 12.x for the runtime
  - Choose an existing role or create a new one
  - Click Create Function
- Under Function code, select "Upload a .zip file" from the Code entry type drop-down
- Click Upload and select the `external-adapter.zip` file
- Handler:
    - index.handler for REST API Gateways
    - index.handlerv2 for HTTP API Gateways
- Add the environment variable (repeat for all environment variables):
  - Key: API_KEY
  - Value: Your_API_key
- Save

#### To Set Up an API Gateway (HTTP API)

If using a HTTP API Gateway, Lambda's built-in Test will fail, but you will be able to externally call the function successfully.

- Click Add Trigger
- Select API Gateway in Trigger configuration
- Under API, click Create an API
- Choose HTTP API
- Select the security for the API
- Click Add

#### To Set Up an API Gateway (REST API)

If using a REST API Gateway, you will need to disable the Lambda proxy integration for Lambda-based adapter to function.

- Click Add Trigger
- Select API Gateway in Trigger configuration
- Under API, click Create an API
- Choose REST API
- Select the security for the API
- Click Add
- Click the API Gateway trigger
- Click the name of the trigger (this is a link, a new window opens)
- Click Integration Request
- Uncheck Use Lamba Proxy integration
- Click OK on the two dialogs
- Return to your function
- Remove the API Gateway and Save
- Click Add Trigger and use the same API Gateway
- Select the deployment stage and security
- Click Add

### Install to GCP

- In Functions, create a new function, choose to ZIP upload
- Click Browse and select the `external-adapter.zip` file
- Select a Storage Bucket to keep the zip in
- Function to execute: gcpservice
- Click More, Add variable (repeat for all environment variables)
  - NAME: API_KEY
  - VALUE: Your_API_key
