# Prerequisites Setup
## Cassandra REST API with Google Cloud Functions in Node.js

In this example we will manually set up an Apache Cassandra instance in GCP

### Launch an instance in Google Cloud

1. To get things started, log into the Google Cloud Console, if you do not have an account, create one. https://console.cloud.google.com/
2. After logging in, create a new project for this example.
3. Once the project is created, go to the Compute Engine panel via the navigation menu in the top left and select "VM Instances" and click "Create" when available.
4. Follow the Create VM Instance wizard and change the image to be Ubuntu 18.04 LTS, also go to the Security portion and add your public key so that you can SSH to the instance from your computer.
5. Once the instance is created, go to the Networking Details to create firewall rules that will allow inbound and outbound TCP traffic on the C* CQL port 9042. **Note: This is simply for example purposes and this security setting should never be deployed for any real use cases**.

### Start the database
Now that we have an instance running in Google Cloud, we will install and start Cassandra on that instance.

1. To SSH to your instance in Google Cloud, you will need to use the private key that is the pair to the public key that was uploaded in Step 4 above as well as the Public IP.
```
ssh -i <path-to-private-key> <user-name>@<public-ip>
```
2. Install Java 8
```
sudo apt-get update 
sudo apt-get install -y openjdk-8-jdk-headless
```
3. Install Cassandra
Note if the mirror below is no longer working, visit the [downloads mirror](https://www.apache.org/dyn/closer.lua/cassandra/3.11.6/apache-cassandra-3.11.6-bin.tar.gz) for a recent link
```
mkdir cassandra; wget -c http://mirror.cogentco.com/pub/apache/cassandra/3.11.6/apache-cassandra-3.11.6-bin.tar.gz -O - | tar -xz -C cassandra --strip-components=1
```
4. Set `broadcast_rpc_address` to the Public IP ( needed for client connections )
```
sed -i 's/# broadcast_rpc_address:.*/broadcast_rpc_address: <public-ip>/' cassandra/conf/cassandra.yaml
```
5. Set `rpc_address` to listen on 0.0.0.0
```
sed -i 's/^rpc_address:.*/rpc_address: 0.0.0.0/' cassandra/conf/cassandra.yaml
```
6. Start the database
```
cassandra/bin/cassandra
```
7. Confirm the data center name is "datacenter1". You will need to set this as the `LOCAL_DC` value in serverless.yml
```
cassandra/bin/cqlsh
```
```
cqlsh> select data_center from system.local;

 data_center
-------------
 datacenter1

(1 rows)
```

### Set up local development environment
1. Install Node.js and NPM: https://docs.npmjs.com/downloading-and-installing-node-js-and-npm
2. Install Serverless
```
npm install -g serverless
```
3. Follow the instructions for setting up serverless with your GCP Credentials https://serverless.com/framework/docs/providers/google/guide/credentials/


Hope that wasn't too difficult and you are still here, if so now head on over to the main [README](README.md) to run this example.


