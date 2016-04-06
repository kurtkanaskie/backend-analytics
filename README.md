# Example apiproxy backend-analytics

Example Apigee API Node.js based proxy to pass values as custom headers (X-BackendSystem) to be use for custom analytics in Edge.

Goal is to capture which backend target, of many, in the Node.js app that are being used.

Approach: In Target PostFlow Response
* Pass values from Node.js back to Apigee Edge flow via header params
* Extract Variables to access the header param from Node.js
* Statistics Collector to collect the custom property
* Assign Message to remove the header param


###Download API proxy

#####Management API 
* curl -X GET -u username:password "https://api.enterprise.apigee.com/v1/o/ORG/apis"
* curl -X GET -u username:password "https://api.enterprise.apigee.com/v1/o/ORG/apis/backend-analytics"
* curl -X GET -u username:password "https://api.enterprise.apigee.com/v1/o/ORG/apis/backend-analytics/revisions"
* curl -X GET -u username:password "https://api.enterprise.apigee.com/v1/o/ORG/apis/backend-analytics/revisions/2"
* curl -X GET -u username:password "https://api.enterprise.apigee.com/v1/o/ORG/apis/backend-analytics/revisions/2?format=bundle" -o backend-analytics.zip

#####Apigeetool
* ```apigeetool fetchproxy -u $UN -p $PW -o $ORG -n backend-analytics -r $REV -f backend-analytics-r2.zip```

#####Maven
Download the API bundle, as per above.

* ```mvn install -P test -Dusername=your.name@email.com -Dpassword=secret-value``` (install and test)
* ```mvn deploy -P test -Dusername=your.name@email.com -Dpassword=secret-value``` (just deploy)
* ```mvn jmeter:jmeter -P test -DtestData=forecastapi_test.csv -DthreadNum=5 -DrampUpPeriodSecs=5 -DloopCount=2``` (test in test)
* ```mvn jmeter:jmeter -P prod -DtestData=forecastapi_prod.csv -DthreadNum=5 -DrampUpPeriodSecs=5 -DloopCount=2``` (test in prod)

###To test node locally
* ```cd apiproxy/resources/node```
* Update/install node modules ```npm install```
* Invoke with ```ENV=local node api.js```
