# IOT Core (MQTT)
- AWS Pub/Sub
- Solves issues on network deadzones by queueing messages locally then re-attempts once connection is re-established

# Location Services
- Tracker Resource filters location updates it receives to save on costs, specifically costs on Geofence evaluations
- Location Services has geofence support so we get event updates when a device enters or exits a geofence

# EventBridge
- This is the only means for the Tracker Resource to "push" data to a destination

*SNS pros to follow*
