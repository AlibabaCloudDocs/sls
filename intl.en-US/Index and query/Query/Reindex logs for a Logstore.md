# Reindex logs for a Logstore

Log Service allows you to reindex logs for a Logstore based on the latest indexing rules in a specified time range. This topic describes how to reindex logs for a Logstore in the Log Service console.

## Scenarios

-   Query the collected logs for which you have not configured indexes.
-   Reconfigure indexes of historical logs if you want to query these logs after you modify the index configurations.

## Limits

-   You can only reindex logs that are generated in the last 15 minutes to 30 days.
-   You can create up to 10 reindexing tasks.
-   You can run only one reindexing task at a time.

## Billing

-   Reindexing incurs additional indexing fees and storage fees. Indexing fees are charged only once and storage fees are charged on an hourly basis. The unit price is the same as that of the existing indexes.
-   After you delete a reindexing task, the new index data is also deleted and no more storage fees are incurred for the deleted index data.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Find the project in the Projects section, and then click the project name.

3.  In the left-side navigation pane, click the Logstore for which you want to reindex logs.

4.  Choose **Index Attributes** \> **Reindex**.

5.  In the Reindex Logs dialog box, click **Create Task**.

6.  In the dialog box that appears, set the **Task Name**, **Start Time** and **End Time**, and then click **OK**.

    The **Start Time** is the time when Log Service starts to receive logs. The **End Time** is the time when Log Service stops to receive logs. The start time and end time must be within the last 15 minutes to 20 days.

    After you create the task, you can view the reindexing progress. When the progress reaches 100%, the reindexing task is completed.


## What to do next

After you create a reindexing task, you can perform the following operations:

-   Stop a reindexing task

    In the Reindex Logs dialog box, click **Stop** to stop the reindexing task.

    **Note:** After you stop the task, you can delete it, but you cannot restart it. Proceed with caution.

-   Delete a reindexing task

    In the Reindex Logs dialog box, click **Delete** to delete the reindexing task.

    **Note:** After you delete the reindexing task, the new index data is also deleted. Proceed with caution.


