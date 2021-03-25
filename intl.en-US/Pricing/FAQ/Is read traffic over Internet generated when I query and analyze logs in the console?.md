# Is read traffic over Internet generated when I query and analyze logs in the console?

Traffic over Internet refers to the traffic that is generated when you use a third-party application to consume log data from Log Service.

If you use a third-party application, Log Service allows you to consume log data only by using related SDKs. Therefore, when you query, analyze, transform, and ship log data in the console, no traffic over Internet is generated. All these operations are performed over the internal network of Alibaba Cloud.

