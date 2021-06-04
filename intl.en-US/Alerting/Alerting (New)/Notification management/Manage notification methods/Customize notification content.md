# Customize notification content

Log Service allows you to customize notification content when you configure an alert template.

## Use template variables

When you configure an alert template, you can add template variables to the title and content of an email or a DingTalk message. When Log Service sends an alert notification, it replaces the template variables with actual values. For example, the $\{project\} variable is replaced with the name of an actual project. For more information, see [Template variables](/intl.en-US/Alerting/Alerting (New)/Notification management/Manage notification methods/Template variables.md).

Each time an alert is triggered, Log Service automatically generates the alert information and stores it in the Results field. All subfields in the Results field can be used as template variables. For more information, see [Fields in alert rule evaluation logs](/intl.en-US/Alerting/Alerting (Old)/Relevant syntax and fields for reference/Fields in alert rule evaluation logs.md).

-   Fields of the array type are referenced by using the $\{results\[\{index\}\]\} format. \{index\} indicates an array subscript, which starts from 0. For example, $\{results\[0\]\} indicates that the first element in the Results array is referenced.
-   Fields of the object type are referenced by using the $\{object.key\} format. For example, $\{results\[0\].StartTimeTs\} indicates a timestamp of 1542453580.

**Note:** The fields in RawResults and FireResult are query results. These fields are case-sensitive. Other fields are case-insensitive.

Example of the Results array:

```
{
  "EndTime": "2006-01-02 15:04:05",
  "EndTimeTs": 1542507580,
  "FireResult": {
    "__time__": "1542453580",
    "field": "value1",
    "count": "100"
  },
  "FireResultAsKv": "[field:value1,count:100]",
  "Truncated": false,
  "LogStore": "test-logstore",
  "Query": "* | SELECT field, count(1) group by field",
  "QueryUrl": "http://xxxx",
  "RawResultCount": 2,
  "RawResults": [
    {
      "__time__": "1542453580",
      "field": "value1",
      "count": "100"
    },
    {
      "__time__": "1542453580",
      "field": "value2",
      "count": "20"
    }
  ],
  "RawResultsAsKv": "[field:value1,count:100],[field:value2,count:20]",
  "StartTime": "2006-01-02 15:04:05",
  "StartTimeTs": 1542453580
}
```

## Content formats

-   DingTalk

    DingTalk messages support subsets of the Markdown syntax. The following elements are available:

    -   Title

        ```
        # Level 1 heading
        ## Level 2 heading
        ### Level 3 heading
        #### Level 4 heading
        ##### Level 5 heading
        ###### Level 6 heading
        ```

    -   Reference

        ```
        > A man who stands for nothing will fall for anything.
        ```

    -   Bold and italic text

        ```
        **bold**
        *italic*
        ```

    -   Link

        ```
        [this is a link](http://name.com)
        ```

    -   Image

        ```
        ![](http://name.com/pic.jpg)
        ```

    -   Unordered list

        ```
        - item1
        - item2
        ```

    -   Ordered list

        ```
        1. item1
        2. item2
        ```

-   Webhook

    Webhooks support sending one or more messages at a time.

    -   Send one message at a time:

        ```
        {
          "Project": "${project}",
          "Alert name": "${alert_name}"
        }
        ```

    -   Send multiple message at a time:

        ```
        [
          {
            "Project": "project-name1",
            "Alert name": "alert-name1"
          },
          {
            "Project": "project-name2",
            "Alert name": "alert-name2"
          }
        ]
        ```

-   Email

    Emails support HTML tags. Examples:

    -   Use `<br>` to break a line.
    -   Use `<a href="${query_url}">View details</a>` to add a link. You can click the link to view the alert details.
    -   Use `<strong>${severity}</strong>` to display an alert severity in bold text.

