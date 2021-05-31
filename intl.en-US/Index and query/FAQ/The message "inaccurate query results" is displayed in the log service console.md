# What can cause an inaccurate query result to return?

When you query or analyze logs, a message indicating **The results are inaccurate.** may be displayed. This is displayed when only partial logs are scanned for query and analysis results, meaning the results do not include scans of full-log queries or analysis, and are therefore considered inaccurate.

Possible causes include:

## 1. The time range for queries is excessive.

**Cause**: The time range for queries is excessively wide, for example, three months or a year. In this case, Log Service cannot scan all logs generated within this time period.

**Solution**: Narrow down the time range for queries and perform multiple queries.

## 2. The query condition is exceedingly complex.

**Cause**: The query condition is exceedingly complex, or Log Service cannot read query results because the query condition contains multiple frequently used words.

**Solution**: Narrow down the query scope and perform multiple queries.

## 3. The SQL database reads an abnormally large amount of data.

**Cause**: The SQL database reads an abnormally large amount of data, which leads to inaccurate query results. For example, if the SQL database reads strings from multiple columns, it can read only 1 GB of data from each Shard. If this threshold is exceeded, inaccurate query results will be returned.

**Solution**: Narrow down the query scope and perform multiple queries.

