# Authorize a RAM user to use Cost Manager

This topic describes how to authorize a RAM user to use the Cost Manager application.

For more information about how to authorize a RAM user, see [Grant permissions to a RAM user](/intl.en-US/Developer Guide/Access control RAM/Create a RAM user and authorize the RAM user to access Log Service.md).

To authorize a RAM user to use the Cost Manager application, use the following policy. For more information about relevant actions, see [Action list](/intl.en-US/Developer Guide/API Reference/RAM/STS/Action list.md).

```
{
  "Version": "1",
  "Statement": [
    {
      "Action": "log:CreateLogStore",
      "Resource": "acs:log:*:*:project/bill-analysis-*/logstore/*",
      "Effect": "Allow"
    },
    {
      "Action": "log:CreateIndex",
      "Resource": "acs:log:*:*:project/bill-analysis-*/logstore/aliyun_bill",
      "Effect": "Allow"
    },
    {
      "Action": "log:UpdateIndex",
      "Resource": "acs:log:*:*:project/bill-analysis-*/logstore/aliyun_bill",
      "Effect": "Allow"
    },
    {
      "Action": "log:CreateDashboard",
      "Resource": "acs:log:*:*:project/bill-analysis-*/dashboard/*",
      "Effect": "Allow"
    },
  {
      "Action": "log:UpdateDashboard",
      "Resource": "acs:log:*:*:project/bill-analysis-*/dashboard/*",
      "Effect": "Allow"
    },
  {
      "Action": "log:CreateSavedSearch",
      "Resource": "acs:log:*:*:project/bill-analysis-*/savedsearch/*",
      "Effect": "Allow"
    },
  {
      "Action": "log:UpdateSavedSearch",
      "Resource": "acs:log:*:*:project/bill-analysis-*/savedsearch/*",
      "Effect": "Allow"
    },
{
      "Action": "log:CreateJob",
      "Resource": "acs:log:*:*:project/bill-analysis-*/job/*",
      "Effect": "Allow"
    },
  {
      "Action": "log:UpdateJob",
      "Resource": "acs:log:*:*:project/bill-analysis-*/job/*",
      "Effect": "Allow"
    },
 {
      "Action": "log:CreateApp",
      "Resource": "acs:log:*:*:app/bill",
      "Effect": "Allow"
    },
{
      "Action": "log:UpdateApp",
      "Resource": "acs:log:*:*:app/bill",
      "Effect": "Allow"
    },
{
      "Action": "log:GetApp",
      "Resource": "acs:log:*:*:app/bill",
      "Effect": "Allow"
    },
{
      "Action": "log:DeleteApp",
      "Resource": "acs:log:*:*:app/bill",
      "Effect": "Allow"
    }
  ]
}
```

