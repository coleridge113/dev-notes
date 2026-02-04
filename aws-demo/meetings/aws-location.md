
# PREVIOUS MEETING QUESTIONS

**1. How long does AWS retain the tracker data for?**
    - Device location history is stored up to 30 days 
    - Non-adjustable

**2. Check if you can create Trackers programmatically**
    - Trackers can be created programatically 
    - 500 Tracker Resources per account (adj. to 1000)
    - 10 creation requests per second (same for delete requests)

> [!INFO]Create tracker endpoint
```
    POST /tracking/v0/trackers
    Content-type: application/json

    {
       "TrackerName": "Tracker-{JobOrderId}",
       "Description": "string",
       "KmsKeyEnableGeospatialQueries": false,
       "EventBridgeEnabled": false,
       "KmsKeyId": "1234abcd-12ab-34cd-56ef-1234567890ab",
       "PositionFiltering": "AccuracyBased",
       "Tags": { 
          "string" : "string" 
       }
    }
```


**3. Validate if Trackers should be created on a device/JO level**
    - Custom properties can be attached to every position update (up to 3 only)

> [!NOTE] Sample payload with custom property
```
    {
        "DeviceId": "Device-1",
        "SampleTime": "2026-01-12T06:26:46.859000+00:00",
        "ReceivedTime": "2026-01-12T06:26:47.791000+00:00",
        "Position": [
            121.0296089,
            14.5503713
        ],
        "PositionProperties": {
            "jobOrderId": "JobOrder-2",
            "key2": "value2",
            "key3": "value3"
        }
    }
```

> [!NOTE]Tracker Filters
>   Geometry-based filtering
>   Time-based filtering
>   Custom-property-based filtering


# GEOFENCES

**1. Geofence Collections and Geofences can be created programatically**

**2. Geofence Collections can be linked to Trackers**
    - Every valid update to the Tracker is automatically evaluated against the geofence LINKED to it
    - Trackers have options to validate an update (accuracy, location, time)


**3. Geofence Collections can be updated directly (and separately)**


        Store -----{ --------R------- }------|-- Customer

Tracker > Geofence Collection

**4. Costing is based per evaluation REQUEST**
    - A position update to a Tracker is ONE evaluation REQUEST to an entire Geofence collection
    - One REQUEST will evaluate up to ten device positions against all geofences in a single geofence collection.
    
**5. Storage charges apply to Geofence Collections that exceed one month storage**

**6. CRUD operations are also charged**

- total cost est.

# REFERENCES

**Trackers:**
- [30-day storage](https://docs.aws.amazon.com/location/latest/developerguide/start-create-tracker.html)
- [Programatic creation](https://docs.aws.amazon.com/location/latest/developerguide/tracking-components.html)
- [Quotas and usages](https://docs.aws.amazon.com/location/latest/developerguide/trackers-quotas.html)
- [Geometry filtering](https://docs.aws.amazon.com/location/latest/developerguide/list-device-positions.html)

**Geofences:**
- [Define one request](https://docs.aws.amazon.com/location/latest/developerguide/geofence-price.html)
- [Tracker name is optional](https://docs.aws.amazon.com/location/latest/APIReference/API_WaypointGeofencing_BatchEvaluateGeofences.html)
- [Custom properties](https://docs.aws.amazon.com/location/latest/APIReference/API_WaypointGeofencing_BatchEvaluateGeofences.html)
- [Quotas and usages](https://docs.aws.amazon.com/location/latest/developerguide/geofence-quotas.html)


Next week:
Live tracking for event bridge to send to API and Location Services at the same time
- MQTT
- AppSync

RS -> IOT -> (IOT Rule) -> Location Services
                        -> API





RS -> Tracker -> EventBridge -> Lambda -> API 
                                    ->    TD


























