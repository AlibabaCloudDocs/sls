# Grok patterns

Grok is a tool that combines multiple predefined regular expressions to match and split text and map the text segments to keys. Grok can be used to process log data. This topic describes the Grok patterns and provides several examples of basic syntax.

The following table lists the Grok patterns.

**Note:** Named parameters cannot be used in some Grok patterns and their combinations, such as SYSLOGBASE, COMMONAPACHELOG, COMBINEDAPACHELOG, HTTPD20\_ERRORLOG, HTTPD24\_ERRORLOG, and HTTPD\_ERRORLOG.

|Type|Pattern|Description|
|----|-------|-----------|
|Common patterns|EXTRACTJSON|Matches JSON data.|
|CHINAID|Matches the numbers of Chinese resident identity cards.|
|USERNAME|Matches content that contains letters, digits, and `. _-`.|
|USER|Matches content that contains letters, digits, and `. _-`.|
|EMAILLOCALPART|Matches the characters before the at sign \(@\) in an email address. For example, in the email address 123456@alibaba.com, the matched content is 123456.|
|EMAILADDRESS|Matches email addresses.|
|HTTPDUSER|Matches email addresses or usernames.|
|INT|Matches integers.|
|BASE10NUM|Matches decimal numbers.|
|NUMBER|Matches numbers.|
|BASE16NUM|Matches hexadecimal numbers.|
|BASE16FLOAT|Matches hexadecimal floating-point numbers.|
|POSINT|Matches positive integers.|
|NONNEGINT|Matches non-negative integers.|
|WORD|Matches words.|
|NOTSPACE|Matches characters that are not spaces.|
|SPACE|Matches spaces.|
|DATA|Matches line breaks.|
|GREEDYDATA|Matches zero or multiple characters that are not line breaks.|
|QUOTEDSTRING|Matches quoted content. For example, in the `I am "Iron Man"` string, the matched content is `Iron Man`.|
|UUID|Matches universally unique identifiers \(UUIDs\).|
|Networking|MAC|Matches MAC addresses.|
|CISCOMAC|Matches Cisco MAC addresses.|
|WINDOWSMAC|Matches Windows MAC addresses.|
|COMMONMAC|Matches common MAC addresses.|
|IPV6|Matches IPv6 addresses.|
|IPV4|Matches IPv4 addresses.|
|IP|Matches IPv6 or IPv4 addresses.|
|HOSTNAME|Matches hostnames.|
|IPORHOST|Matches IP addresses or hostnames.|
|HOSTPORT|Matches IP addresses, hostnames, or positive integers.|
|Paths|PATH|Matches UNIX paths or Windows paths.|
|UNIXPATH|Matches UNIX paths.|
|WINPATH|Matches Windows paths.|
|URIPROTO|Matches URI schemes. For example, in `http://hostname.domain.tld/_astats?application=&inf.name=eth0`, the matched content is `http`.|
|TTY|Matches tty paths.|
|URIHOST|Matches IP addresses, hostnames, or positive integers. For example, in `http://hostname.domain.tld/_astats?application=&inf.name=eth0`, the matched content is `hostname.domain.tld`.|
|URI|Matches URIs.|
|Date|MONTH|Matches months that are in the numeric, abbreviated, or full-name format.|
|MONTHNUM|Matches months that are in the numeric format.|
|MONTHDAY|Matches days in a month.|
|DAY|Matches weekdays that are in the abbreviated or full-name format.|
|YEAR|Matches years.|
|Time|HOUR|Matches hours.|
|MINUTE|Matches minutes.|
|SECOND|Matches seconds.|
|TIME|Matches time.|
|DATE\_US|Matches dates that are in the format of Month-Day-Year or dates where the month, day, and year are separated by forward slashes \(`/`\).|
|DATE\_EU|Matches dates that are in the format of Day-Month-Year or dates where the day, month, and year are separated by forward slashes \(`/`\) or periods \(`.`\).|
|ISO8601\_TIMEZONE|Matches hours and minutes that are in the ISO 8601 format.|
|ISO8601\_SECOND|Matches seconds that are in the ISO 8601 format.|
|TIMESTAMP\_ISO8601|Matches time that is in the ISO 8601 format.|
|DATE|Matches dates that are in the US or EU format.|
|DATESTAMP|Matches dates and time.|
|TZ|Matches UTC time zones.|
|DATESTAMP\_RFC822|Matches time that is in the RFC 822 format.|
|DATESTAMP\_RFC2822|Matches time that is in the RFC 2822 format.|
|DATESTAMP\_OTHER|Matches time that is in other formats.|
|DATESTAMP\_EVENTLOG|Matches time that is in the EventLog format.|
|HTTPDERROR\_DATE|Matches time that is in the httpd error format.|
|SYSLOG|SYSLOGTIMESTAMP|Matches time that is in the Syslog format.|
|PROG|Matches programs.|
|SYSLOGPROG|Matches programs and process identifiers \(PIDs\).|
|SYSLOGHOST|Matches IP addresses or hostnames.|
|SYSLOGFACILITY|Matches facilities.|
|HTTPDATE|Matches dates and time that are in the HTTP format.|
|LOGFORMATL|LOGFORMAT|Matches Syslog logs that are in the traditional format.|
|COMMONAPACHELOG|Matches common Apache logs.|
|COMBINEDAPACHELOG|Matches combined Apache logs.|
|HTTPD20\_ERRORLOG|Matches httpd20 logs.|
|HTTPD24\_ERRORLOG|Matches httpd24 logs.|
|HTTPD\_ERRORLOG|Matches httpd logs.|
|LOGLEVELS|LOGLEVELS|Matches log levels, such as warn and debug.|

## General Grok patterns

```
EXTRACTJSON (? <json>(?:\{\s*"(?:\\"|[^"])+"\s*:\s*(?:(? P>json)|"(?:\\"|[^"])+"|[-+]?( 0|[1-9]\d*)(?:\.[-+]?( 0|[1-9]\d*))?(?:[eE][-+]?( 0|[1-9]\d*))? |(?:true|false)|null)(?:\s*,\s*"(?:\\"|[^"])+"\s*:\s*(?:(? P>json)|"(?:\\"|[^"])+"|[-+]?( 0|[1-9]\d*)(?:\.[-+]?( 0|[1-9]\d*))?(?:[eE][-+]?( 0|[1-9]\d*))? |(?:true|false)|null))*\s*\}|\[\s*(?:(? P>json)|"(?:\\"|[^"])+"|[-+]?( 0|[1-9]\d*)(?:\.[-+]?( 0|[1-9]\d*))?(?:[eE][-+]?( 0|[1-9]\d*))? |(?:true|false)|null)(?:\s*,\s*(?:(? P>json)|"(?:\\"|[^"])+"|[-+]?( 0|[1-9]\d*)(?:\.[-+]?( 0|[1-9]\d*))?(?:[eE][-+]?( 0|[1-9]\d*))? |(?:true|false)|null))*\s*\]))
CHINAID [1-9]\d{5})((18|19|([23]\d))\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{3}[0-9Xx]$)|(^[1-9]\d{5}\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{3}
USERNAME [a-zA-Z0-9. _-]+
USER %{USERNAME}
EMAILLOCALPART [a-zA-Z][a-zA-Z0-9_. +-=:]+
EMAILADDRESS %{EMAILLOCALPART}@%{HOSTNAME}
HTTPDUSER %{EMAILADDRESS}|%{USER}
INT (?:[+-]?(?:[0-9]+))
BASE10NUM (? <![ 0-9. +-])(? >[+-]?(?:(?:[0-9]+(?:\.[ 0-9]+)?) |(?:\.[ 0-9]+)))
NUMBER (?:%{BASE10NUM})
BASE16NUM (? <![ 0-9A-Fa-f])(?:[+-]?(?:0x)?(?:[0-9A-Fa-f]+))
BASE16FLOAT \b(? <![ 0-9A-Fa-f.])(?:[+-]?(?:0x)?(?:(?:[0-9A-Fa-f]+(?:\.[ 0-9A-Fa-f]*)?) |(?:\.[ 0-9A-Fa-f]+)))\b

POSINT \b(?:[1-9][0-9]*)\b
NONNEGINT \b(?:[0-9]+)\b
WORD \b\w+\b
NOTSPACE \S+
SPACE \s*
DATA . *?
GREEDYDATA . *
QUOTEDSTRING (? >(? <! \\)(? >"(? >\\. |[^\\"]+)+"|""|(? >'(? >\\. |[^\\']+)+')|''|(? >`(? >\\. |[^\\`]+)+`)|``))
UUID [A-Fa-f0-9]{8}-(?:[A-Fa-f0-9]{4}-){3}[A-Fa-f0-9]{12}

"""Networking"""
MAC (?:%{CISCOMAC}|%{WINDOWSMAC}|%{COMMONMAC})
CISCOMAC (?:(?:[A-Fa-f0-9]{4}\.){2} [A-Fa-f0-9]{4})
WINDOWSMAC (?:(?:[A-Fa-f0-9]{2}-){5}[A-Fa-f0-9]{2})
COMMONMAC (?:(?:[A-Fa-f0-9]{2}:){5}[A-Fa-f0-9]{2})
IPV6 ((([0-9A-Fa-f]{1,4}:){7}([0-9A-Fa-f]{1,4}|:))|(([0-9A-Fa-f]{1,4}:){6}(:[0-9A-Fa-f]{1,4}|((25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)){3})|:))|(([0-9A-Fa-f]{1,4}:){5}(((:[0-9A-Fa-f]{1,4}){1,2})|:((25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)){3})|:))|(([0-9A-Fa-f]{1,4}:){4}(((:[0-9A-Fa-f]{1,4}){1,3})|((:[0-9A-Fa-f]{1,4})?:((25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)){3}))|:))|(([0-9A-Fa-f]{1,4}:){3}(((:[0-9A-Fa-f]{1,4}){1,4})|((:[0-9A-Fa-f]{1,4}){0,2}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)){3}))|:))|(([0-9A-Fa-f]{1,4}:){2}(((:[0-9A-Fa-f]{1,4}){1,5})|((:[0-9A-Fa-f]{1,4}){0,3}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)){3}))|:))|(([0-9A-Fa-f]{1,4}:){1}(((:[0-9A-Fa-f]{1,4}){1,6})|((:[0-9A-Fa-f]{1,4}){0,4}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)){3}))|:))|(:(((:[0-9A-Fa-f]{1,4}){1,7})|((:[0-9A-Fa-f]{1,4}){0,5}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)){3}))|:)))(%. +)?
IPV4 (? <![ 0-9])(?:(?:[0-1]?[ 0-9]{1,2}|2[0-4][0-9]|25[0-5])[.](?:[0-1]?[ 0-9]{1,2}|2[0-4][0-9]|25[0-5])[.](?:[0-1]?[ 0-9]{1,2}|2[0-4][0-9]|25[0-5])[.](?:[0-1]?[ 0-9]{1,2}|2[0-4][0-9]|25[0-5]))(?![ 0-9])
IP (?:%{IPV6}|%{IPV4})
HOSTNAME \b(?:[0-9A-Za-z][0-9A-Za-z-]{0,62})(?:\.(?:[0-9A-Za-z][0-9A-Za-z-]{0,62}))*(\.? |\b)
IPORHOST (?:%{IP}|%{HOSTNAME})
HOSTPORT %{IPORHOST}:%{POSINT}

