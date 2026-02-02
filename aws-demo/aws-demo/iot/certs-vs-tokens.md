# X509 Certificates vs Custom Authorization using JWT
## X509 is meant for a different use case
Device registration with X509 is meant for long-lived, unfrequently updated devices such as cameras, sensors, etc. 
The certificate is also long-lived and lives within the device which can pose a security issue, especially if the device is jailbroken.
Rotating the certificate is very tedious which involves the following:
- Delete old certificates
- Download new certificates
- Update the app with new certificates

## Custom Authorization with JWT
Custom authorizer + JWT fits more for a mobile app use case when registering a device.
The lambda (linked to the Custom Authorizer) can be very strict and only allow publishing to the topic defined for the specific rider.
JWT tokens are short-lived so they 
