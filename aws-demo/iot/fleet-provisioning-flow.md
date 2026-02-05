# The Gateway: Backend Claim Generation
The app requests a certificate on-demand when the rider accepts a job order.
- The Request: The Android app sends its JWT to your /get-iot-claim endpoint.
- The Command: Your backend runs CreateProvisioningClaimCommand for the RiderAppTemplate.
- The Payload: AWS returns a temporary Claim Certificate and Private Key. Your backend converts the key to PKCS#8 (which the Android SDK prefers) and sends both to the app.
- The Expiration: This claim is a "ticking bomb"—it usually expires in 5 minutes. The app must use it to connect and provision immediately.

# The First Connection: "The Handshake"
The app uses those temporary credentials to establish its first MQTT connection to AWS IoT Core.
- Identity: At this moment, the rider is seen as a "provisioning claimant," not a specific "Thing."
- Scope: The app can only talk to the $aws/provisioning-templates/... topics. Any attempt to publish to tracker/Rider-123/update will be rejected by AWS at this stage.

# The Blueprint: The Provisioning Template
Now the app tells AWS: "I'm here, I have a valid claim, please make me permanent using 'RiderAppTemplate'."

The Provisioning Template on the AWS side is the "Manager" that executes these automated steps:
- Device Validation: (Optional) If you have a "Pre-Provisioning Hook" (a Lambda function), AWS will ask the Lambda: "Is this Rider ID allowed?"
- Thing Creation: It creates a new entry in the IoT Registry (e.g., ThingName: Rider-123).
- Credential Issuance: It generates the Permanent Certificate and Private Key unique to that specific rider.
- Policy Attachment: Crucially, it attaches your BasicIOTActionsPolicy to this new certificate. This policy explicitly grants the iot:Publish permission for the tracker/Rider-123/update topic.

# The Transition: "The Identity Swap"
This is the most critical part of the code you wrote earlier:
- The Receipt: AWS sends the permanent cert/key back to the app via an MQTT message (RegisterThingAccepted).
- The Shutdown: The app saves these credentials locally (ideally in the Keystore) and kills the current MQTT connection.
- The Re-Birth: The app starts a new MQTT connection using the permanent credentials.


| Step | Location | Role |
| --- | --- | --- |
| Login | Android App | Rider enters username/password. |
| Claim | Backend | Verifies JWT and fetches 5-min claim from AWS. |
| Bootstrap | Android App | Connects to MQTT using the 5-min claim. |
| Provision | AWS IoT Core | Provisioning Template creates the Thing and Policy.    |
| Permanent | Android App | Reconnects with the new cert to start tracking. |


RS (login) -> Backend (get token) -> RS (w/ token) -> Backend (get claim) / Lambda -> RS (w/ claim) -> IOT Core (exchange claim for permanent cert & private key) -> RS creates MQTT client

