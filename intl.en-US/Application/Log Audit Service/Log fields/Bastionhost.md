# Bastionhost

This topic describes the fields of operation logs in Bastionhost \(BH\).

|Log field|Description|
|---------|-----------|
|\_\_topic\_\_|The topic of a log entry.|
|owner\_id|The ID of an Alibaba Cloud account.|
|content|The content of a log entry.|
|event\_type|The type of an event. For more information, see [Table 1](#table_xym_zqz_6e5).|
|instance\_id|The ID of a bastion host.|
|log\_level|The severity of a log entry.|
|resource\_address|The address of the server where a resource resides.|
|resource\_name|The name of the resource on which an operation is performed.|
|result|The result of an operation.|
|session\_id|The ID of a session.|
|user\_client\_ip|The source IP address.|
|user\_id|The ID of a user.|
|user\_name|The username.|

|Event type|Description|
|----------|-----------|
|cmd.Command|The CMD commands.|
|file.Upload|Uploads a file.|
|file.Download|Downloads a file.|
|file.Rename|Renames a file.|
|file.Delete|Deletes a file.|
|file.DeleteDir|Deletes a directory.|
|file.CreateDir|Creates a directory.|
|graph.Text|Text event.|
|graph.Keyboard|Keyboard event.|

