# Analyze sales system logs

Bills are the core data of e-commerce companies, and the outcome of a series of marketing and promotional activities. Billing data contains a lot of valuable information. The information helps you define user profiles and create guidelines for future marketing plans. The billing data can also serve as an indicator of popularity and used to provide suggestions for subsequent stocking options.

Billing data is stored as logs in Alibaba Cloud Log Service. With the computing capacity to process hundreds of millions of log entries per second, Log Service supports high-speed queries and SQL-based statistics. This topic uses several examples to describe how to mine useful information from billing data.

The following example uses a complete bill that contains goods information \(name and price\), deal information \(final price, payment method, and discount information\), and buyer information \(membership information\):

```
__source__:  10.164.232.105      __topic__:  bonus_discount:  category:  men's clothing   commodity:  Everyday discount autumn and winter teenager velvet and thickened skinny jeans men's winter slim pants commodity_id:  443 discount:  member_discount:  member_level:  nomember_point:  memberid:  mobile:  pay_transaction_id:  060f0e0d080e0b05060307010c0f0209010e0e010c0a0605000606050b0c0400 pay_with:  alipay real_price:  52.0 suggest_price:  52.0   
```

## Perform statistical analysis

Before query and analysis, enable and configure the index feature. For more information, see [Enable and configure the indexing feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

-   View the percentage of the sales of each category of products to the total sales.

    ```
    *|select count(1) as pv ,category group by category limit 100                    
    ```

-   View the sales trends of different women's clothes.

    ```
    category: women's clothing  | select count(1) as deals , commodity 
    group by commodity order by deals  desc limit 20                    
    ```

-   View share and turnover of different payment methods.

    ```
    *  | select count(1) as deals , pay_with group by pay_with  order by deals  desc limit 20
    *  | select sum(real_price)  as total_money , pay_with group by pay_with  order by total_money  desc limit 20                 
    ```

    ![Pie-chart of payment methods](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0884688951/p32465.png)

    ![Column chart of payment methods](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0884688951/p32466.png)


