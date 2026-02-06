# CS perspective
**Signing in**
```
CS -> MM API sign in -> JWT token (user session token)
```

**Assigned to a job order**
```
CS -> Load map -> /api/v1/iot/viewertoken (mqtt token)
                  Token content: { topics: "dev/orders/1/devices/1-123/location", ... }
               -> mqttmqtt.connect(token, ...)
```




# Rider perspective
**Signing in**
```
RS -> MM API sign in -> JWT token (user session token)
```

**Sending coordinates**
```
RS -> Send coordinates -> /api/v1/iot/publishtoken (mqtt token)
                          Token content: { topics: "dev/devices/1/location", ... }
                       -> mqttmqtt.connect(token, ...)
```

**Assigned to a job order**
```
RS -> Send coordinates -> /api/v1/iot/publishtoken (mqtt token)
                          { topics:
                            "dev/devices/1/location"
                            "dev/orders/1/devices/1-123/location"
                          }
                       -> mqttmqtt.connect(token, ...)
```



topic:
`dev/orders/1/devices/1-123/location` - With Job Order
`dev/devices/1/location` - Generic Location
