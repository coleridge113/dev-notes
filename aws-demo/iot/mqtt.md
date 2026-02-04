```
val builder = AwsIotMqtt5ClientBuilder.newDirectMqttBuilderWithMtlsFromMemory(clientEndpoint, certificateData, keyData)
    .withCertificateAuthority(rootCA)
    .withClientId("Rider-1")
    .withSessionExpiryIntervalSeconds(3600L)
    .withSessionBehavior(Mqtt5ClientOptions.ClientSessionBehavior.REJOIN_ALWAYS)
    .withKeepAliveIntervalSeconds(60L)
    .withLifeCycleEvents(MqttLifeCycleEvents())
```

## Session Expirty
