
## CASI PLUS DOCKER SETUP
Casi plus is a health application which tracks and traces the index clients and their sexual partners.

This is docker setup running many services including the following:

1. HapiFhir server
2. Keycloak Authenticator
3. Fhir-web(web dashboard)
4. Fhir-gateway
5. Proxy(Routing local Ports)

Setup on Ubuntu 22 LTS

After cloning this repository follow the steps below to have CASI-PLUS server running.

### Clone the CASI code and install docker

Run `sudo apt-get update`

Run `sudo apt-get install docker-compose`

Run `sudo git clone https://github.com/I-TECH-UW/CASI.git`


### Generate Self assigned Certificates

On the root of this project create and move to security directory

Run  `cd nginx`

Run  `sudo mkdir security`

Run  `cd security`


Now self-assigned generate Certificate and key

Run  `sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout casi_key.key -out casi_certificate.crt
`

From the certificate and key generate keystore

Run  `sudo openssl pkcs12 -inkey casi_key.key -in casi_certificate.crt -export -out keystore
`

*** Set keystore password, Be sure to remember your keystore password, you will need it later ***


#### Generate Truststore


Run  `sudo openssl pkcs12 -export -nokeys -in casi_certificate.crt -out truststore

`

*** Set Truststore password, Sure to remember your truststore password, you will need it later **

### Create enviroment variables
Navigate to the root directory of the project

Run  `cd ../../`

Generate .env file
Run `cp -r .env.sample .env`

Make some adjustments in the .env file variable values to your preferred values
`
DB_VENDOR=postgres
DB_ADDR=postgres_keycloak
DB_DATABASE=keycloak
DB_USERNAME=
DB_PASSWORD=
DB_PORT=5836
KEYCLOAK_DEFAULT_USERNAME=
KEYCLOAK_DEFAULT_PASSWORD=
KEYCLOAK_PORT=8180
HOSTNAME=localhost
keystorePassword=
`
And also replace all localhost addresses `http://localhost` with your server https domain name eg) FROM `http://localhost/keycloak/realms/fhir/protocol/openid-connect/token` to `https://example.com/keycloak/realms/fhir/protocol/openid-connect/token`

Generate config.js.tpl file

Run `cp -r config.js.tpl.sample config.js.tpl`

Update all host addresses from `http://localhost` with your server https domain name eg) FROM `http://localhost/keycloak/admin/realms/fhir` to `http://example.com/keycloak/admin/realms/fhir` within config.js.tpl file

### Changes in the docker-compose file

Run `sudo nano docker-compose.yml`


Replace keyStorePassword value with the password that was used when creating keystore

Run `sudo nano docker-compose.yml`

Then do the same thing with trustStorePassword value in docker-compose

Save the docker compose configuration

### Turn on the docker containers

Run` sudo docker-compose up -d`
