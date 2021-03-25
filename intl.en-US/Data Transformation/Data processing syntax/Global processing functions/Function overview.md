# Function overview

LOG domain specific language \(DSL\) of Log Service provides global processing functions that can be configured as steps in data processing rules.

The following table lists the global processing functions.

|Type|Function|Description|
|----|--------|-----------|
|[Flow control functions](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Flow control functions.md)|`e_if`|Performs an operation when a condition is met. Multiple condition-operation pairs can be specified.|
|`e_if_else`|Performs an operation when a condition is met and performs another operation if the condition is not met.|
|`e_switch`|Performs an operation when a condition is met and returns.|
|`e_compose`|Combines a series of operations.|
|[Event processing functions](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Event processing functions.md)|`e_drop`|Discards an event based on a condition.|
|`e_keep`|Retains an event based on a condition.|
|`e_split`|Splits an event to multiple events based on the value of a field.|
|`e_output`|Writes an event to a specified target and deletes the original event.|
|`e_coutput`|Writes an event to a specified target but retains the original event.|
|`e_to_metric`|Converts an event to time series data.|
|[Field processing functions](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Field processing functions.md)|`e_drop_fields`|Deletes the fields that meet a specified condition.|
|`e_keep_fields`|Retains the fields that meet a specified condition.|
|`e_rename`|Renames the fields that meet a specified condition.|
|[Value assignment function](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Value assignment function.md)|`e_set`|Assigns new values to the fields of an event.|
|[Value extraction functions](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Value extraction functions.md)|`e_regex`|Extracts the value of a field based on a regular expression.|
|`e_json`|Expands or extracts JSON objects.|
|`e_kv`|Extracts key-value pairs.|
|`e_kv_delimit`|Extracts key-value pairs based on a delimiter.|
|`e_csv`|Extracts fields based on a delimiter. The default delimiter is the comma \(,\).|
|`e_tsv`|Extracts fields based on a delimiter. The default delimiter is the tab \(\\t\).|
|`e_psv`|Extracts fields based on a delimiter. The default delimiter is the vertical bar \(`|`\).|
|`e_syslogrfc`|Extracts field values based on the Syslog protocol.|
|`e_anchor`|Extracts the values between specified start and end positions.|
|[Data mapping and enrichment functions](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Data mapping and enrichment functions.md)|`e_dict_map`|Enriches an event based on a dictionary.|
|`e_table_map`|Enriches an event based on a table.|
|`e_search_map`|Enriches an event based on a search.|
|[Value-added content function](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Value-added content function.md)|`e_threat_intelligence`|Obtains threat intelligence.|

