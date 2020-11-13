# Encrypt data

Log Service allows you to use Key Management Service \(KMS\) to encrypt data for secure storage. This topic describes the data encryption mechanism of Log Service and how to encrypt data by using KMS.

KMS is activated. For more information, see [Activate KMS](/intl.en-US/Quick Start/Activate KMS.md).

## Data encryption mechanism

Log Service encrypts data by using KMS. The data encryption mechanism has the following characteristics:

-   Log Service supports the Advanced Encryption Standard \(AES\) and SM4 encryption algorithms.
-   You can create and manage a customer master key \(CMK\) in the KMS console to ensure the security of the CMK.
-   Log Service supports the following encryption types:
    -   Service key-based encryption: Log service generates an independent service key for each Logstore. The service key never expires.
    -   Bring Your Own Key \(BYOK\) encryption: You can create a CMK in the KMS console and grant the relevant permissions to Log Service. When Log Service calls a KMS API operation, this CMK is used to create a key for data encryption. If the CMK is deleted or disabled, the BYOK key becomes invalid.

**Note:**

-   You can configure data encryption for a Logstore only when you call the [CreateLogstore](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/CreateLogstore.md) operation to create the Logstore. The algorithm or type of data encryption cannot be changed after the Logstore is created.
-   If the BYOK key becomes invalid, all read and write requests on the Logstore fail.

## Authorize Log Service to access KMS

If you use the BYOK key for data encryption, you must authorize Log Service to access KMS.

1.  Log on to the [RAM console](https://ram.console.aliyun.com).

2.  Create a Resource Access Management \(RAM\) role. For more information, see [Step 1: Create a RAM role and specify an Alibaba Cloud account for the RAM role](/intl.en-US/Developer Guide/Access control RAM/Assign a RAM role to an Alibaba Cloud account.md).

3.  Edit the trust policy of the RAM role so that Log Service can assume the role. For more information, see [Edit the trust policy of a RAM role](/intl.en-US/RAM Role Management/Edit the trust policy of a RAM role.md).

    ```
    {
        "Statement": [
            {
                "Action": "sts:AssumeRole",
                "Effect": "Allow",
                "Principal": {
                    "Service": [
                        "log.aliyuncs.com"
                    ]
                }
            }
        ],
        "Version": "1"
    }
    ```

4.  Grant the AliyunKMSReadOnlyAccess and AliyunKMSCryptoUserAccess permissions to the RAM role. For more information, see [Grant permissions to a RAM role](/intl.en-US/RAM Role Management/Grant permissions to a RAM role.md).

    ![Grant permissions](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2392525061/p176844.png)

5.  If you use a RAM user to perform BYOK encryption, you must grant the PassRole permission to the RAM user by creating a custom policy and attaching the policy to the RAM user. For more information, see [Create a custom policy](/intl.en-US/Policy Management/Custom policies/Create a custom policy.md) and [Grant permissions to a RAM user](/intl.en-US/RAM User Management/Grant permissions to a RAM user.md).

    ```
    {
        "Version": "1",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": "ram:PassRole",
                "Resource": "acs:ram::*"# The Alibaba Cloud Resource Name (ARN) of the RAM role. For more information about how to obtain the ARN of a RAM role, see [Ship log data from Log Service to OSS](/intl.en-US/Log consumption and shipping/Data shipping/Ship logs to OSS/Ship log data from Log Service to OSS.md).
            }
        ]
    }
    ```


## Configure data encryption of a Logstore

When you call the [CreateLogstore](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/CreateLogstore.md) operation to create a Logstore, add the encrypt\_conf node to encrypt data by using the following sample code:

```
encrypt_conf = {
      "enable" : True, # Specifies whether data encryption is enabled.
    "encrypt_type" : "default"# The encryption algorithm. The encrypt_type parameter can be set to only default or m4.
    "user_cmk_info" : # Optional parameter. If this parameter is specified, the BYOK key is used. Otherwise, the service key is used.
    {
           "cmk_key_id" : "" # The ID of the CMK to which the BYOK key belongs, for example, f5136b95-2420-ab31-xxxxxxxxx.
          "arn" : ""# The ARN of the RAM role. For more information about how to obtain the ARN of a RAM role, see [Ship log data from Log Service to OSS](/intl.en-US/Log consumption and shipping/Data shipping/Ship logs to OSS/Ship log data from Log Service to OSS.md).
          "region_id" : ""# The ID of the region where the CMK resides.
    }
}
```

