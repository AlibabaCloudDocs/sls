# OSS

This topic describes the fields of Object Storage Service \(OSS\) logs.

-   Access logs

    Access logs record access to OSS buckets. The logs are collected in real time.

    |Log field|Description|
    |---------|-----------|
    |\_\_topic\_\_|The topic of a log entry. Valid value: oss\_access\_log.|
    |owner\_id|The ID of an Alibaba Cloud account.|
    |region|The region where a bucket resides.|
    |access\_id|The AccessKey ID that is used to access OSS.|
    |time|The time when OSS receives a request. If a timestamp is required, use the value of the \_time\_ field.|
    |owner\_id|The ID of an Alibaba Cloud account that belongs to a bucket owner.|
    |User-Agent|The User-Agent HTTP header.|
    |logging\_flag|Indicates whether logging has been enabled to export logs to OSS buckets at regular intervals.|
    |bucket|The name of a bucket.|
    |content\_length\_in|The value of the Content-Length field in an HTTP request. Unit: bytes.|
    |content\_length\_out|The value of the Content-Length field in an HTTP response. Unit: bytes.|
    |object|The requested URL-encoded object. You can include the select url\_decode\(object\) clause in a query statement to decode the object.|
    |object\_size|The size of a requested object. Unit: bytes.|
    |operation|The API operation. For more information, see [Table 2](#table_a45_6g6_5p2).|
    |request\_uri|The URL-encoded URI of a request. This includes a query string. You can include the select url\_decode\(request\_uri\) clause in a query statement to decode the URI.|
    |error\_code|The error code that is returned by OSS. For more information, see [Error responses](/intl.en-US/Troubleshooting/Error responses.md).|
    |request\_length|The size of an HTTP request message that includes the header information. Unit: bytes.|
    |client\_ip|The IP address from which a request is sent. This can be the IP address of a client, firewall, or proxy.|
    |response\_body\_length|The size of an HTTP response body that excludes the header information.|
    |http\_method|The HTTP request method.|
    |referer|The Referer HTTP header.|
    |requester\_id|The ID of an Alibaba Cloud account that belongs to a requester. If you use anonymous logon, the value of this field is a hyphen \(-\).|
    |request\_id|The ID of a request.|
    |response\_time|The response time of a request. Unit: milliseconds.|
    |server\_cost\_time|The processing time of an OSS instance. Unit: milliseconds. The value of this field indicates the period of time that is required by the OSS instance to process a request.|
    |http\_type|The protocol of an HTTP request. Valid values: HTTP and HTTPS.|
    |sign\_type|The type of a signature. For more information, see [Table 4](#table_dps_lar_puz).|
    |http\_status|The status code of an HTTP connection that is returned in a request to OSS.|
    |sync\_request|The type of a synchronization request. For more information, see [Table 3](#table_mr3_mdo_h0j).|
    |bucket\_storage\_type|The OSS storage class. For more information, see [Table 1](#table_fh9_7lz_ylz).|
    |host|The domain name of an OSS server from which resources are requested.|
    |vpc\_addr|The VPC IP address of an OSS server. The IP address is based on the domain name of the server.|
    |vpc\_id|VPC ID|
    |delta\_data\_size|The size change of an object. If the object size does not change, the value of this field is 0. If a request is not an upload request, the value of this field is a hyphen \(-\).|
    |acc\_access\_region|If a request is a transfer acceleration request, this field indicates the ID of the region where the requested access point resides. Otherwise, the value of this field is a hyphen \(-\).|

-   Batch deletion logs

    Batch deletion logs record the information of deleted objects. The logs are collected in real time.

    **Note:** When you call the DeleteObjects operation, a request record is generated in an access log. The information of the objects that you request to delete is stored in the HTTP request body. The information of an object is not recorded in the object field in the related access log. The value of this field is a hyphen \(-\). If you want to view the list of deleted objects, you can view batch deletion logs that are associated by request\_id.

    |Log field|Description|
    |---------|-----------|
    |\_\_topic\_\_|The topic of a log entry. Valid value: oss\_batch\_delete\_log.|
    |owner\_id|The ID of an Alibaba Cloud account.|
    |region|The region where a bucket resides.|
    |client\_ip|The IP address from which a request is sent. This can be the IP address of a client, firewall, or proxy.|
    |user\_agent|The User-Agent HTTP header.|
    |bucket|The name of a bucket.|
    |error\_code|The error code that is returned by OSS. For more information, see [Table 3](#table_mr3_mdo_h0j).|
    |request\_length|The size of an HTTP request body that includes the header information. Unit: bytes.|
    |response\_body\_length|The size of an HTTP response body that excludes the header information.|
    |object|The requested URL-encoded object. You can include the select url\_decode\(object\) clause in a query statement to decode the object.|
    |object\_size|The size of a requested object. Unit: bytes.|
    |operation|The API operation. For more information, see [Table 2](#table_a45_6g6_5p2).|
    |bucket\_location|The cluster to which a bucket belongs.|
    |http\_method|The HTTP request method.|
    |referer|The Referer HTTP header.|
    |request\_id|The ID of a request.|
    |http\_status|The HTTP status code that is returned by an OSS request.|
    |sync\_request|The type of a synchronization request. For more information, see [Table 3](#table_mr3_mdo_h0j).|
    |request\_uri|The URL-encoded URI of a request. This includes a query string. You can include the select url\_decode\(request\_uri\) clause in a query statement to decode the URI.|
    |host|The domain name of an OSS server from which resources are requested.|
    |logging\_flag|Indicates whether logging has been enabled to export logs to OSS buckets at regular intervals.|
    |server\_cost\_time|The time that an OSS server requires to process a request. Unit: milliseconds.|
    |owner\_id|The ID of an Alibaba Cloud account that belongs to a bucket owner.|
    |requester\_id|The ID of an Alibaba Cloud account that belongs to a requester. If you use anonymous logon, the value of this field is a hyphen \(-\).|
    |delta\_data\_size|The size change of an object. If the object size does not change, the value of this field is 0. If a request is not an upload request, the value of this field is a hyphen \(-\).|

-   Hourly metering logs

    Hourly metering logs record the hourly metering statistics of a specific bucket. A latency of several hours exists in log collection.

    |Log field|Description|
    |---------|-----------|
    |\_\_topic\_\_|The topic of a log entry. Valid value: oss\_metering\_log.|
    |owner\_id|The ID of an Alibaba Cloud account that belongs to a bucket owner.|
    |bucket|The name of a bucket.|
    |cdn\_in|The inbound traffic from CDN. Unit: bytes.|
    |cdn\_out|The outbound traffic to CDN. Unit: bytes.|
    |get\_request|The number of GET requests.|
    |intranet\_in|The inbound traffic over the internal network. Unit: bytes.|
    |intranet\_out|The outbound traffic over the internal network. Unit: bytes.|
    |network\_in|The inbound traffic over the Internet. Unit: bytes.|
    |network\_out|The outbound traffic over the Internet. Unit: bytes.|
    |put\_request|The number of PUT requests.|
    |storage\_type|The OSS storage class. For more information, see [Table 1](#table_fh9_7lz_ylz).|
    |storage|The storage usage of a bucket. Unit: bytes.|
    |metering\_datasize|The size of metering data of non-Standard OSS buckets.|
    |process\_img\_size|The size of a processed image. Unit: bytes.|
    |process\_img|The processed image.|
    |sync\_in|The inbound synchronization traffic. Unit: bytes.|
    |sync\_out|The outbound synchronization traffic. Unit: bytes.|
    |start\_time|The time when a metering operation starts.|
    |end\_time|The time when a metering operation ends.|
    |region|The region where a bucket resides.|


|Storage class|Description|
|-------------|-----------|
|standard|Standard|
|archive|Archive|
|infrequent\_access|IA|

For information about related API operations, see [API overview](/intl.en-US/API Reference/API overview.md).

|Operation|Description|
|---------|-----------|
|AbortMultiPartUpload|Cancels a multipart upload task.|
|AppendObject|Appends an object to an existing object.|
|CompleteUploadPart|Completes the multipart upload task of an object.|
|CopyObject|Copies an object.|
|DeleteBucket|Deletes a bucket.|
|DeleteLiveChannel|Deletes a LiveChannel.|
|DeleteObject|Delete an object.|
|DeleteObjects|Deletes multiple objects.|
|GetBucket|Lists the information of all objects in a bucket.|
|GetBucketAcl|Queries the access control list \(ACL\) of a bucket.|
|GetBucketCors|Queries the cross-origin resource sharing \(CORS\) rules of a bucket.|
|GetBucketEventNotification|Queries the notification configurations of a bucket.|
|GetBucketInfo|Queries the information of a bucket.|
|GetBucketLifecycle|Queries the lifecycle rules configured for the objects in a bucket.|
|GetBucketLocation|Queries the region where a bucket resides.|
|GetBucketLog|Queries the access log configurations of a bucket.|
|GetBucketReferer|Queries the hotlink protection rules configured for a bucket.|
|GetBucketReplication|Queries the cross-region replication \(CRR\) rules configured for a bucket.|
|GetBucketReplicationProgress|Queries the progress of a CRR task that is performed on a bucket.|
|GetBucketStat|Queries the information of a bucket.|
|GetBucketWebSite|Queries the status of the static website hosting for a bucket.|
|GetLiveChannelStat|Queries the status of a LiveChannel.|
|GetObject|Reads an object.|
|GetObjectAcl|Queries the ACL of an object.|
|GetObjectInfo|Queries the information of an object.|
|GetObjectMeta|Queries the metadata of an object.|
|GetObjectSymlink|Queries the symbolic link of an object.|
|GetPartData|Queries the data in all parts of an object.|
|GetPartInfo|Queries the information of all parts of an object.|
|GetProcessConfiguration|Queries the image processing configurations of a bucket.|
|GetService|Lists all buckets.|
|HeadBucket|Queries the information of a bucket.|
|HeadObject|Queries the metadata of an object.|
|InitiateMultipartUpload|Initializes the multipart upload for an object.|
|ListMultiPartUploads|Lists multipart upload events.|
|ListParts|Queries the status of all parts of an object.|
|PostObject|Uploads an object by using a form.|
|PostProcessTask|Commits data processing operations, such as screenshots.|
|PostVodPlaylist|Creates a video-on-demand \(VOD\) playlist of a LiveChannel.|
|ProcessImage|Processes an image.|
|PutBucket|Creates a bucket.|
|PutBucketCors|Specifies the CORS rule for a bucket.|
|PutBucketLifecycle|Specifies the lifecycle of a bucket.|
|PutBucketLog|Specifies the access log for a bucket.|
|PutBucketWebSite|Specifies the static website hosting mode for a bucket.|
|PutLiveChannel|Creates a LiveChannel.|
|PutLiveChannelStatus|Specifies the status of a LiveChannel.|
|PutObject|Uploads an object.|
|PutObjectAcl|Modifies the ACL of an object.|
|PutObjectSymlink|Creates a symbolic link for an object.|
|RedirectBucket|Redirects the request to a bucket endpoint.|
|RestoreObject|Restores an object.|
|UploadPart|Resumes the upload of an object from a specified checkpoint.|
|UploadPartCopy|Copies a part of an object.|
|get\_image\_exif|Queries the exchangeable image file format \(Exif\) data of an image.|
|get\_image\_info|Queries the length and width of an image.|
|get\_image\_infoexif|Queries the length, width, and Exif data of an image.|
|get\_style|Queries the style of a bucket.|
|list\_style|Queries all styles of a bucket.|
|put\_style|Creates a picture processing rule for a bucket.|

|Synchronization request type|Description|
|----------------------------|-----------|
|Hyphen \(-\)|General requests|
|cdn|CDN back-to-origin requests|

For more information about signatures, see [Verify user signatures](/intl.en-US/API Reference/Access control/Verify user signatures.md).

|Signature type|Description|
|--------------|-----------|
|NotSign|A request is unsigned.|
|NormalSign|A request is signed with a regular signature.|
|UriSign|A request is signed with a URL signature.|
|AdminSign|A request is signed with an administrator account.|

