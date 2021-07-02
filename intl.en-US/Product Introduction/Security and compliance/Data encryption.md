# Data encryption

Log Service provides server-side encryption and encrypted transmission based on the SSL or TLS protocol to protect data from potential security risks on the cloud.

## Server-side encryption

Log Service allows you to use Key Management Service \(KMS\) to encrypt data for secure storage. KMS is a secure and easy-to-use management service that is provided by Alibaba Cloud. You can use KMS to ensure the privacy, integrity, and availability of your keys at low cost. You can use the keys in a secure and convenient manner. You can also develop encryption and decryption solutions based on your business requirements. You can view and manage the keys in the KMS console. For more information, see [Overview](/intl.en-US/Key Service/Overview.md).

KMS stores and manages customer master keys \(CMKs\) that are used to encrypt data keys. KMS also generates data keys that can be used to encrypt and decrypt large amounts of data. Envelope encryption provided by KMS can protect your data and data keys from unauthorized access. You can use the default CMK stored in KMS or generate a CMK by using your BYOK materials or the BYOK materials that are provided by Alibaba Cloud.

## Encrypted transmission based on the SSL or TLS protocol

Log Service can be accessed by using HTTP or HTTPS. SSL or TLS is a cryptographic protocol that provides secure communication and ensures data integrity between a web server and a client.

