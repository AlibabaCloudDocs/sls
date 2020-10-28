# Mask sensitive data

Data masking can effectively reduce the exposure of sensitive data when you transform, ship, and use the data. Therefore, you can mitigate the risk of data leakage. This topic describes how to use functions to mask sensitive data in various scenarios.

## Background information

Data masking is commonly used to mask sensitive information such as mobile phone numbers, bank card numbers, email addresses, IP addresses, AccessKey pairs, ID numbers, URLs, order numbers, and strings. When you transform data in the Log Service console, you can use the following masking solutions: regular expression substitution \(key function regex\_replace\), Base64 transcoding \(key function base64\_encoding\), MD5 encoding \(key function md5\_encoding\), str\_translate mapping \(key function str\_translate\), Grok capture \(key function grok\). For more information, see [Regular expression functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Regular expression functions.md), [Grok function](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Grok function.md), and [Encoding and decoding functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Encoding and decoding functions.md).

## Scenario 1: Mask phone numbers

-   Solution

    To mask phone numbers in a log entry, you can use the regex\_replace function.

-   Example
    -   Raw log entry

        ```
        iphone: 13012345678
        ```

    -   Domain-specific language \(DSL\) orchestration

        ```
        e_set("sec_iphone",regex_replace(v('iphone'), r"(\d{0,3})\d{4}(\d{4})", replace=r"\1****\2"))
        ```

    -   Result

        ```
        iphone: 13012345678
        sec_iphone: 130****5678
        ```


## Scenario 2: Mask bank card information

-   Solution

    To mask the bank card information in a log entry, for example, bank card numbers, you can use the regex\_replace function.

-   Example
    -   Raw log entry

        ```
        content: bank number is 491648411333978312 and credit card number is 4916484113339780
        ```

    -   DSL orchestration

        ```
        e_set("bank_number",regex_replace(v('content'), r'([1-9]{1})(\d{11}|\d{13}|\d{14})(\d{4})', replace=r"****\3"))
        ```

    -   Result

        ```
        content: bank number is 491648411333978312 and credit card number is 4916484113339780 
        bank_number: bank number is ****978312 and credit card number is ****9780
        ```


## Scenario 3: Mask email addresses

-   Solution

    To mask the email addresses in a log entry, you can use the regex\_replace function.

-   Example
    -   Raw log entry

        ```
        content: email is twiss2345@aliyun.com
        ```

    -   DSL orchestration

        ```
        e_set("email_encrypt",regex_replace(v('content'), r'[A-Za-z\d]+([-_.][ A-Za-z\d]+)*(@([A-Za-z\d]+[-.]) +[A-Za-z\d]{2,4})', replace=r"****\2"))
        ```

    -   Result

        ```
        content: email is twiss2345@aliyun.com
        email_encrypt: email is ****@aliyun.com
        ```


## Scenario 4: Mask AccessKey pairs

-   Solution

    To mask the AccessKey pairs in a log entry, you can use the regex\_replace function.

-   Example
    -   Raw log entry

        ```
        content: ak id is rDhc9qxjhIhlBiyphP7buo5yg5h6Eq and ak key is XQr1EPtfnlZLYlQc
        ```

    -   DSL orchestration

        ```
        e_set("akid_encrypt",regex_replace(v('content'), r'([a-zA-Z0-9]{4})(([a-zA-Z0-9]{26})|([a-zA-Z0-9]{12}))', replace=r"\1****"))
        ```

    -   Result

        ```
        content: ak id is rDhc9qxjhIhlBiyphP7buo5yg5h6Eq and ak key is XQr1EPtfnlZLYlQc
        akid_encrypt: ak id is rDhc**** and ak key is XQr1****
        ```


## Scenario 5: Mask IP addresses

-   Solution

    To extract and then mask the IP addresses in a log entry, you can use the regex\_replace function and the grok function.

-   Example
    -   Raw log entry

        ```
        content: ip is 192.168.1.1
        ```

    -   DSL orchestration

        ```
        e_set("ip_encrypt",regex_replace(v('content'), grok('(%{IP})'), replace=r"****"))
        ```

    -   Result

        ```
        content: ip is 192.168.1.1
        ip_encrypt: ip is ****
        ```


## Scenario 6: Mask ID card numbers

-   Solution

    To mask the ID card numbers in a log entry, you can use the regex\_replace function and the grok function.

-   Example
    -   Raw log entry

        ```
        content: Id card is 11010519491231002X
        ```

    -   DSL orchestration

        ```
        e_set("id_encrypt",regex_replace(v('id_card'content'), grok('(%{CHINAID})'), replace=r"\1****"))
        ```

    -   Result

        ```
        content: Id card is 11010519491231002X
        id_encrypt: idcard is 110105****
        ```


## Scenario 7: Mask URLs

-   Solution

    To mask the URLs in a log entry, you can convert the URLs to plaintext and then use the Base64 encoding and decoding functions to transcode the URLs.

-   Example
    -   Raw log entry

        ```
        url: https://www.aliyun.com/sls?logstore
        ```

    -   DSL orchestration

        ```
        e_set("base64_url",base64_encoding(v("url")))
        ```

    -   Result

        ```
        url: https://www.aliyun.com/sls?logstore
        base64_url: aHR0cHM6Ly93d3cuYWxpeXVuLmNvbS9zbHM/bG9nc3RvcmU=
        ```

        **Note:** To decode the value of the `base64_url` field, you can use the `base64_decoding(v("base64_url")` function.


## Scenario 8: Mask order numbers

-   Solution

    To mask the order numbers in a log entry and prevent other users from decoding the order numbers, you can use the MD5 encoding function to encode the order numbers.

-   Example
    -   Raw log entry

        ```
        orderId: 15121412314
        ```

    -   DSL orchestration

        ```
        e_set("md5_orderId",md5_encoding(v("orderId")))
        ```

    -   Result

        ```
        orderId: 15121412314
        md5_orderId: 852751f9aa48303a5691b0d020e52a0a
        ```


## Scenario 9: Mask strings

-   Solution

    To mask the strings in a log entry, you can use the str\_translate function to develop mapping rules for the strings.

-   Example
    -   Raw log entry

        ```
        data: message level is info_
        ```

    -   DSL orchestration

        ```
        e_set("data_translate", str_translate(v("data"),"aeiou","12345"))
        ```

    -   Result

        ```
        data: message level is info
        data_translate: m2ss1g2 l2v2l 3s 3nf4
        ```


