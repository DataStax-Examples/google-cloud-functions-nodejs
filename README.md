<!--- STARTEXCLUDE --->
# Astra DB REST API with Google Cloud Functions in Node.js
*30 minutes, Intermediate, [Start Building](https://github.com/DataStax-Examples/google-cloud-functions-nodejs#prerequisites)*

This example shows how to use [Google Cloud Functions](https://cloud.google.com/functions/) with the [Node.js DataStax Cassandra Driver](https://docs.datastax.com/en/developer/nodejs-driver/latest) to set up a basic REST API for a Cassandra database via HTTP Endpoints. The [Serverless Framework](https://serverless.com/) is used to facilitate the setup and deployment of the functions.
<!--- ENDEXCLUDE --->


![image](https://raw.githubusercontent.com/DataStax-Examples/sample-app-template/master/screenshots/astra-sample-app-default.png)


## Objectives
- How to use the DataStax Cassandra Driver with Google Cloud Functions
- How to use the Serverless Framework to set up Google Cloud Functions HTTP Endpoints
  
## How it works
The Serverless Framework handles the packaging and deployment of the functions to the Google Cloud resources. Once the functions are deployed, the DataStax Cassandra Driver establishes the connection to the database and returns the results via the Google Cloud HTTP Endpoints which can be accessed to interact with the database.

## Get Started
To build and play with this app, follow the build instructions that are located here: [https://github.com/DataStax-Examples/google-cloud-functions-nodejs](https://github.com/DataStax-Examples/google-cloud-functions-nodejs#prerequisites)

<!--- STARTEXCLUDE --->
## Prerequisites
Let's do some initial setup by creating a serverless(!) database.

### DataStax Astra
<!--- enter a unique UTM_CODE for your sample app below --->
1. Create a [DataStax Astra account](https://astra.datastax.com/register?utm_source=github&utm_medium=referral&utm_campaign=gcp-functions-nodejs) if you don't already have one:
![image](https://raw.githubusercontent.com/DataStax-Examples/sample-app-template/master/screenshots/astra-register-basic-auth.png)

2. On the home page. Locate the button **`Create Database`**
![image](https://raw.githubusercontent.com/DataStax-Examples/sample-app-template/master/screenshots/astra-dashboard.png)

3. Locate the **`Get Started`** button to continue
![image](https://raw.githubusercontent.com/DataStax-Examples/sample-app-template/master/screenshots/astra-select-plan.png)

4. Define a **database name**, **keyspace name** and select a database **region**, then click **create database**.
![image](https://raw.githubusercontent.com/DataStax-Examples/sample-app-template/master/screenshots/astra-create-db.png)

5. Your Astra DB will be ready when the status will change from *`Pending`* to **`Active`** 💥💥💥 
![image](https://raw.githubusercontent.com/DataStax-Examples/sample-app-template/master/screenshots/astra-db-active.png)

6. After your database is provisioned, we need to generate an Application Token for our App. Go to the `Settings` tab in the database home screen.
![image](https://raw.githubusercontent.com/DataStax-Examples/sample-app-template/master/screenshots/astra-db-settings.png)

7. Select `Admin User` for the role for this Sample App and then generate the token. Download the CSV so that we can use the credentials we need later.
![image](https://raw.githubusercontent.com/DataStax-Examples/sample-app-template/master/screenshots/astra-db-settings-token.png)

8. After you have your Application Token, head to the database connect screen and select the driver connection that we need. Go ahead and download the `Secure Bundle` for the driver.
![image](https://raw.githubusercontent.com/DataStax-Examples/sample-app-template/master/screenshots/astra-db-connect-bundle.png)

9. Make note of where to use the `Client Id` and `Client Secret` that is part of the Application Token that we generated earlier.
![image](https://raw.githubusercontent.com/DataStax-Examples/sample-app-template/master/screenshots/astra-db-connect-bundle-driver.png)

### Github

1. Click `Use this template` at the top of the [GitHub Repository](https://github.com/DataStax-Examples/google-cloud-functions-nodejs):
![image](https://raw.githubusercontent.com/DataStax-Examples/sample-app-template/master/screenshots/github-use-template.png)

2. Enter a repository name and click 'Create repository from template':
![image](https://raw.githubusercontent.com/DataStax-Examples/sample-app-template/master/screenshots/github-create-repository.png)

3. Clone the repository:
![image](https://raw.githubusercontent.com/DataStax-Examples/sample-app-template/master/screenshots/github-clone.png)

## 🚀 Getting Started Paths:
*Make sure you've completed the [prerequisites](#prerequisites) before starting this step*
  - [Running on your local machine](#running-on-your-local-machine)

### Running on your local machine
Before running with this example, head over to the [SETUP-README](SETUP-README.md) for instructions on how to 
1. launch an instance in Google Cloud
2. install and start a Cassandra database
3. setup your local development environment for Node.js and [serverless](https://serverless.com)

Once the above is completed, you will have all of the needed pieces in place to run this example.

1. Install the DataStax Cassandra Driver
```sh
npm install cassandra-driver
```
4. Install serverless-google-cloudfunctions plugin
```sh
npm install serverless-google-cloudfunctions
```
5. Configure `serverless.yml` with your project-id, credentials file, Contact Points ( public IP of GCP instance ), and Local Data Center ( likely `datacenter1` )

From the project directory, deploy your function. This should output the endpoints that you can use to access the database.
```sh
sls deploy
```
* When you are done, don't forget to clean things up with
```
sls remove
```

#### createCatalog
```sh
curl -X POST https://us-central1-<project-id>.cloudfunctions.net/createCatalog
````
expected output:
```sh
"Successfully created shopping.catalog schema"
```
#### addItem
Note the `-H "Content-Type:application/json"` is required here.
```sh
curl -X POST -H "Content-Type:application/json" -d '{"item_id": 0, "name": "name_0", "description": "desc_0", "price": 10.1}' https://us-central1-<project-id>.cloudfunctions.net/addItem
```
expected output:
```sh
{"query":"INSERT INTO shopping.catalog (item_id, name, description, price) VALUES (?, ?, ?, ?)","item_id":0,"name":"name_0","description":"desc_0","price":10.1}
```
#### getItem
```sh
curl -X GET https://us-central1-<project-id>.cloudfunctions.net/getItem/0
```
expected output:
```sh
{"query":"SELECT name, description, price FROM shopping.catalog WHERE item_id = ?","item_id":["0"],"name":"name_0","description":"desc_0","price":"10.1"}
```

<!--- ENDEXCLUDE --->
