X509 Certificates are meant for long-lived, unfrequently updated hardware such as cameras, sensors, etc.
Having to update an embedded certificate requires the following:
- Revoke/delete the old certificate
- Create and download the new certificate
- Embed new certificate in RS app
- Release a new build

Cognito or Custom Auth (with JWT) is more appropriate for mobile due to the short-lived nature of tokens