"""paths"""
PATH (?:%{UNIXPATH}|%{WINPATH})
UNIXPATH (/([\w_%! $@:.,~-]+|\\.) *)+
TTY (?:/dev/(pts|tty([pq])?)( \w+)? /?(?:[0-9]+))
WINPATH (? >[A-Za-z]+:|\\)(?:\\[^\\? *]*)+
URIPROTO [A-Za-z]+(\+[A-Za-z+]+)?
URIHOST %{IPORHOST}(?::%{POSINT:port})?
"""uripath comes loosely from RFC1738, but mostly from what Firefox"""
"""doesn't turn into %XX"""
URIPATH (?:/[A-Za-z0-9$. +! *'(){},~:;=@#%_\-]*)+
"""URIPARAM \?(?:[A-Za-z0-9]+(?:=(?:[^&]*))?(?:&(?:[A-Za-z0-9]+(?:=(?:[^&]*))?)?) *)?"""
URIPARAM \?[ A-Za-z0-9$. +! *'|(){},~@#%&/=:;_? \-\[\]<>]*
URIPATHPARAM %{URIPATH}(?:%{URIPARAM})?
URI %{URIPROTO}://(?:%{USER}(?::[^@]*)? @)?(?:%{URIHOST})?(?:%{URIPATHPARAM})?

""" Months: January, Feb, 3, 03, 12, December"""
MONTH \b(?:Jan(?:uary|uar)? |Feb(?:ruary|ruar)? |M(?:a|ä)? r(?:ch|z)? |Apr(?:il)? |Ma(?:y|i)? |Jun(?:e|i)? |Jul(?:y)? |Aug(?:ust)? |Sep(?:tember)? |O(?:c|k)? t(?:ober)? |Nov(?:ember)? |De(?:c|z)(?:ember)?) \b
MONTHNUM (?:0?[ 1-9]|1[0-2])
MONTHNUM2 (?:0[1-9]|1[0-2])
MONTHDAY (?:(?:0[1-9])|(?:[12][0-9])|(?:3[01])|[1-9])

"""Days: Monday, Tue, Thu, etc..."""
DAY (?:Mon(?:day)? |Tue(?:sday)? |Wed(?:nesday)? |Thu(?:rsday)? |Fri(?:day)? |Sat(?:urday)? |Sun(?:day)?)

""" Years?"""
YEAR (? >\d\d){1,2}
HOUR (?:2[0123]|[01]?[ 0-9])
MINUTE (?:[0-5][0-9])
"""'60' is a leap second in most time standards and thus is valid."""
SECOND (?:(?:[0-5]?[ 0-9]|60)(?:[:.,][0-9]+)?)
TIME (?! <[0-9])%{HOUR}:%{MINUTE}(?::%{SECOND})(?![ 0-9])
"""datestamp is YYYY/MM/DD-HH:MM:SS.UUUU (or something like it)"""
DATE_US %{MONTHNUM}[/-]%{MONTHDAY}[/-]%{YEAR}
DATE_EU %{MONTHDAY}[. /-]%{MONTHNUM}[. /-]%{YEAR}
ISO8601_TIMEZONE (?:Z|[+-]%{HOUR}(?::? %{MINUTE}))
ISO8601_SECOND (?:%{SECOND}|60)
TIMESTAMP_ISO8601 %{YEAR}-%{MONTHNUM}-%{MONTHDAY}[T ]%{HOUR}:? %{MINUTE}(?::? %{SECOND})? %{ISO8601_TIMEZONE}?
DATE %{DATE_US}|%{DATE_EU}
DATESTAMP %{DATE}[- ]%{TIME}
TZ (?:[PMCE][SD]T|UTC)
DATESTAMP_RFC822 %{DAY} %{MONTH} %{MONTHDAY} %{YEAR} %{TIME} %{TZ}
DATESTAMP_RFC2822 %{DAY}, %{MONTHDAY} %{MONTH} %{YEAR} %{TIME} %{ISO8601_TIMEZONE}
DATESTAMP_OTHER %{DAY} %{MONTH} %{MONTHDAY} %{TIME} %{TZ} %{YEAR}
DATESTAMP_EVENTLOG %{YEAR}%{MONTHNUM2}%{MONTHDAY}%{HOUR}%{MINUTE}%{SECOND}
HTTPDERROR_DATE %{DAY} %{MONTH} %{MONTHDAY} %{TIME} %{YEAR}

"""Syslog Dates: Month Day HH:MM:SS"""
SYSLOGTIMESTAMP %{MONTH} +%{MONTHDAY} %{TIME}
PROG [\x21-\x5a\x5c\x5e-\x7e]+
SYSLOGPROG %{PROG:program}(?:\[%{POSINT:pid}\])?
SYSLOGHOST %{IPORHOST}
SYSLOGFACILITY <%{NONNEGINT:facility}. %{NONNEGINT:priority}>
HTTPDATE %{MONTHDAY}/%{MONTH}/%{YEAR}:%{TIME} %{INT}

"""Shortcuts"""
QS %{QUOTEDSTRING}

"""Log formats"""
SYSLOGBASE %{SYSLOGTIMESTAMP:timestamp} (?:%{SYSLOGFACILITY} )? %{SYSLOGHOST:logsource} %{SYSLOGPROG}:
COMMONAPACHELOG %{IPORHOST:clientip} %{HTTPDUSER:ident} %{USER:auth} \[%{HTTPDATE:timestamp}\] "(?:%{WORD:verb} %{NOTSPACE:request}(?: HTTP/%{NUMBER:httpversion})? |%{DATA:rawrequest})" %{NUMBER:response} (?:%{NUMBER:bytes}|-)
COMBINEDAPACHELOG %{COMMONAPACHELOG} %{QS:referrer} %{QS:agent}
HTTPD20_ERRORLOG \[%{HTTPDERROR_DATE:timestamp}\] \[%{LOGLEVEL:loglevel}\] (?:\[client %{IPORHOST:clientip}\] ){0,1}%{GREEDYDATA:errormsg}
HTTPD24_ERRORLOG \[%{HTTPDERROR_DATE:timestamp}\] \[%{WORD:module}:%{LOGLEVEL:loglevel}\] \[pid %{POSINT:pid}:tid %{NUMBER:tid}\]( \(%{POSINT:proxy_errorcode}\)%{DATA:proxy_errormessage}:)?( \[client %{IPORHOST:client}:%{POSINT:clientport}\])? %{DATA:errorcode}: %{GREEDYDATA:message}
HTTPD_ERRORLOG %{HTTPD20_ERRORLOG}|%{HTTPD24_ERRORLOG}

"""Log Levels"""
LOGLEVEL ([Aa]lert|ALERT|[Tt]race|TRACE|[Dd]ebug|DEBUG|[Nn]otice|NOTICE|[Ii]nfo|INFO|[Ww]arn?(?:ing)? |WARN?(?:ING)? |[Ee]rr?(?:or)? |ERR?(?:OR)? |[Cc]rit?(?:ical)? |CRIT?(?:ICAL)? |[Ff]atal|FATAL|[Ss]evere|SEVERE|EMERG(?:ENCY)? |[Ee]merg(?:ency)?)
```

## Examples of basic syntax

```
CHINAID [1-9]\d{5})((18|19|([23]\d))\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{3}[0-9Xx]$)|(^[1-9]\d{5}\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{3}
USERNAME [a-zA-Z0-9. _-]+
USER %{USERNAME}
EMAILLOCALPART [a-zA-Z][a-zA-Z0-9_. +-=:]+
EMAILADDRESS %{EMAILLOCALPART}@%{HOSTNAME}
HTTPDUSER %{EMAILADDRESS}|%{USER}
INT (?:[+-]?(?:[0-9]+))
BASE10NUM (? <![ 0-9. +-])(? >[+-]?(?:(?:[0-9]+(?:\.[ 0-9]+)?) |(?:\.[ 0-9]+)))
NUMBER (?:%{BASE10NUM})
BASE16NUM (? <![ 0-9A-Fa-f])(?:[+-]?(?:0x)?(?:[0-9A-Fa-f]+))
BASE16FLOAT \b(? <![ 0-9A-Fa-f.])(?:[+-]?(?:0x)?(?:(?:[0-9A-Fa-f]+(?:\.[ 0-9A-Fa-f]*)?) |(?:\.[ 0-9A-Fa-f]+)))\b

POSINT \b(?:[1-9][0-9]*)\b
NONNEGINT \b(?:[0-9]+)\b
WORD \b\w+\b
NOTSPACE \S+
SPACE \s*
DATA . *?
GREEDYDATA . *
QUOTEDSTRING (? >(? <! \\)(? >"(? >\\. |[^\\"]+)+"|""|(? >'(? >\\. |[^\\']+)+')|''|(? >`(? >\\. |[^\\`]+)+`)|``))
UUID [A-Fa-f0-9]{8}-(?:[A-Fa-f0-9]{4}-){3}[A-Fa-f0-9]{12}

"""Networking"""
MAC (?:%{CISCOMAC}|%{WINDOWSMAC}|%{COMMONMAC})
CISCOMAC (?:(?:[A-Fa-f0-9]{4}\.){2} [A-Fa-f0-9]{4})
WINDOWSMAC (?:(?:[A-Fa-f0-9]{2}-){5}[A-Fa-f0-9]{2})
COMMONMAC (?:(?:[A-Fa-f0-9]{2}:){5}[A-Fa-f0-9]{2})
IPV6 ((([0-9A-Fa-f]{1,4}:){7}([0-9A-Fa-f]{1,4}|:))|(([0-9A-Fa-f]{1,4}:){6}(:[0-9A-Fa-f]{1,4}|((25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)){3})|:))|(([0-9A-Fa-f]{1,4}:){5}(((:[0-9A-Fa-f]{1,4}){1,2})|:((25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)){3})|:))|(([0-9A-Fa-f]{1,4}:){4}(((:[0-9A-Fa-f]{1,4}){1,3})|((:[0-9A-Fa-f]{1,4})?:((25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)){3}))|:))|(([0-9A-Fa-f]{1,4}:){3}(((:[0-9A-Fa-f]{1,4}){1,4})|((:[0-9A-Fa-f]{1,4}){0,2}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)){3}))|:))|(([0-9A-Fa-f]{1,4}:){2}(((:[0-9A-Fa-f]{1,4}){1,5})|((:[0-9A-Fa-f]{1,4}){0,3}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)){3}))|:))|(([0-9A-Fa-f]{1,4}:){1}(((:[0-9A-Fa-f]{1,4}){1,6})|((:[0-9A-Fa-f]{1,4}){0,4}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)){3}))|:))|(:(((:[0-9A-Fa-f]{1,4}){1,7})|((:[0-9A-Fa-f]{1,4}){0,5}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]? \d)){3}))|:)))(%. +)?
IPV4 (? <![ 0-9])(?:(?:[0-1]?[ 0-9]{1,2}|2[0-4][0-9]|25[0-5])[.](?:[0-1]?[ 0-9]{1,2}|2[0-4][0-9]|25[0-5])[.](?:[0-1]?[ 0-9]{1,2}|2[0-4][0-9]|25[0-5])[.](?:[0-1]?[ 0-9]{1,2}|2[0-4][0-9]|25[0-5]))(?![ 0-9])
IP (?:%{IPV6}|%{IPV4})
HOSTNAME \b(?:[0-9A-Za-z][0-9A-Za-z-]{0,62})(?:\.(?:[0-9A-Za-z][0-9A-Za-z-]{0,62}))*(\.? |\b)
IPORHOST (?:%{IP}|%{HOSTNAME})
HOSTPORT %{IPORHOST}:%{POSINT}

