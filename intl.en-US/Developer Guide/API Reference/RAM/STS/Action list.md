# Action list

This topic describes the actions that a Resource Access Management \(RAM\) user can perform on Log Service resources.

## Actions that a RAM user can perform on Log Service resources

The following table describes the actions that you can authorize a RAM user to perform on Log Service resources.

|Resource type|Action|Description|
|-------------|:-----|:----------|
|Project|[log:CreateProject](/intl.en-US/Developer Guide/API Reference/Project interfaces/CreateProject.md)|Creates a project.|
|[log:GetProject](/intl.en-US/Developer Guide/API Reference/Project interfaces/GetProject.md)|Queries the attributes of a project.|
|Logstore|[log:GetLogStoreLogs](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/GetLogs.md)|Queries logs in a Logstore of a specified project.|
|[log:GetLogStoreHistogram](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/GetHistograms.md)|Queries the distribution of logs in a Logstore of a project.|
|[log:GetLogStore](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/GetLogstore.md)|Queries the attributes of a Logstore.|
|[log:ListLogStores](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/ListLogstore.md)|Lists all Logstores in a project.|
|[log:CreateLogStore](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/CreateLogstore.md)|Creates a Logstore in a project.|
|[log:DeleteLogStore](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/DeleteLogstore.md)|Deletes a Logstore, including all shards and indexes of the Logstore.|
|[log:UpdateLogStore](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/UpdateLogstore.md)|Updates the attributes of a Logstore.|
|log:GetCursorOrData \([GetCursor](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/GetCursor.md), [PullLogs](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/PullLogs.md), and [GetCursorTime](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/GetCursorTime.md)\)|Queries a cursor based on the server time, queries logs in a specified shard based on a cursor, and queries the server time based on a cursor.|
|[log:ListShards](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/ListShards.md)|Lists all available shards in a Logstore.|
|[log:PostLogStoreLogs](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/PutLogs.md)|Writes logs to a Logstore.|
|[log:GetShipperStatus](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/GetShipperStatus.md)|Queries the status of LogShipper tasks.|
|[log:RetryShipperTask](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/RetryShipperTask.md)|Re-runs failed LogShipper tasks.|
|[log:CreateIndex](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/CreateIndex.md)|Creates indexes for a Logstore.|
|[log:DeleteIndex](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/DeleteIndex.md)|Deletes indexes from a Logstore.|
|[log:GetIndex](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/GetIndex.md)|Queries the indexes of a Logstore.|
|[log:UpdateIndex](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/UpdateIndex.md)|Updates the indexes of a Logstore.|
|Config|[log:CreateConfig](/intl.en-US/Developer Guide/API Reference/Logtail configuration related interfaces/CreateConfig.md)|Creates a Logtail configuration in a project.|
|[log:UpdateConfig](/intl.en-US/Developer Guide/API Reference/Logtail configuration related interfaces/UpdateConfig.md)|Updates a Logtail configuration.|
|[log:DeleteConfig](/intl.en-US/Developer Guide/API Reference/Logtail configuration related interfaces/DeleteConfig.md)|Deletes a Logtail configuration.|
|[log:GetConfig](/intl.en-US/Developer Guide/API Reference/Logtail configuration related interfaces/GetConfig.md)|Queries the details of a Logtail configuration.|
|[log:ListConfig](/intl.en-US/Developer Guide/API Reference/Logtail configuration related interfaces/ListConfig.md)|Lists all Logtail configurations in a project. You can configure related parameters to specify the number of entries to display on each page.|
|MachineGroup|[log:CreateMachineGroup](/intl.en-US/Developer Guide/API Reference/Logtail machine group related interfaces/CreateMachineGroup.md)|Creates a machine group to specify the servers from which logs are collected.|
|[log:UpdateMachineGroup](/intl.en-US/Developer Guide/API Reference/Logtail machine group related interfaces/UpdateMachineGroup.md)|Updates a machine group.|
|[log:DeleteMachineGroup](/intl.en-US/Developer Guide/API Reference/Logtail machine group related interfaces/DeleteMachineGroup.md)|Deletes a machine group.|
|[log:GetMachineGroup](/intl.en-US/Developer Guide/API Reference/Logtail machine group related interfaces/GetMachineGroup.md)|Queries the details of a machine group.|
|[log:ListMachineGroup](/intl.en-US/Developer Guide/API Reference/Logtail machine group related interfaces/ListMachineGroup.md)|Lists machine groups.|
|[log:ListMachines](/intl.en-US/Developer Guide/API Reference/Logtail machine group related interfaces/ListMachines.md)|Lists the machines in a machine group. The machines are connected to Log Service.|
|[log:ApplyConfigToMachineGroup](/intl.en-US/Developer Guide/API Reference/Logtail machine group related interfaces/ApplyConfigToMachineGroup.md)|Applies a Logtail configuration to a machine group.|
|[log:RemoveConfigFromMachineGroup](/intl.en-US/Developer Guide/API Reference/Logtail machine group related interfaces/RemoveConfigFromMachineGroup.md)|Deletes a Logtail configuration from a machine group.|
|[log:GetAppliedMachineGroups](/intl.en-US/Developer Guide/API Reference/Logtail configuration related interfaces/GetAppliedMachineGroups.md)|Lists the machines to which a Logtail configuration is applied.|
|[log:GetAppliedConfigs](/intl.en-US/Developer Guide/API Reference/Logtail machine group related interfaces/GetAppliedConfigs.md)|Lists the Logtail configurations that are applied to a machine group.|
|ConsumerGroup|[log:CreateConsumerGroup](/intl.en-US/Developer Guide/API Reference/Consumer group interfaces/CreateConsumerGroup.md)|Creates a consumer group in a Logstore.|
|[log:UpdateConsumerGroup](/intl.en-US/Developer Guide/API Reference/Consumer group interfaces/UpdateConsumerGroup.md)|Modifies the attributes of a consumer group.|
|[log:DeleteConsumerGroup](/intl.en-US/Developer Guide/API Reference/Consumer group interfaces/DeleteConsumerGroup.md)|Deletes a consumer group.|
|[log:ListConsumerGroup](/intl.en-US/Developer Guide/API Reference/Consumer group interfaces/ListConsumerGroup.md)|Lists all consumer groups in a specified Logstore.|
|[log:UpdateCheckPoint](/intl.en-US/Developer Guide/API Reference/Consumer group interfaces/UpdateCheckPoint.md)|Updates the checkpoint of a shard in a specified consumer group.|
|[log:HeartBeat](/intl.en-US/Developer Guide/API Reference/Consumer group interfaces/HeartBeat.md)|Sends a heartbeat packet to Log Service for a consumer.|
|[log:GetCheckPoint](/intl.en-US/Developer Guide/API Reference/Consumer group interfaces/GetCheckPoint.md)|Retrieves the checkpoints of one or all shards in a specified consumer group.|
|SavedSearch|log:CreateSavedSearch|Creates a saved search.|
|log:UpdateSavedSearch|Updates a saved search.|
|log:GetSavedSearch|Queries a saved search.|
|log:DeleteSavedSearch|Deletes a saved search.|
|log:ListSavedSearch|Queries the saved search list.|
|Dashboard|log:CreateDashboard|Creates a dashboard.|
|log:UpdateDashboard|Updates a dashboard.|
|log:GetDashboard|Queries a dashboard.|
|log:DeleteDashboard|Deletes a dashboard.|
|log:ListDashboard|Lists dashboards.|
|Job|log:CreateJob|Creates a task, for example, an alert or a subscription.|
|log:UpdateJob|Updates a task.|
|App|log:CreateApp|Grants a RAM user the permissions to create an application.|
|log:UpdateApp|Grants a RAM user the permissions to update an application.|
|log:GetApp|Grants a RAM user the permissions to query an application.|
|log:DeleteApp|Grants a RAM user the permissions to delete an application.|

