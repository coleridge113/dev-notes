
# AWS IOT
## Custom Authorizer

IOT Console -> Security -> Authorizer

The **Custom Authorizer** is built with a lambda that receives an event and should return the following response:
```
return {
        isAuthenticated: true,
        principalId: "user" + (decodedPayload.userId || "guest"),
        policyDocuments: [JSON.stringify({
            Version: "2012-10-17",
            Statement: [{ Action: "iot:*", Effect: "Allow", Resource: "*" }]
        })],
        disconnectAfterInSeconds: 86400,
        refreshAfterInSeconds: 300
    };
```


## Android / IOT Flow
1. RS fetches JWT Token from backend
2. RS sets up MQTT configuration with JWT token
3. RS makes an authorization request to IOT core with the token to build the MQTT client locally
4. IOT Core sees the request to authenticate using a Custom Authorizer
5. IOT Core finds the Custom Authorizer (assuming it's already configured in IOT Core)
6. The Custom Authorizer's linked Lambda is called to process the request
7. Lambda returns the appropriate response whether authenticated/authorized or not
8. If authenticated, RS successfully builds the MQTT client with the authorized actions provided in Lambda (connect, subscribe, and/or publish)
9. RS can call on mqttClient.publish() to send messages to IOT Core Topic