"""paths"""
PATH (?:%{UNIXPATH}|%{WINPATH})
UNIXPATH (/([\w_%! $@:.,~-]+|\\.) *)+
TTY (?:/dev/(pts|tty([pq])?)( \w+)? /?(?:[0-9]+))
WINPATH (? >[A-Za-z]+:|\\)(?:\\[^\\? *]*)+
URIPROTO [A-Za-z]+(\+[A-Za-z+]+)?
URIHOST %{IPORHOST}(?::%{POSINT:port})?
"""uripath comes loosely from RFC1738, but mostly from what Firefox"""
"""doesn't turn into %XX"""
URIPATH (?:/[A-Za-z0-9$. +! *'(){},~:;=@#%_\-]*)+
"""URIPARAM \?(?:[A-Za-z0-9]+(?:=(?:[^&]*))?(?:&(?:[A-Za-z0-9]+(?:=(?:[^&]*))?)?) *)?"""
URIPARAM \?[ A-Za-z0-9$. +! *'|(){},~@#%&/=:;_? \-\[\]<>]*
URIPATHPARAM %{URIPATH}(?:%{URIPARAM})?
URI %{URIPROTO}://(?:%{USER}(?::[^@]*)? @)?(?:%{URIHOST})?(?:%{URIPATHPARAM})?

""" Months: January, Feb, 3, 03, 12, December"""
MONTH \b(?:Jan(?:uary|uar)? |Feb(?:ruary|ruar)? |M(?:a|ä)? r(?:ch|z)? |Apr(?:il)? |Ma(?:y|i)? |Jun(?:e|i)? |Jul(?:y)? |Aug(?:ust)? |Sep(?:tember)? |O(?:c|k)? t(?:ober)? |Nov(?:ember)? |De(?:c|z)(?:ember)?) \b
MONTHNUM (?:0?[ 1-9]|1[0-2])
MONTHNUM2 (?:0[1-9]|1[0-2])
MONTHDAY (?:(?:0[1-9])|(?:[12][0-9])|(?:3[01])|[1-9])

"""Days: Monday, Tue, Thu, etc..."""
DAY (?:Mon(?:day)? |Tue(?:sday)? |Wed(?:nesday)? |Thu(?:rsday)? |Fri(?:day)? |Sat(?:urday)? |Sun(?:day)?)

""" Years?"""
YEAR (? >\d\d){1,2}
HOUR (?:2[0123]|[01]?[ 0-9])
MINUTE (?:[0-5][0-9])
"""'60' is a leap second in most time standards and thus is valid."""
SECOND (?:(?:[0-5]?[ 0-9]|60)(?:[:.,][0-9]+)?)
TIME (?! <[0-9])%{HOUR}:%{MINUTE}(?::%{SECOND})(?![ 0-9])
"""datestamp is YYYY/MM/DD-HH:MM:SS.UUUU (or something like it)"""
DATE_US %{MONTHNUM}[/-]%{MONTHDAY}[/-]%{YEAR}
DATE_EU %{MONTHDAY}[. /-]%{MONTHNUM}[. /-]%{YEAR}
ISO8601_TIMEZONE (?:Z|[+-]%{HOUR}(?::? %{MINUTE}))
ISO8601_SECOND (?:%{SECOND}|60)
TIMESTAMP_ISO8601 %{YEAR}-%{MONTHNUM}-%{MONTHDAY}[T ]%{HOUR}:? %{MINUTE}(?::? %{SECOND})? %{ISO8601_TIMEZONE}?
DATE %{DATE_US}|%{DATE_EU}
DATESTAMP %{DATE}[- ]%{TIME}
TZ (?:[PMCE][SD]T|UTC)
DATESTAMP_RFC822 %{DAY} %{MONTH} %{MONTHDAY} %{YEAR} %{TIME} %{TZ}
DATESTAMP_RFC2822 %{DAY}, %{MONTHDAY} %{MONTH} %{YEAR} %{TIME} %{ISO8601_TIMEZONE}
DATESTAMP_OTHER %{DAY} %{MONTH} %{MONTHDAY} %{TIME} %{TZ} %{YEAR}
DATESTAMP_EVENTLOG %{YEAR}%{MONTHNUM2}%{MONTHDAY}%{HOUR}%{MINUTE}%{SECOND}
HTTPDERROR_DATE %{DAY} %{MONTH} %{MONTHDAY} %{TIME} %{YEAR}

"""Syslog Dates: Month Day HH:MM:SS"""
SYSLOGTIMESTAMP %{MONTH} +%{MONTHDAY} %{TIME}
PROG [\x21-\x5a\x5c\x5e-\x7e]+
SYSLOGPROG %{PROG:program}(?:\[%{POSINT:pid}\])?
SYSLOGHOST %{IPORHOST}
SYSLOGFACILITY <%{NONNEGINT:facility}. %{NONNEGINT:priority}>
HTTPDATE %{MONTHDAY}/%{MONTH}/%{YEAR}:%{TIME} %{INT}

"""Shortcuts"""
QS %{QUOTEDSTRING}

"""Log formats"""
SYSLOGBASE %{SYSLOGTIMESTAMP:timestamp} (?:%{SYSLOGFACILITY} )? %{SYSLOGHOST:logsource} %{SYSLOGPROG}:
COMMONAPACHELOG %{IPORHOST:clientip} %{HTTPDUSER:ident} %{USER:auth} \[%{HTTPDATE:timestamp}\] "(?:%{WORD:verb} %{NOTSPACE:request}(?: HTTP/%{NUMBER:httpversion})? |%{DATA:rawrequest})" %{NUMBER:response} (?:%{NUMBER:bytes}|-)
COMBINEDAPACHELOG %{COMMONAPACHELOG} %{QS:referrer} %{QS:agent}
HTTPD20_ERRORLOG \[%{HTTPDERROR_DATE:timestamp}\] \[%{LOGLEVEL:loglevel}\] (?:\[client %{IPORHOST:clientip}\] ){0,1}%{GREEDYDATA:errormsg}
HTTPD24_ERRORLOG \[%{HTTPDERROR_DATE:timestamp}\] \[%{WORD:module}:%{LOGLEVEL:loglevel}\] \[pid %{POSINT:pid}:tid %{NUMBER:tid}\]( \(%{POSINT:proxy_errorcode}\)%{DATA:proxy_errormessage}:)?( \[client %{IPORHOST:client}:%{POSINT:clientport}\])? %{DATA:errorcode}: %{GREEDYDATA:message}
HTTPD_ERRORLOG %{HTTPD20_ERRORLOG}|%{HTTPD24_ERRORLOG}

"""Log Levels"""
LOGLEVEL ([Aa]lert|ALERT|[Tt]race|TRACE|[Dd]ebug|DEBUG|[Nn]otice|NOTICE|[Ii]nfo|INFO|[Ww]arn?(?:ing)? |WARN?(?:ING)? |[Ee]rr?(?:or)? |ERR?(?:OR)? |[Cc]rit?(?:ical)? |CRIT?(?:ICAL)? |[Ff]atal|FATAL|[Ss]evere|SEVERE|EMERG(?:ENCY)? |[Ee]merg(?:ency)?)
```

## Sample patterns for AWS

```
S3_REQUEST_LINE (?:%{WORD:verb} %{NOTSPACE:request}(?: HTTP/%{NUMBER:httpversion})? |%{DATA:rawrequest})

S3_ACCESS_LOG %{WORD:owner} %{NOTSPACE:bucket} \[%{HTTPDATE:timestamp}\] %{IP:clientip} %{NOTSPACE:requester} %{NOTSPACE:request_id} %{NOTSPACE:operation} %{NOTSPACE:key} (?:"%{S3_REQUEST_LINE}"|-) (?:%{INT:response:int}|-) (?:-|%{NOTSPACE:error_code}) (?:%{INT:bytes:int}|-) (?:%{INT:object_size:int}|-) (?:%{INT:request_time_ms:int}|-) (?:%{INT:turnaround_time_ms:int}|-) (?:%{QS:referrer}|-) (?:"? %{QS:agent}"? |-) (?:-|%{NOTSPACE:version_id})

ELB_URIPATHPARAM %{URIPATH:path}(?:%{URIPARAM:params})?

ELB_URI %{URIPROTO:proto}://(?:%{USER}(?::[^@]*)? @)?(?:%{URIHOST:urihost})?(?:%{ELB_URIPATHPARAM})?

ELB_REQUEST_LINE (?:%{WORD:verb} %{ELB_URI:request}(?: HTTP/%{NUMBER:httpversion})? |%{DATA:rawrequest})

ELB_ACCESS_LOG %{TIMESTAMP_ISO8601:timestamp} %{NOTSPACE:elb} %{IP:clientip}:%{INT:clientport:int} (?:(%{IP:backendip}:?:%{INT:backendport:int})|-) %{NUMBER:request_processing_time:float} %{NUMBER:backend_processing_time:float} %{NUMBER:response_processing_time:float} %{INT:response:int} %{INT:backend_response:int} %{INT:received_bytes:int} %{INT:bytes:int} "%{ELB_REQUEST_LINE}"
```

## Sample patterns for Bacula

```
BACULA_TIMESTAMP %{MONTHDAY}-%{MONTH} %{HOUR}:%{MINUTE}
BACULA_HOST [a-zA-Z0-9-]+
BACULA_VOLUME %{USER}
BACULA_DEVICE %{USER}
BACULA_DEVICEPATH %{UNIXPATH}
BACULA_CAPACITY %{INT}{1,3}(,%{INT}{3})*
BACULA_VERSION %{USER}
BACULA_JOB %{USER}

BACULA_LOG_MAX_CAPACITY User defined maximum volume capacity %{BACULA_CAPACITY} exceeded on device \"%{BACULA_DEVICE:device}\" \(%{BACULA_DEVICEPATH}\)
BACULA_LOG_END_VOLUME End of medium on Volume \"%{BACULA_VOLUME:volume}\" Bytes=%{BACULA_CAPACITY} Blocks=%{BACULA_CAPACITY} at %{MONTHDAY}-%{MONTH}-%{YEAR} %{HOUR}:%{MINUTE}.
BACULA_LOG_NEW_VOLUME Created new Volume \"%{BACULA_VOLUME:volume}\" in catalog.
BACULA_LOG_NEW_LABEL Labeled new Volume \"%{BACULA_VOLUME:volume}\" on device \"%{BACULA_DEVICE:device}\" \(%{BACULA_DEVICEPATH}\).
BACULA_LOG_WROTE_LABEL Wrote label to prelabeled Volume \"%{BACULA_VOLUME:volume}\" on device \"%{BACULA_DEVICE}\" \(%{BACULA_DEVICEPATH}\)
BACULA_LOG_NEW_MOUNT New volume \"%{BACULA_VOLUME:volume}\" mounted on device \"%{BACULA_DEVICE:device}\" \(%{BACULA_DEVICEPATH}\) at %{MONTHDAY}-%{MONTH}-%{YEAR} %{HOUR}:%{MINUTE}.
BACULA_LOG_NOOPEN \s+Cannot open %{DATA}: ERR=%{GREEDYDATA:berror}
BACULA_LOG_NOOPENDIR \s+Could not open directory %{DATA}: ERR=%{GREEDYDATA:berror}
BACULA_LOG_NOSTAT \s+Could not stat %{DATA}: ERR=%{GREEDYDATA:berror}
BACULA_LOG_NOJOBS There are no more Jobs associated with Volume \"%{BACULA_VOLUME:volume}\". Marking it purged.
BACULA_LOG_ALL_RECORDS_PRUNED All records pruned from Volume \"%{BACULA_VOLUME:volume}\"; marking it \"Purged\"
BACULA_LOG_BEGIN_PRUNE_JOBS Begin pruning Jobs older than %{INT} month %{INT} days .
BACULA_LOG_BEGIN_PRUNE_FILES Begin pruning Files.
BACULA_LOG_PRUNED_JOBS Pruned %{INT} Jobs* for client %{BACULA_HOST:client} from catalog.
BACULA_LOG_PRUNED_FILES Pruned Files from %{INT} Jobs* for client %{BACULA_HOST:client} from catalog.
BACULA_LOG_ENDPRUNE End auto prune.
BACULA_LOG_STARTJOB Start Backup JobId %{INT}, Job=%{BACULA_JOB:job}
BACULA_LOG_STARTRESTORE Start Restore Job %{BACULA_JOB:job}
BACULA_LOG_USEDEVICE Using Device \"%{BACULA_DEVICE:device}\"
BACULA_LOG_DIFF_FS \s+%{UNIXPATH} is a different filesystem. Will not descend from %{UNIXPATH} into it.
BACULA_LOG_JOBEND Job write elapsed time = %{DATA:elapsed}, Transfer rate = %{NUMBER} (K|M|G)? Bytes/second
BACULA_LOG_NOPRUNE_JOBS No Jobs found to prune.
BACULA_LOG_NOPRUNE_FILES No Files found to prune.
BACULA_LOG_VOLUME_PREVWRITTEN Volume \"%{BACULA_VOLUME:volume}\" previously written, moving to end of data.
BACULA_LOG_READYAPPEND Ready to append to end of Volume \"%{BACULA_VOLUME:volume}\" size=%{INT}
BACULA_LOG_CANCELLING Cancelling duplicate JobId=%{INT}.
BACULA_LOG_MARKCANCEL JobId %{INT}, Job %{BACULA_JOB:job} marked to be canceled.
BACULA_LOG_CLIENT_RBJ shell command: run ClientRunBeforeJob \"%{GREEDYDATA:runjob}\"
BACULA_LOG_VSS (Generate )? VSS (Writer)?
BACULA_LOG_MAXSTART Fatal error: Job canceled because max start delay time exceeded.
BACULA_LOG_DUPLICATE Fatal error: JobId %{INT:duplicate} already running. Duplicate job not allowed.
BACULA_LOG_NOJOBSTAT Fatal error: No Job status returned from FD.
BACULA_LOG_FATAL_CONN Fatal error: bsock.c:133 Unable to connect to (Client: %{BACULA_HOST:client}|Storage daemon) on %{HOSTNAME}:%{POSINT}. ERR=(? <berror>%{GREEDYDATA})
BACULA_LOG_NO_CONNECT Warning: bsock.c:127 Could not connect to (Client: %{BACULA_HOST:client}|Storage daemon) on %{HOSTNAME}:%{POSINT}. ERR=(? <berror>%{GREEDYDATA})
BACULA_LOG_NO_AUTH Fatal error: Unable to authenticate with File daemon at %{HOSTNAME}. Possible causes:
BACULA_LOG_NOSUIT No prior or suitable Full backup found in catalog. Doing FULL backup.
BACULA_LOG_NOPRIOR No prior Full backup Job record found.

BACULA_LOG_JOB (Error: )? Bacula %{BACULA_HOST} %{BACULA_VERSION} \(%{BACULA_VERSION}\):

BACULA_LOGLINE %{BACULA_TIMESTAMP:bts} %{BACULA_HOST:hostname} JobId %{INT:jobid}: (%{BACULA_LOG_MAX_CAPACITY}|%{BACULA_LOG_END_VOLUME}|%{BACULA_LOG_NEW_VOLUME}|%{BACULA_LOG_NEW_LABEL}|%{BACULA_LOG_WROTE_LABEL}|%{BACULA_LOG_NEW_MOUNT}|%{BACULA_LOG_NOOPEN}|%{BACULA_LOG_NOOPENDIR}|%{BACULA_LOG_NOSTAT}|%{BACULA_LOG_NOJOBS}|%{BACULA_LOG_ALL_RECORDS_PRUNED}|%{BACULA_LOG_BEGIN_PRUNE_JOBS}|%{BACULA_LOG_BEGIN_PRUNE_FILES}|%{BACULA_LOG_PRUNED_JOBS}|%{BACULA_LOG_PRUNED_FILES}|%{BACULA_LOG_ENDPRUNE}|%{BACULA_LOG_STARTJOB}|%{BACULA_LOG_STARTRESTORE}|%{BACULA_LOG_USEDEVICE}|%{BACULA_LOG_DIFF_FS}|%{BACULA_LOG_JOBEND}|%{BACULA_LOG_NOPRUNE_JOBS}|%{BACULA_LOG_NOPRUNE_FILES}|%{BACULA_LOG_VOLUME_PREVWRITTEN}|%{BACULA_LOG_READYAPPEND}|%{BACULA_LOG_CANCELLING}|%{BACULA_LOG_MARKCANCEL}|%{BACULA_LOG_CLIENT_RBJ}|%{BACULA_LOG_VSS}|%{BACULA_LOG_MAXSTART}|%{BACULA_LOG_DUPLICATE}|%{BACULA_LOG_NOJOBSTAT}|%{BACULA_LOG_FATAL_CONN}|%{BACULA_LOG_NO_CONNECT}|%{BACULA_LOG_NO_AUTH}|%{BACULA_LOG_NOSUIT}|%{BACULA_LOG_JOB}|%{BACULA_LOG_NOPRIOR})
```

## Sample patterns for Bro

```
"""https://www.bro.org/sphinx/script-reference/log-files.html"""

"""http.log"""
BRO_HTTP %{NUMBER:ts}\t%{NOTSPACE:uid}\t%{IP:orig_h}\t%{INT:orig_p}\t%{IP:resp_h}\t%{INT:resp_p}\t%{INT:trans_depth}\t%{GREEDYDATA:method}\t%{GREEDYDATA:domain}\t%{GREEDYDATA:uri}\t%{GREEDYDATA:referrer}\t%{GREEDYDATA:user_agent}\t%{NUMBER:request_body_len}\t%{NUMBER:response_body_len}\t%{GREEDYDATA:status_code}\t%{GREEDYDATA:status_msg}\t%{GREEDYDATA:info_code}\t%{GREEDYDATA:info_msg}\t%{GREEDYDATA:filename}\t%{GREEDYDATA:bro_tags}\t%{GREEDYDATA:username}\t%{GREEDYDATA:password}\t%{GREEDYDATA:proxied}\t%{GREEDYDATA:orig_fuids}\t%{GREEDYDATA:orig_mime_types}\t%{GREEDYDATA:resp_fuids}\t%{GREEDYDATA:resp_mime_types}

"""dns.log"""
BRO_DNS %{NUMBER:ts}\t%{NOTSPACE:uid}\t%{IP:orig_h}\t%{INT:orig_p}\t%{IP:resp_h}\t%{INT:resp_p}\t%{WORD:proto}\t%{INT:trans_id}\t%{GREEDYDATA:query}\t%{GREEDYDATA:qclass}\t%{GREEDYDATA:qclass_name}\t%{GREEDYDATA:qtype}\t%{GREEDYDATA:qtype_name}\t%{GREEDYDATA:rcode}\t%{GREEDYDATA:rcode_name}\t%{GREEDYDATA:AA}\t%{GREEDYDATA:TC}\t%{GREEDYDATA:RD}\t%{GREEDYDATA:RA}\t%{GREEDYDATA:Z}\t%{GREEDYDATA:answers}\t%{GREEDYDATA:TTLs}\t%{GREEDYDATA:rejected}

"""conn.log"""
BRO_CONN %{NUMBER:ts}\t%{NOTSPACE:uid}\t%{IP:orig_h}\t%{INT:orig_p}\t%{IP:resp_h}\t%{INT:resp_p}\t%{WORD:proto}\t%{GREEDYDATA:service}\t%{NUMBER:duration}\t%{NUMBER:orig_bytes}\t%{NUMBER:resp_bytes}\t%{GREEDYDATA:conn_state}\t%{GREEDYDATA:local_orig}\t%{GREEDYDATA:missed_bytes}\t%{GREEDYDATA:history}\t%{GREEDYDATA:orig_pkts}\t%{GREEDYDATA:orig_ip_bytes}\t%{GREEDYDATA:resp_pkts}\t%{GREEDYDATA:resp_ip_bytes}\t%{GREEDYDATA:tunnel_parents}

"""files.log"""
BRO_FILES %{NUMBER:ts}\t%{NOTSPACE:fuid}\t%{IP:tx_hosts}\t%{IP:rx_hosts}\t%{NOTSPACE:conn_uids}\t%{GREEDYDATA:source}\t%{GREEDYDATA:depth}\t%{GREEDYDATA:analyzers}\t%{GREEDYDATA:mime_type}\t%{GREEDYDATA:filename}\t%{GREEDYDATA:duration}\t%{GREEDYDATA:local_orig}\t%{GREEDYDATA:is_orig}\t%{GREEDYDATA:seen_bytes}\t%{GREEDYDATA:total_bytes}\t%{GREEDYDATA:missing_bytes}\t%{GREEDYDATA:overflow_bytes}\t%{GREEDYDATA:timedout}\t%{GREEDYDATA:parent_fuid}\t%{GREEDYDATA:md5}\t%{GREEDYDATA:sha1}\t%{GREEDYDATA:sha256}\t%{GREEDYDATA:extracted}
```

## Sample patterns for Exim

```
EXIM_MSGID [0-9A-Za-z]{6}-[0-9A-Za-z]{6}-[0-9A-Za-z]{2}
EXIM_FLAGS (<=|[-=>*]>|[*]{2}|==)
EXIM_DATE %{YEAR:exim_year}-%{MONTHNUM:exim_month}-%{MONTHDAY:exim_day} %{TIME:exim_time}
EXIM_PID \[%{POSINT}\]
EXIM_QT ((\d+y)?( \d+w)?( \d+d)?( \d+h)?( \d+m)?( \d+s)?)
EXIM_EXCLUDE_TERMS (Message is frozen|(Start|End) queue run| Warning: | retry time not reached | no (IP address|host name) found for (IP address|host) | unexpected disconnection while reading SMTP command | no immediate delivery: |another process is handling this message)
EXIM_REMOTE_HOST (H=(%{NOTSPACE:remote_hostname} )?( \(%{NOTSPACE:remote_heloname}\) )? \[%{IP:remote_host}\])
EXIM_INTERFACE (I=\[%{IP:exim_interface}\](:%{NUMBER:exim_interface_port}))
EXIM_PROTOCOL (P=%{NOTSPACE:protocol})
EXIM_MSG_SIZE (S=%{NUMBER:exim_msg_size})
EXIM_HEADER_ID (id=%{NOTSPACE:exim_header_id})
EXIM_SUBJECT (T=%{QS:exim_subject})
```

## Sample patterns for Cisco firewalls

```
""" NetScreen firewall logs"""
NETSCREENSESSIONLOG %{SYSLOGTIMESTAMP:date} %{IPORHOST:device} %{IPORHOST}: NetScreen device_id=%{WORD:device_id}%{DATA}: start_time=%{QUOTEDSTRING:start_time} duration=%{INT:duration} policy_id=%{INT:policy_id} service=%{DATA:service} proto=%{INT:proto} src zone=%{WORD:src_zone} dst zone=%{WORD:dst_zone} action=%{WORD:action} sent=%{INT:sent} rcvd=%{INT:rcvd} src=%{IPORHOST:src_ip} dst=%{IPORHOST:dst_ip} src_port=%{INT:src_port} dst_port=%{INT:dst_port} src-xlated ip=%{IPORHOST:src_xlated_ip} port=%{INT:src_xlated_port} dst-xlated ip=%{IPORHOST:dst_xlated_ip} port=%{INT:dst_xlated_port} session_id=%{INT:session_id} reason=%{GREEDYDATA:reason}

"""== Cisco ASA =="""
CISCO_TAGGED_SYSLOG ^<%{POSINT:syslog_pri}>%{CISCOTIMESTAMP:timestamp}( %{SYSLOGHOST:sysloghost})? ?: %%{CISCOTAG:ciscotag}:
CISCOTIMESTAMP %{MONTH} +%{MONTHDAY}(?: %{YEAR})? %{TIME}
CISCOTAG [A-Z0-9]+-%{INT}-(?:[A-Z0-9_]+)
"""Common Particles"""
CISCO_ACTION Built|Teardown|Deny|Denied|denied|requested|permitted|denied by ACL|discarded|est-allowed|Dropping|created|deleted
CISCO_REASON Duplicate TCP SYN|Failed to locate egress interface|Invalid transport field|No matching connection|DNS Response|DNS Query|(?:%{WORD}\s*)*
CISCO_DIRECTION Inbound|inbound|Outbound|outbound
CISCO_INTERVAL first hit|%{INT}-second interval
CISCO_XLATE_TYPE static|dynamic
"""ASA-1-104001"""
CISCOFW104001 \((?:Primary|Secondary)\) Switching to ACTIVE - %{GREEDYDATA:switch_reason}
"""ASA-1-104002"""
CISCOFW104002 \((?:Primary|Secondary)\) Switching to STANDBY - %{GREEDYDATA:switch_reason}
"""ASA-1-104003"""
CISCOFW104003 \((?:Primary|Secondary)\) Switching to FAILED\.
"""ASA-1-104004"""
CISCOFW104004 \((?:Primary|Secondary)\) Switching to OK\.
"""ASA-1-105003"""
CISCOFW105003 \((?:Primary|Secondary)\) Monitoring on [Ii]nterface %{GREEDYDATA:interface_name} waiting
"""ASA-1-105004"""
CISCOFW105004 \((?:Primary|Secondary)\) Monitoring on [Ii]nterface %{GREEDYDATA:interface_name} normal
"""ASA-1-105005"""
CISCOFW105005 \((?:Primary|Secondary)\) Lost Failover communications with mate on [Ii]nterface %{GREEDYDATA:interface_name}
"""ASA-1-105008"""
CISCOFW105008 \((?:Primary|Secondary)\) Testing [Ii]nterface %{GREEDYDATA:interface_name}
"""ASA-1-105009"""
CISCOFW105009 \((?:Primary|Secondary)\) Testing on [Ii]nterface %{GREEDYDATA:interface_name} (?:Passed|Failed)
"""ASA-2-106001"""
CISCOFW106001 %{CISCO_DIRECTION:direction} %{WORD:protocol} connection %{CISCO_ACTION:action} from %{IP:src_ip}/%{INT:src_port} to %{IP:dst_ip}/%{INT:dst_port} flags %{GREEDYDATA:tcp_flags} on interface %{GREEDYDATA:interface}
"""ASA-2-106006, ASA-2-106007, ASA-2-106010"""
CISCOFW106006_106007_106010 %{CISCO_ACTION:action} %{CISCO_DIRECTION:direction} %{WORD:protocol} (?:from|src) %{IP:src_ip}/%{INT:src_port}(\(%{DATA:src_fwuser}\))? (?:to|dst) %{IP:dst_ip}/%{INT:dst_port}(\(%{DATA:dst_fwuser}\))? (?:on interface %{DATA:interface}|due to %{CISCO_REASON:reason})
"""ASA-3-106014"""
CISCOFW106014 %{CISCO_ACTION:action} %{CISCO_DIRECTION:direction} %{WORD:protocol} src %{DATA:src_interface}:%{IP:src_ip}(\(%{DATA:src_fwuser}\))? dst %{DATA:dst_interface}:%{IP:dst_ip}(\(%{DATA:dst_fwuser}\))? \(type %{INT:icmp_type}, code %{INT:icmp_code}\)
"""ASA-6-106015"""
CISCOFW106015 %{CISCO_ACTION:action} %{WORD:protocol} \(%{DATA:policy_id}\) from %{IP:src_ip}/%{INT:src_port} to %{IP:dst_ip}/%{INT:dst_port} flags %{DATA:tcp_flags}  on interface %{GREEDYDATA:interface}
"""ASA-1-106021"""
CISCOFW106021 %{CISCO_ACTION:action} %{WORD:protocol} reverse path check from %{IP:src_ip} to %{IP:dst_ip} on interface %{GREEDYDATA:interface}
"""ASA-4-106023"""
CISCOFW106023 %{CISCO_ACTION:action}( protocol)? %{WORD:protocol} src %{DATA:src_interface}:%{DATA:src_ip}(/%{INT:src_port})?( \(%{DATA:src_fwuser}\))? dst %{DATA:dst_interface}:%{DATA:dst_ip}(/%{INT:dst_port})?( \(%{DATA:dst_fwuser}\))?( \(type %{INT:icmp_type}, code %{INT:icmp_code}\))? by access-group "? %{DATA:policy_id}"? \[%{DATA:hashcode1}, %{DATA:hashcode2}\]
"""ASA-4-106100, ASA-4-106102, ASA-4-106103"""
CISCOFW106100_2_3 access-list %{NOTSPACE:policy_id} %{CISCO_ACTION:action} %{WORD:protocol} for user '%{DATA:src_fwuser}' %{DATA:src_interface}/%{IP:src_ip}\(%{INT:src_port}\) -> %{DATA:dst_interface}/%{IP:dst_ip}\(%{INT:dst_port}\) hit-cnt %{INT:hit_count} %{CISCO_INTERVAL:interval} \[%{DATA:hashcode1}, %{DATA:hashcode2}\]
"""ASA-5-106100"""
CISCOFW106100 access-list %{NOTSPACE:policy_id} %{CISCO_ACTION:action} %{WORD:protocol} %{DATA:src_interface}/%{IP:src_ip}\(%{INT:src_port}\)(\(%{DATA:src_fwuser}\))? -> %{DATA:dst_interface}/%{IP:dst_ip}\(%{INT:dst_port}\)(\(%{DATA:src_fwuser}\))? hit-cnt %{INT:hit_count} %{CISCO_INTERVAL:interval} \[%{DATA:hashcode1}, %{DATA:hashcode2}\]
"""ASA-6-110002"""
CISCOFW110002 %{CISCO_REASON:reason} for %{WORD:protocol} from %{DATA:src_interface}:%{IP:src_ip}/%{INT:src_port} to %{IP:dst_ip}/%{INT:dst_port}
"""ASA-6-302010"""
CISCOFW302010 %{INT:connection_count} in use, %{INT:connection_count_max} most used
"""ASA-6-302013, ASA-6-302014, ASA-6-302015, ASA-6-302016"""
CISCOFW302013_302014_302015_302016 %{CISCO_ACTION:action}(?: %{CISCO_DIRECTION:direction})? %{WORD:protocol} connection %{INT:connection_id} for %{DATA:src_interface}:%{IP:src_ip}/%{INT:src_port}( \(%{IP:src_mapped_ip}/%{INT:src_mapped_port}\))?( \(%{DATA:src_fwuser}\))? to %{DATA:dst_interface}:%{IP:dst_ip}/%{INT:dst_port}( \(%{IP:dst_mapped_ip}/%{INT:dst_mapped_port}\))?( \(%{DATA:dst_fwuser}\))?( duration %{TIME:duration} bytes %{INT:bytes})?(?: %{CISCO_REASON:reason})?( \(%{DATA:user}\))?
"""ASA-6-302020, ASA-6-302021"""
CISCOFW302020_302021 %{CISCO_ACTION:action}(?: %{CISCO_DIRECTION:direction})? %{WORD:protocol} connection for faddr %{IP:dst_ip}/%{INT:icmp_seq_num}(?:\(%{DATA:fwuser}\))? gaddr %{IP:src_xlated_ip}/%{INT:icmp_code_xlated} laddr %{IP:src_ip}/%{INT:icmp_code}( \(%{DATA:user}\))?
"""ASA-6-305011"""
CISCOFW305011 %{CISCO_ACTION:action} %{CISCO_XLATE_TYPE:xlate_type} %{WORD:protocol} translation from %{DATA:src_interface}:%{IP:src_ip}(/%{INT:src_port})?( \(%{DATA:src_fwuser}\))? to %{DATA:src_xlated_interface}:%{IP:src_xlated_ip}/%{DATA:src_xlated_port}
"""ASA-3-313001, ASA-3-313004, ASA-3-313008"""
CISCOFW313001_313004_313008 %{CISCO_ACTION:action} %{WORD:protocol} type=%{INT:icmp_type}, code=%{INT:icmp_code} from %{IP:src_ip} on interface %{DATA:interface}( to %{IP:dst_ip})?
"""ASA-4-313005"""
CISCOFW313005 %{CISCO_REASON:reason} for %{WORD:protocol} error message: %{WORD:err_protocol} src %{DATA:err_src_interface}:%{IP:err_src_ip}(\(%{DATA:err_src_fwuser}\))? dst %{DATA:err_dst_interface}:%{IP:err_dst_ip}(\(%{DATA:err_dst_fwuser}\))? \(type %{INT:err_icmp_type}, code %{INT:err_icmp_code}\) on %{DATA:interface} interface\.  Original IP payload: %{WORD:protocol} src %{IP:orig_src_ip}/%{INT:orig_src_port}(\(%{DATA:orig_src_fwuser}\))? dst %{IP:orig_dst_ip}/%{INT:orig_dst_port}(\(%{DATA:orig_dst_fwuser}\))?
"""ASA-5-321001"""
CISCOFW321001 Resource '%{WORD:resource_name}' limit of %{POSINT:resource_limit} reached for system
"""ASA-4-402117"""
CISCOFW402117 %{WORD:protocol}: Received a non-IPSec packet \(protocol= %{WORD:orig_protocol}\) from %{IP:src_ip} to %{IP:dst_ip}
"""ASA-4-402119"""
CISCOFW402119 %{WORD:protocol}: Received an %{WORD:orig_protocol} packet \(SPI= %{DATA:spi}, sequence number= %{DATA:seq_num}\) from %{IP:src_ip} \(user= %{DATA:user}\) to %{IP:dst_ip} that failed anti-replay checking
"""ASA-4-419001"""
CISCOFW419001 %{CISCO_ACTION:action} %{WORD:protocol} packet from %{DATA:src_interface}:%{IP:src_ip}/%{INT:src_port} to %{DATA:dst_interface}:%{IP:dst_ip}/%{INT:dst_port}, reason: %{GREEDYDATA:reason}
"""ASA-4-419002"""
CISCOFW419002 %{CISCO_REASON:reason} from %{DATA:src_interface}:%{IP:src_ip}/%{INT:src_port} to %{DATA:dst_interface}:%{IP:dst_ip}/%{INT:dst_port} with different initial sequence number
"""ASA-4-500004"""
CISCOFW500004 %{CISCO_REASON:reason} for protocol=%{WORD:protocol}, from %{IP:src_ip}/%{INT:src_port} to %{IP:dst_ip}/%{INT:dst_port}
"""ASA-6-602303, ASA-6-602304"""
CISCOFW602303_602304 %{WORD:protocol}: An %{CISCO_DIRECTION:direction} %{GREEDYDATA:tunnel_type} SA \(SPI= %{DATA:spi}\) between %{IP:src_ip} and %{IP:dst_ip} \(user= %{DATA:user}\) has been %{CISCO_ACTION:action}
"""ASA-7-710001, ASA-7-710002, ASA-7-710003, ASA-7-710005, ASA-7-710006"""
CISCOFW710001_710002_710003_710005_710006 %{WORD:protocol} (?:request|access) %{CISCO_ACTION:action} from %{IP:src_ip}/%{INT:src_port} to %{DATA:dst_interface}:%{IP:dst_ip}/%{INT:dst_port}
"""ASA-6-713172"""
CISCOFW713172 Group = %{GREEDYDATA:group}, IP = %{IP:src_ip}, Automatic NAT Detection Status:\s+Remote end\s*%{DATA:is_remote_natted}\s*behind a NAT device\s+This\s+end\s*%{DATA:is_local_natted}\s*behind a NAT device
"""ASA-4-733100"""
CISCOFW733100 \[\s*%{DATA:drop_type}\s*\] drop %{DATA:drop_rate_id} exceeded. Current burst rate is %{INT:drop_rate_current_burst} per second, max configured rate is %{INT:drop_rate_max_burst}; Current average rate is %{INT:drop_rate_current_avg} per second, max configured rate is %{INT:drop_rate_max_avg}; Cumulative total count is %{INT:drop_total_count}
"""== End Cisco ASA =="""

"""Shorewall firewall logs"""
SHOREWALL (%{SYSLOGTIMESTAMP:timestamp}) (%{WORD:nf_host}) kernel:. *Shorewall:(%{WORD:nf_action1})?:(%{WORD:nf_action2})?. *IN=(%{USERNAME:nf_in_interface})?. *(OUT= *MAC=(%{COMMONMAC:nf_dst_mac}):(%{COMMONMAC:nf_src_mac})? |OUT=%{USERNAME:nf_out_interface}). *SRC=(%{IPV4:nf_src_ip}). *DST=(%{IPV4:nf_dst_ip}). *LEN=(%{WORD:nf_len}).? *TOS=(%{WORD:nf_tos}).? *PREC=(%{WORD:nf_prec}).? *TTL=(%{INT:nf_ttl}).? *ID=(%{INT:nf_id}).? *PROTO=(%{WORD:nf_protocol}).? *SPT=(%{INT:nf_src_port}?. *DPT=%{INT:nf_dst_port}?. *)
"""== End Shorewall"""
```

## Sample patterns for HAProxy

```
""" These patterns were tested w/ haproxy-1.4.15"""

""" Documentation of the haproxy log formats can be found at the following links:"""
""" http://code.google.com/p/haproxy-docs/wiki/HTTPLogFormat"""
""" http://code.google.com/p/haproxy-docs/wiki/TCPLogFormat"""

HAPROXYTIME (?! <[0-9])%{HOUR:haproxy_hour}:%{MINUTE:haproxy_minute}(?::%{SECOND:haproxy_second})(?![ 0-9])
HAPROXYDATE %{MONTHDAY:haproxy_monthday}/%{MONTH:haproxy_month}/%{YEAR:haproxy_year}:%{HAPROXYTIME:haproxy_time}. %{INT:haproxy_milliseconds}

""" Override these default patterns to parse out what is captured in your haproxy.cfg"""
HAPROXYCAPTUREDREQUESTHEADERS %{DATA:captured_request_headers}
HAPROXYCAPTUREDRESPONSEHEADERS %{DATA:captured_response_headers}

""" Example:"""
"""  These haproxy config lines will add data to the logs that are captured"""
"""  by the patterns below. Place them in your custom patterns directory to"""
"""  override the defaults."""
"""
"""  capture request header Host len 40"""
"""  capture request header X-Forwarded-For len 50"""
"""  capture request header Accept-Language len 50"""
"""  capture request header Referer len 200"""
"""  capture request header User-Agent len 200"""
"""
"""  capture response header Content-Type len 30"""
"""  capture response header Content-Encoding len 10"""
"""  capture response header Cache-Control len 200"""
"""  capture response header Last-Modified len 200"""
"""parse a haproxy 'httplog' line"""
HAPROXYHTTPBASE %{IP:client_ip}:%{INT:client_port} \[%{HAPROXYDATE:accept_date}\] %{NOTSPACE:frontend_name} %{NOTSPACE:backend_name}/%{NOTSPACE:server_name} %{INT:time_request}/%{INT:time_queue}/%{INT:time_backend_connect}/%{INT:time_backend_response}/%{NOTSPACE:time_duration} %{INT:http_status_code} %{NOTSPACE:bytes_read} %{DATA:captured_request_cookie} %{DATA:captured_response_cookie} %{NOTSPACE:termination_state} %{INT:actconn}/%{INT:feconn}/%{INT:beconn}/%{INT:srvconn}/%{NOTSPACE:retries} %{INT:srv_queue}/%{INT:backend_queue} (\{%{HAPROXYCAPTUREDREQUESTHEADERS}\})?( )?( \{%{HAPROXYCAPTUREDRESPONSEHEADERS}\})?( )?"( <BADREQ>|(%{WORD:http_verb} (%{URIPROTO:http_proto}://)?(?:%{USER:http_user}(?::[^@]*)? @)?(?:%{URIHOST:http_host})?(?:%{URIPATHPARAM:http_request})?( HTTP/%{NUMBER:http_version})?))?"

HAPROXYHTTP (?:%{SYSLOGTIMESTAMP:syslog_timestamp}|%{TIMESTAMP_ISO8601:timestamp8601}) %{IPORHOST:syslog_server} %{SYSLOGPROG}: %{HAPROXYHTTPBASE}

"""parse a haproxy 'tcplog' line"""
HAPROXYTCP (?:%{SYSLOGTIMESTAMP:syslog_timestamp}|%{TIMESTAMP_ISO8601:timestamp8601}) %{IPORHOST:syslog_server} %{SYSLOGPROG}: %{IP:client_ip}:%{INT:client_port} \[%{HAPROXYDATE:accept_date}\] %{NOTSPACE:frontend_name} %{NOTSPACE:backend_name}/%{NOTSPACE:server_name} %{INT:time_queue}/%{INT:time_backend_connect}/%{NOTSPACE:time_duration} %{NOTSPACE:bytes_read} %{NOTSPACE:termination_state} %{INT:actconn}/%{INT:feconn}/%{INT:beconn}/%{INT:srvconn}/%{NOTSPACE:retries} %{INT:srv_queue}/%{INT:backend_queue}
```

## Sample patterns for Java

```
JAVACLASS (?:[a-zA-Z$_][a-zA-Z$_0-9]*\.)*[a-zA-Z$_][a-zA-Z$_0-9]*
"""Space is an allowed character to match special cases like 'Native Method' or 'Unknown Source'"""
JAVAFILE (?:[A-Za-z0-9_. -]+)
"""Allow special <init> method"""
JAVAMETHOD (?:(<init>)|[a-zA-Z$_][a-zA-Z$_0-9]*)
"""Line number is optional in special cases 'Native method' or 'Unknown source'"""
JAVASTACKTRACEPART %{SPACE}at %{JAVACLASS:class}\. %{JAVAMETHOD:method}\(%{JAVAFILE:file}(?::%{NUMBER:line})? \)
""" Java Logs"""
JAVATHREAD (?:[A-Z]{2}-Processor[\d]+)
JAVACLASS (?:[a-zA-Z0-9-]+\.) +[A-Za-z0-9$]+
JAVAFILE (?:[A-Za-z0-9_. -]+)
JAVASTACKTRACEPART at %{JAVACLASS:class}\. %{WORD:method}\(%{JAVAFILE:file}:%{NUMBER:line}\)
JAVALOGMESSAGE (. *)
""" MMM dd, yyyy HH:mm:ss eg: Jan 9, 2014 7:13:13 AM"""
CATALINA_DATESTAMP %{MONTH} %{MONTHDAY}, 20%{YEAR} %{HOUR}:? %{MINUTE}(?::? %{SECOND}) (?:AM|PM)
""" yyyy-MM-dd HH:mm:ss,SSS ZZZ eg: 2014-01-09 17:32:25,527 -0800"""
TOMCAT_DATESTAMP 20%{YEAR}-%{MONTHNUM}-%{MONTHDAY} %{HOUR}:? %{MINUTE}(?::? %{SECOND}) %{ISO8601_TIMEZONE}
CATALINALOG %{CATALINA_DATESTAMP:timestamp} %{JAVACLASS:class} %{JAVALOGMESSAGE:logmessage}
""" 2014-01-09 20:03:28,269 -0800 | ERROR | com.example.service.ExampleService - something compeletely unexpected happened..."""
TOMCATLOG %{TOMCAT_DATESTAMP:timestamp} \| %{LOGLEVEL:level} \| %{JAVACLASS:class} - %{JAVALOGMESSAGE:logmessage}
```

## Sample patterns for Junos

```
"""JUNOS 11.4 RT_FLOW patterns"""
RT_FLOW_EVENT (RT_FLOW_SESSION_CREATE|RT_FLOW_SESSION_CLOSE|RT_FLOW_SESSION_DENY)

RT_FLOW1 %{RT_FLOW_EVENT:event}: %{GREEDYDATA:close-reason}: %{IP:src-ip}/%{INT:src-port}->%{IP:dst-ip}/%{INT:dst-port} %{DATA:service} %{IP:nat-src-ip}/%{INT:nat-src-port}->%{IP:nat-dst-ip}/%{INT:nat-dst-port} %{DATA:src-nat-rule-name} %{DATA:dst-nat-rule-name} %{INT:protocol-id} %{DATA:policy-name} %{DATA:from-zone} %{DATA:to-zone} %{INT:session-id} \d+\(%{DATA:sent}\) \d+\(%{DATA:received}\) %{INT:elapsed-time} . *

RT_FLOW2 %{RT_FLOW_EVENT:event}: session created %{IP:src-ip}/%{INT:src-port}->%{IP:dst-ip}/%{INT:dst-port} %{DATA:service} %{IP:nat-src-ip}/%{INT:nat-src-port}->%{IP:nat-dst-ip}/%{INT:nat-dst-port} %{DATA:src-nat-rule-name} %{DATA:dst-nat-rule-name} %{INT:protocol-id} %{DATA:policy-name} %{DATA:from-zone} %{DATA:to-zone} %{INT:session-id} . *

RT_FLOW3 %{RT_FLOW_EVENT:event}: session denied %{IP:src-ip}/%{INT:src-port}->%{IP:dst-ip}/%{INT:dst-port} %{DATA:service} %{INT:protocol-id}\(\d\) %{DATA:policy-name} %{DATA:from-zone} %{DATA:to-zone} . *
```

## Sample patterns for Linux Syslog

```
SYSLOG5424PRINTASCII [! -~]+

SYSLOGBASE2 (?:%{SYSLOGTIMESTAMP:timestamp}|%{TIMESTAMP_ISO8601:timestamp8601}) (?:%{SYSLOGFACILITY} )? %{SYSLOGHOST:logsource}+(?: %{SYSLOGPROG}:|)
SYSLOGPAMSESSION %{SYSLOGBASE} (? =%{GREEDYDATA:message})%{WORD:pam_module}\(%{DATA:pam_caller}\): session %{WORD:pam_session_state} for user %{USERNAME:username}(?: by %{GREEDYDATA:pam_by})?

CRON_ACTION [A-Z ]+
CRONLOG %{SYSLOGBASE} \(%{USER:user}\) %{CRON_ACTION:action} \(%{DATA:message}\)

SYSLOGLINE %{SYSLOGBASE2} %{GREEDYDATA:message}

"""IETF 5424 syslog(8) format (see http://www.rfc-editor.org/info/rfc5424)"""
SYSLOG5424PRI <%{NONNEGINT:syslog5424_pri}>
SYSLOG5424SD \[%{DATA}\]+
SYSLOG5424BASE %{SYSLOG5424PRI}%{NONNEGINT:syslog5424_ver} +(?:%{TIMESTAMP_ISO8601:syslog5424_ts}|-) +(?:%{HOSTNAME:syslog5424_host}|-) +(-|%{SYSLOG5424PRINTASCII:syslog5424_app}) +(-|%{SYSLOG5424PRINTASCII:syslog5424_proc}) +(-|%{SYSLOG5424PRINTASCII:syslog5424_msgid}) +(?:%{SYSLOG5424SD:syslog5424_sd}|-|)

SYSLOG5424LINE %{SYSLOG5424BASE} +%{GREEDYDATA:syslog5424_msg}
```

## Sample patterns for Mcollective

```
"""Remember, these can be multi-line events."""
MCOLLECTIVE ., \[%{TIMESTAMP_ISO8601:timestamp} #%{POSINT:pid}\]%{SPACE}%{LOGLEVEL:event_level}

MCOLLECTIVEAUDIT %{TIMESTAMP_ISO8601:timestamp}:
```

## Sample patterns for MongoDB

```
MONGO_LOG %{SYSLOGTIMESTAMP:timestamp} \[%{WORD:component}\] %{GREEDYDATA:message}
MONGO_QUERY \{ (? <={ ). *(? = } ntoreturn:) \}
MONGO_SLOWQUERY %{WORD} %{MONGO_WORDDASH:database}\. %{MONGO_WORDDASH:collection} %{WORD}: %{MONGO_QUERY:query} %{WORD}:%{NONNEGINT:ntoreturn} %{WORD}:%{NONNEGINT:ntoskip} %{WORD}:%{NONNEGINT:nscanned}.*nreturned:%{NONNEGINT:nreturned}.. + (? <duration>[0-9]+)ms
MONGO_WORDDASH \b[\w-]+\b
MONGO3_SEVERITY \w
MONGO3_COMPONENT %{WORD}|-
MONGO3_LOG %{TIMESTAMP_ISO8601:timestamp} %{MONGO3_SEVERITY:severity} %{MONGO3_COMPONENT:component}%{SPACE}(?:\[%{DATA:context}\])? %{GREEDYDATA:message}
```

## Sample patterns for Nagios

```
NAGIOSTIME \[%{NUMBER:nagios_epoch}\]
"""nagios log types"""
NAGIOS_TYPE_CURRENT_SERVICE_STATE CURRENT SERVICE STATE
NAGIOS_TYPE_CURRENT_HOST_STATE CURRENT HOST STATE

NAGIOS_TYPE_SERVICE_NOTIFICATION SERVICE NOTIFICATION
NAGIOS_TYPE_HOST_NOTIFICATION HOST NOTIFICATION

NAGIOS_TYPE_SERVICE_ALERT SERVICE ALERT
NAGIOS_TYPE_HOST_ALERT HOST ALERT

NAGIOS_TYPE_SERVICE_FLAPPING_ALERT SERVICE FLAPPING ALERT
NAGIOS_TYPE_HOST_FLAPPING_ALERT HOST FLAPPING ALERT

NAGIOS_TYPE_SERVICE_DOWNTIME_ALERT SERVICE DOWNTIME ALERT
NAGIOS_TYPE_HOST_DOWNTIME_ALERT HOST DOWNTIME ALERT

NAGIOS_TYPE_PASSIVE_SERVICE_CHECK PASSIVE SERVICE CHECK
NAGIOS_TYPE_PASSIVE_HOST_CHECK PASSIVE HOST CHECK

NAGIOS_TYPE_SERVICE_EVENT_HANDLER SERVICE EVENT HANDLER
NAGIOS_TYPE_HOST_EVENT_HANDLER HOST EVENT HANDLER

NAGIOS_TYPE_EXTERNAL_COMMAND EXTERNAL COMMAND
NAGIOS_TYPE_TIMEPERIOD_TRANSITION TIMEPERIOD TRANSITION

"""external check types"""
NAGIOS_EC_DISABLE_SVC_CHECK DISABLE_SVC_CHECK
NAGIOS_EC_ENABLE_SVC_CHECK ENABLE_SVC_CHECK
NAGIOS_EC_DISABLE_HOST_CHECK DISABLE_HOST_CHECK
NAGIOS_EC_ENABLE_HOST_CHECK ENABLE_HOST_CHECK
NAGIOS_EC_PROCESS_SERVICE_CHECK_RESULT PROCESS_SERVICE_CHECK_RESULT
NAGIOS_EC_PROCESS_HOST_CHECK_RESULT PROCESS_HOST_CHECK_RESULT
NAGIOS_EC_SCHEDULE_SERVICE_DOWNTIME SCHEDULE_SERVICE_DOWNTIME
NAGIOS_EC_SCHEDULE_HOST_DOWNTIME SCHEDULE_HOST_DOWNTIME
NAGIOS_EC_DISABLE_HOST_SVC_NOTIFICATIONS DISABLE_HOST_SVC_NOTIFICATIONS
NAGIOS_EC_ENABLE_HOST_SVC_NOTIFICATIONS ENABLE_HOST_SVC_NOTIFICATIONS
NAGIOS_EC_DISABLE_HOST_NOTIFICATIONS DISABLE_HOST_NOTIFICATIONS
NAGIOS_EC_ENABLE_HOST_NOTIFICATIONS ENABLE_HOST_NOTIFICATIONS
NAGIOS_EC_DISABLE_SVC_NOTIFICATIONS DISABLE_SVC_NOTIFICATIONS
NAGIOS_EC_ENABLE_SVC_NOTIFICATIONS ENABLE_SVC_NOTIFICATIONS


NAGIOS_WARNING Warning:%{SPACE}%{GREEDYDATA:nagios_message}

NAGIOS_CURRENT_SERVICE_STATE %{NAGIOS_TYPE_CURRENT_SERVICE_STATE:nagios_type}: %{DATA:nagios_hostname};%{DATA:nagios_service};%{DATA:nagios_state};%{DATA:nagios_statetype};%{DATA:nagios_statecode};%{GREEDYDATA:nagios_message}
NAGIOS_CURRENT_HOST_STATE %{NAGIOS_TYPE_CURRENT_HOST_STATE:nagios_type}: %{DATA:nagios_hostname};%{DATA:nagios_state};%{DATA:nagios_statetype};%{DATA:nagios_statecode};%{GREEDYDATA:nagios_message}

NAGIOS_SERVICE_NOTIFICATION %{NAGIOS_TYPE_SERVICE_NOTIFICATION:nagios_type}: %{DATA:nagios_notifyname};%{DATA:nagios_hostname};%{DATA:nagios_service};%{DATA:nagios_state};%{DATA:nagios_contact};%{GREEDYDATA:nagios_message}
NAGIOS_HOST_NOTIFICATION %{NAGIOS_TYPE_HOST_NOTIFICATION:nagios_type}: %{DATA:nagios_notifyname};%{DATA:nagios_hostname};%{DATA:nagios_state};%{DATA:nagios_contact};%{GREEDYDATA:nagios_message}

NAGIOS_SERVICE_ALERT %{NAGIOS_TYPE_SERVICE_ALERT:nagios_type}: %{DATA:nagios_hostname};%{DATA:nagios_service};%{DATA:nagios_state};%{DATA:nagios_statelevel};%{NUMBER:nagios_attempt};%{GREEDYDATA:nagios_message}
NAGIOS_HOST_ALERT %{NAGIOS_TYPE_HOST_ALERT:nagios_type}: %{DATA:nagios_hostname};%{DATA:nagios_state};%{DATA:nagios_statelevel};%{NUMBER:nagios_attempt};%{GREEDYDATA:nagios_message}

NAGIOS_SERVICE_FLAPPING_ALERT %{NAGIOS_TYPE_SERVICE_FLAPPING_ALERT:nagios_type}: %{DATA:nagios_hostname};%{DATA:nagios_service};%{DATA:nagios_state};%{GREEDYDATA:nagios_message}
NAGIOS_HOST_FLAPPING_ALERT %{NAGIOS_TYPE_HOST_FLAPPING_ALERT:nagios_type}: %{DATA:nagios_hostname};%{DATA:nagios_state};%{GREEDYDATA:nagios_message}

NAGIOS_SERVICE_DOWNTIME_ALERT %{NAGIOS_TYPE_SERVICE_DOWNTIME_ALERT:nagios_type}: %{DATA:nagios_hostname};%{DATA:nagios_service};%{DATA:nagios_state};%{GREEDYDATA:nagios_comment}
NAGIOS_HOST_DOWNTIME_ALERT %{NAGIOS_TYPE_HOST_DOWNTIME_ALERT:nagios_type}: %{DATA:nagios_hostname};%{DATA:nagios_state};%{GREEDYDATA:nagios_comment}

NAGIOS_PASSIVE_SERVICE_CHECK %{NAGIOS_TYPE_PASSIVE_SERVICE_CHECK:nagios_type}: %{DATA:nagios_hostname};%{DATA:nagios_service};%{DATA:nagios_state};%{GREEDYDATA:nagios_comment}
NAGIOS_PASSIVE_HOST_CHECK %{NAGIOS_TYPE_PASSIVE_HOST_CHECK:nagios_type}: %{DATA:nagios_hostname};%{DATA:nagios_state};%{GREEDYDATA:nagios_comment}

NAGIOS_SERVICE_EVENT_HANDLER %{NAGIOS_TYPE_SERVICE_EVENT_HANDLER:nagios_type}: %{DATA:nagios_hostname};%{DATA:nagios_service};%{DATA:nagios_state};%{DATA:nagios_statelevel};%{DATA:nagios_event_handler_name}
NAGIOS_HOST_EVENT_HANDLER %{NAGIOS_TYPE_HOST_EVENT_HANDLER:nagios_type}: %{DATA:nagios_hostname};%{DATA:nagios_state};%{DATA:nagios_statelevel};%{DATA:nagios_event_handler_name}

NAGIOS_TIMEPERIOD_TRANSITION %{NAGIOS_TYPE_TIMEPERIOD_TRANSITION:nagios_type}: %{DATA:nagios_service};%{DATA:nagios_unknown1};%{DATA:nagios_unknown2}

"""Disable host & service check"""
NAGIOS_EC_LINE_DISABLE_SVC_CHECK %{NAGIOS_TYPE_EXTERNAL_COMMAND:nagios_type}: %{NAGIOS_EC_DISABLE_SVC_CHECK:nagios_command};%{DATA:nagios_hostname};%{DATA:nagios_service}
NAGIOS_EC_LINE_DISABLE_HOST_CHECK %{NAGIOS_TYPE_EXTERNAL_COMMAND:nagios_type}: %{NAGIOS_EC_DISABLE_HOST_CHECK:nagios_command};%{DATA:nagios_hostname}

"""Enable host & service check"""
NAGIOS_EC_LINE_ENABLE_SVC_CHECK %{NAGIOS_TYPE_EXTERNAL_COMMAND:nagios_type}: %{NAGIOS_EC_ENABLE_SVC_CHECK:nagios_command};%{DATA:nagios_hostname};%{DATA:nagios_service}
NAGIOS_EC_LINE_ENABLE_HOST_CHECK %{NAGIOS_TYPE_EXTERNAL_COMMAND:nagios_type}: %{NAGIOS_EC_ENABLE_HOST_CHECK:nagios_command};%{DATA:nagios_hostname}

"""Process host & service check"""
NAGIOS_EC_LINE_PROCESS_SERVICE_CHECK_RESULT %{NAGIOS_TYPE_EXTERNAL_COMMAND:nagios_type}: %{NAGIOS_EC_PROCESS_SERVICE_CHECK_RESULT:nagios_command};%{DATA:nagios_hostname};%{DATA:nagios_service};%{DATA:nagios_state};%{GREEDYDATA:nagios_check_result}
NAGIOS_EC_LINE_PROCESS_HOST_CHECK_RESULT %{NAGIOS_TYPE_EXTERNAL_COMMAND:nagios_type}: %{NAGIOS_EC_PROCESS_HOST_CHECK_RESULT:nagios_command};%{DATA:nagios_hostname};%{DATA:nagios_state};%{GREEDYDATA:nagios_check_result}


"""Disable host & service notifications"""
NAGIOS_EC_LINE_DISABLE_HOST_SVC_NOTIFICATIONS %{NAGIOS_TYPE_EXTERNAL_COMMAND:nagios_type}: %{NAGIOS_EC_DISABLE_HOST_SVC_NOTIFICATIONS:nagios_command};%{GREEDYDATA:nagios_hostname}
NAGIOS_EC_LINE_DISABLE_HOST_NOTIFICATIONS %{NAGIOS_TYPE_EXTERNAL_COMMAND:nagios_type}: %{NAGIOS_EC_DISABLE_HOST_NOTIFICATIONS:nagios_command};%{GREEDYDATA:nagios_hostname}
NAGIOS_EC_LINE_DISABLE_SVC_NOTIFICATIONS %{NAGIOS_TYPE_EXTERNAL_COMMAND:nagios_type}: %{NAGIOS_EC_DISABLE_SVC_NOTIFICATIONS:nagios_command};%{DATA:nagios_hostname};%{GREEDYDATA:nagios_service}

"""Enable host & service notifications"""
NAGIOS_EC_LINE_ENABLE_HOST_SVC_NOTIFICATIONS %{NAGIOS_TYPE_EXTERNAL_COMMAND:nagios_type}: %{NAGIOS_EC_ENABLE_HOST_SVC_NOTIFICATIONS:nagios_command};%{GREEDYDATA:nagios_hostname}
NAGIOS_EC_LINE_ENABLE_HOST_NOTIFICATIONS %{NAGIOS_TYPE_EXTERNAL_COMMAND:nagios_type}: %{NAGIOS_EC_ENABLE_HOST_NOTIFICATIONS:nagios_command};%{GREEDYDATA:nagios_hostname}
NAGIOS_EC_LINE_ENABLE_SVC_NOTIFICATIONS %{NAGIOS_TYPE_EXTERNAL_COMMAND:nagios_type}: %{NAGIOS_EC_ENABLE_SVC_NOTIFICATIONS:nagios_command};%{DATA:nagios_hostname};%{GREEDYDATA:nagios_service}

"""Schedule host & service downtime"""
NAGIOS_EC_LINE_SCHEDULE_HOST_DOWNTIME %{NAGIOS_TYPE_EXTERNAL_COMMAND:nagios_type}: %{NAGIOS_EC_SCHEDULE_HOST_DOWNTIME:nagios_command};%{DATA:nagios_hostname};%{NUMBER:nagios_start_time};%{NUMBER:nagios_end_time};%{NUMBER:nagios_fixed};%{NUMBER:nagios_trigger_id};%{NUMBER:nagios_duration};%{DATA:author};%{DATA:comment}

"""End matching line"""
  NAGIOSLOGLINE %{NAGIOSTIME} (?:%{NAGIOS_WARNING}|%{NAGIOS_CURRENT_SERVICE_STATE}|%{NAGIOS_CURRENT_HOST_STATE}|%{NAGIOS_SERVICE_NOTIFICATION}|%{NAGIOS_HOST_NOTIFICATION}|%{NAGIOS_SERVICE_ALERT}|%{NAGIOS_HOST_ALERT}|%{NAGIOS_SERVICE_FLAPPING_ALERT}|%{NAGIOS_HOST_FLAPPING_ALERT}|%{NAGIOS_SERVICE_DOWNTIME_ALERT}|%{NAGIOS_HOST_DOWNTIME_ALERT}|%{NAGIOS_PASSIVE_SERVICE_CHECK}|%{NAGIOS_PASSIVE_HOST_CHECK}|%{NAGIOS_SERVICE_EVENT_HANDLER}|%{NAGIOS_HOST_EVENT_HANDLER}|%{NAGIOS_TIMEPERIOD_TRANSITION}|%{NAGIOS_EC_LINE_DISABLE_SVC_CHECK}|%{NAGIOS_EC_LINE_ENABLE_SVC_CHECK}|%{NAGIOS_EC_LINE_DISABLE_HOST_CHECK}|%{NAGIOS_EC_LINE_ENABLE_HOST_CHECK}|%{NAGIOS_EC_LINE_PROCESS_HOST_CHECK_RESULT}|%{NAGIOS_EC_LINE_PROCESS_SERVICE_CHECK_RESULT}|%{NAGIOS_EC_LINE_SCHEDULE_HOST_DOWNTIME}|%{NAGIOS_EC_LINE_DISABLE_HOST_SVC_NOTIFICATIONS}|%{NAGIOS_EC_LINE_ENABLE_HOST_SVC_NOTIFICATIONS}|%{NAGIOS_EC_LINE_DISABLE_HOST_NOTIFICATIONS}|%{NAGIOS_EC_LINE_ENABLE_HOST_NOTIFICATIONS}|%{NAGIOS_EC_LINE_DISABLE_SVC_NOTIFICATIONS}|%{NAGIOS_EC_LINE_ENABLE_SVC_NOTIFICATIONS})
```

## Sample patterns for PostgreSQL

```
"""Default postgresql pg_log format pattern"""
POSTGRESQL %{DATESTAMP:timestamp} %{TZ} %{DATA:user_id} %{GREEDYDATA:connection_id} %{POSINT:pid}
```

## Sample patterns for Rails

```
RUUID \h{32}
"""rails controller with action"""
RCONTROLLER (? <controller>[^#]+)#(? <action>\w+)

"""this will often be the only line:"""
RAILS3HEAD (? m)Started %{WORD:verb} "%{URIPATHPARAM:request}" for %{IPORHOST:clientip} at (? <timestamp>%{YEAR}-%{MONTHNUM}-%{MONTHDAY} %{HOUR}:%{MINUTE}:%{SECOND} %{ISO8601_TIMEZONE})
"""for some a strange reason, params are stripped of {} - not sure that's a good idea."""
RPROCESSING \W*Processing by %{RCONTROLLER} as (? <format>\S+)(?:\W*Parameters: {%{DATA:params}}\W*)?
RAILS3FOOT Completed %{NUMBER:response}%{DATA} in %{NUMBER:totalms}ms %{RAILS3PROFILE}%{GREEDYDATA}
RAILS3PROFILE (?:\(Views: %{NUMBER:viewms}ms \| ActiveRecord: %{NUMBER:activerecordms}ms|\(ActiveRecord: %{NUMBER:activerecordms}ms)?

"""putting it all together"""
RAILS3 %{RAILS3HEAD}(?:%{RPROCESSING})?(? <context>(?:%{DATA}\n)*)(?:%{RAILS3FOOT})?
```

## Sample patterns for Redis

```
REDISTIMESTAMP %{MONTHDAY} %{MONTH} %{TIME}
REDISLOG \[%{POSINT:pid}\] %{REDISTIMESTAMP:timestamp} \*
```

## Sample patterns for Ruby

```
RUBY_LOGLEVEL (?:DEBUG|FATAL|ERROR|WARN|INFO)
RUBY_LOGGER [DFEWI], \[%{TIMESTAMP_ISO8601:timestamp} #%{POSINT:pid}\] *%{RUBY_LOGLEVEL:loglevel} -- +%{DATA:progname}: %{GREEDYDATA:message}
```

