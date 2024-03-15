
## CASI PLUS DOCKER SETUP
Casi plus is a health application which tracks and traces the index clients and their sexual partners.

This is docker setup running many services including the following:

1. HapiFhir server 
2. Keycloak Authenticator
3. Fhir-web(web dashboard)
4. Fhir-gateway 
5. Proxy(Routing local Ports)

After cloning this repository follow the steps below to have CASI-PLUS server running.

### Generate Self assigned Certificates 

On the root of this project create and move to security directory

Run  `sudo mkdir /nginx/`

Run  `sudo mkdir /nginx/security`

Run  ` cd /nginx/security`

Now self-assigned generate Certificate and key

Run  `sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout apache-selfsigned.key -out apache-selfsigned.crt
`

From the certificate and key generate keystore 

Run  `sudo openssl pkcs12 -inkey apache-selfsigned.key -in apache-selfsigned.crt -export -out keystore
`

*** Set keystore password, Be sure to remember your keystore password, you will need it later ***


#### Generate Truststore 

Run  `openssl pkcs12 -export -nokeys -in apache-selfsigned.crt -out truststore
`

*** Set Truststore password, Sure to remember your truststore password, you will need it later **

### Create enviroment variables 
Navigate to the root directory of the project

Run  `cd ..`

Generate .env file 
Run `sudo cp -r .env.sample .env`

Make some adjustments in the .env variable values like DB_USERNAME,DB_PASSWORD and etc;

Generate config.js.tpl file

Run `sudo cp -r config.js.tpl.sample config.js.tpl`

### Changes in the docker-compose file 

Replace keyStorePassword value with the password that was used when creating keystore 

Then do the same thing with trustStorePassword value in docker-compose 

### Turn up the docker containers 
Run` sudo docker-compose up -d`
