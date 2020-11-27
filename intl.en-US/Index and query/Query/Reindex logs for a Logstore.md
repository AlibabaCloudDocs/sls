# Reindex logs for a Logstore

Log Service allows you to reindex logs for a Logstore after you modify the current indexing rules or when you need to configure indexes for historical logs. You can reindex logs in a specified time range for a Logstore based on the latest indexing rules. This topic describes how to reindex logs for a Logstore in the Log Service console.

## Limits

-   You can reindex only the logs that are generated in the last 15 minutes to 30 days.
-   You can create a maximum of 10 reindexing tasks.
-   You can run only one reindexing task at a time.

## Billing

-   Reindexing incurs indexing fees and storage fees. Indexing fees are charged only once and storage fees are charged on an hourly basis. For more information, see [Billing items and methods](/intl.en-US/Pricing/Billing overview.md).
-   After you delete a reindexing task, the new index data is also deleted and no more storage fees are incurred for the deleted index data.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click a project.

3.  On the **Log Management** \> **Logstores** tab, choose **![Management icon](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9484688951/p52166.png)** \> **Search & Analysis** to the right of a Logstore.

4.  Click **Index Attributes** \> **Reindex**.

5.  In the Reindex Logstore dialog box, click **Create Task**.

6.  In the dialog box that appears, set the **Task Name**, **Start Time** and **End Time** parameters, and then click **OK**.

    The **Start Time** is the time when Log Service starts to receive logs. The **End Time** is the time when Log Service stops receiving logs. The start time and end time must be in the last 15 minutes to 30 days.

    After you create the task, you can view the reindexing progress. When the progress reaches 100%, the reindexing task is completed.


## What to do next

After you create a reindexing task, you can perform the following operations:

-   Stop the reindexing task

    In the Reindex Logstore dialog box, click **Stop** to stop the reindexing task.

    **Note:** After you stop the task, it can be deleted, but cannot be restarted. Proceed with caution.

-   Delete the reindexing task

    In the Reindex Logstore dialog box, click **Delete** to delete the reindexing task.

    **Note:** After you delete the reindexing task, the new index data is also deleted. Proceed with caution.


