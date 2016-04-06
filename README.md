# Example apiproxy backend-analytics

Example Apigee API Node.js based proxy to pass values as custom headers (X-BackendSystem) to be use for custom analytics in Edge.

Goal is to capture which backend target, of many, in the Node.js app that are being used.

Approach: In Target PostFlow Response
* Pass values from Node.js back to Apigee Edge flow via header params
* Extract Variables to access the header param from Node.js
* Statistics Collector to collect the custom property
* Assign Message to remove the header param

##Usage
Clone this repository and upload apiproxy into your Edge environment.

###Apigeetool Commands
#####Download and install Apigeetool a Node.js module you can install with npm:
$ npm install –g apigeetool
```
MGMTSVR = https://api.enterprise.apigee.com (this is the default, override with –L $MGMTSVR)
ORG = your-org-name
UN = username@email.com:password
PW = username@email.com:password
```

#####Create (new or revision) do not Deploy
* apigeetool deployproxy -u $UN -p $PW -o $ORG -n apitest -e test -i
Create (new or revision) and Deploy (in directory were apiproxy is located)
* apigeetool deployproxy -u $UN -p $PW -o $ORG -n apitest -e test
#####Downlowd
* apigeetool fetchproxy -u $UN -p $PW -o $ORG -n apitest -r 2
#####List Deployments in environment
* apigeetool listdeployments -u $UN -p $PW -o $ORG -e test
#####List Deployments for API
* apigeetool listdeployments -u $UN -p $PW -o $ORG –n apitest

Apigeetool Docs: https://github.com/apigee/apigeetool-node

###Curl commands to Management API
```
MGMTSVR = https://api.enterprise.apigee.com
UNPW = username@email.com:password 
ORG = your-org-name
```

#####Create
* curl -X POST -u $UNPW "$MGMTSVR/v1/o/$ORG/apis" -d '{ "name": "apitest" }' -H 'Content-Type:application/json’

#####Download
* curl -X GET -u $UNPW ”$MGMTSVR/v1/o/$ORG/apis"
* curl -X GET -u $UNPW “$MGMTSVR/v1/o/$ORG/apis/apitest"
* curl -X GET -u $UNPW ”$MGMTSVR/v1/o/$ORG/apis/apitest/revisions"
* curl -X GET -u $UNPW ”$MGMTSVR/v1/o/$ORG/apis/apitest/revisions/1"
* curl -X GET -u $UNPW ”$MGMTSVR/v1/o/$ORG/apis/apitest/revisions/1?format=bundle" -o apitest.zip

#####Upload new revision
* curl -v -X POST -u $UNPW “$MGMTSVR/v1/o/$ORG/apis?name=apitest&validate=true&action=import -F file=@apitest.zip -H Content-Type:multipart/form-data

#####Update existing revision
* curl -v -X POST -u $UNPW $MGMTSVR/v1/o/$ORG/apis/apitest/revisions/2?validate=true -F file=@apitest.zip -H Content-Type:multipart/form-data

Management API Docs: http://docs.apigee.com/api/apis-0

###To test node locally
* ```cd apiproxy/resources/node```
* Update/install node modules ```npm install```
* Invoke with ```ENV=local node api.js```
