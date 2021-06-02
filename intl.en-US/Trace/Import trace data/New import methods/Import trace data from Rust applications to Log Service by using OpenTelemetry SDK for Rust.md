# Import trace data from Rust applications to Log Service by using OpenTelemetry SDK for Rust

This topic describes how to import trace data from Rust applications to Log Service by using OpenTelemetry SDK for Rust.

-   A trace instance is created. For more information, see [Create a trace instance]().
-   A Rust 1.46 or later development environments is installed.

## Procedure

1.  Add dependencies.

    ```
    [package]
    name = "test"
    version = "0.1.0"
    authors = [""]
    edition = "2018"
    
    # See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html
    
    [dependencies]
    futures = "0.3"
    lazy_static = "1.4"
    opentelemetry = { version = "0.12.0", features = ["tokio-support", "metrics", "serialize"] }
    opentelemetry-otlp = { version = "0.5.0", features = ["tonic", "metrics", "tls", "tls-roots"] }
    serde_json = "1.0"
    tokio = { version = "1.0", features = ["full"] }
    tonic="0.4.0"
    url = "2.2.0"
    ```

2.  Run the following code.

    Replace the variables in the following code with the actual values. For more information about the variables, see [Table 1](#table_afq_a87_fg1).

    ```
    use futures::stream::Stream;
    use futures::StreamExt;
    use opentelemetry::sdk::metrics::{selectors, PushController};
    use opentelemetry::trace::TraceError;
    use opentelemetry::{
        baggage::BaggageExt,
        metrics::{self, ObserverResult},
        trace::{TraceContextExt, Tracer},
        Context, Key, KeyValue,
    };
    use opentelemetry::{global, sdk::trace as sdktrace};
    use opentelemetry_otlp::ExporterConfig;
    use std::error::Error;
    use std::time::Duration;
    use tonic::{
      metadata::MetadataMap,
      transport::ClientTlsConfig,
    };
    use url::Url;
    use opentelemetry::sdk::Resource;
    use opentelemetry::sdk::export::trace::stdout;
    use opentelemetry::sdk::{
        propagation::TraceContextPropagator,
        trace::{self, Sampler},
    };
    use opentelemetry::sdk::export::trace::stdout::Uninstall;
    
    
    static ENDPOINT : &str = "https://$\{endpoint\}";
    static PROJECT : &str = "$\{project\}";
    static INSTANCE_ID : &str = "$\{instance\}";
    static AK_ID : &str = "$\{access-key-id\}";
    static AK_SECRET : &str = "$\{access-key-secret\}";
    static SERVICE_VERSION : &str = "$\{version\}";
    static SERVICE_NAME : &str = "$\{service\}";
    static HOST_NAME : &str = "$\{host\}";
    
    static SLS_PROJECT_HEADER : &str = "x-sls-otel-project";
    static SLS_INSTANCE_ID_HEADER : &str = "x-sls-otel-instance-id";
    static SLS_AK_ID_HEADER : &str = "x-sls-otel-ak-id";
    static SLS_AK_SECRET_HEADER : &str = "x-sls-otel-ak-secret";
    static SLS_SERVICE_VERSION : &str = "service.version";
    static SLS_SERVICE_NAME : &str = "service.name";
    static SLS_HOST_NAME : &str = "host.name";
    
    fn init_tracer() -> Result<(sdktrace::Tracer, opentelemetry_otlp::Uninstall), TraceError> {
        let mut metadata_map = MetadataMap::with_capacity(4);
        metadata_map.insert(SLS_PROJECT_HEADER, PROJECT.parse().unwrap());
        metadata_map.insert(SLS_INSTANCE_ID_HEADER, INSTANCE_ID.parse().unwrap());
        metadata_map.insert(SLS_AK_ID_HEADER, AK_ID.parse().unwrap());
        metadata_map.insert(SLS_AK_SECRET_HEADER, AK_SECRET.parse().unwrap());
        let endpoint = ENDPOINT;
        let endpoint = Url::parse(&endpoint).expect("endpoint is not a valid url");
        let resource = vec![KeyValue::new(SLS_SERVICE_VERSION, SERVICE_VERSION),
                            KeyValue::new(SLS_HOST_NAME, HOST_NAME), 
                            KeyValue::new(SLS_SERVICE_NAME, SERVICE_NAME)];
        opentelemetry_otlp::new_pipeline()
            .with_endpoint(endpoint.as_str())
            .with_tls_config(
                ClientTlsConfig::new().domain_name(endpoint.host_str().expect("the specified endpoint should have a valid host")))
            .with_metadata(metadata_map)
            .with_trace_config(
                opentelemetry::sdk::trace::config()
                    .with_resource(Resource::new(resource)),
            )
            .install()
    }
    
    // Skip first immediate tick from tokio, not needed for async_std.
    fn delayed_interval(duration: Duration) -> impl Stream<Item = tokio::time::Instant> {
        opentelemetry::util::tokio_interval_stream(duration).skip(1)
    }
    
    fn init_meter() -> metrics::Result<PushController> {
    
        let mut metadata_map = MetadataMap::with_capacity(4);
        metadata_map.insert(SLS_PROJECT_HEADER, PROJECT.parse().unwrap());
        metadata_map.insert(SLS_INSTANCE_ID_HEADER, INSTANCE_ID.parse().unwrap());
        metadata_map.insert(SLS_AK_ID_HEADER, AK_ID.parse().unwrap());
        metadata_map.insert(SLS_AK_SECRET_HEADER, AK_SECRET.parse().unwrap());
        let endpoint = ENDPOINT;
        let endpoint = Url::parse(&endpoint).expect("endpoint is not a valid url");
        let export_config = ExporterConfig {
            endpoint: endpoint.as_str().to_string(),
            tls_config : Some(ClientTlsConfig::new().domain_name(
                    endpoint
                        .host_str()
                        .expect("the specified endpoint should have a valid host"))),
            metadata : Some(metadata_map),
            ..ExporterConfig::default()
        };
        let resource = vec![KeyValue::new(SLS_SERVICE_VERSION, SERVICE_VERSION),
                            KeyValue::new(SLS_HOST_NAME, HOST_NAME), 
                            KeyValue::new(SLS_SERVICE_NAME, SERVICE_NAME)];
        opentelemetry_otlp::new_metrics_pipeline(tokio::spawn, delayed_interval)
            .with_export_config(export_config)
            .with_resource(resource)
            .with_aggregator_selector(selectors::simple::Selector::Exact)
            .build()
    }
    
    fn init_tracer_stdout() -> (opentelemetry::sdk::trace::Tracer, Uninstall) {
        global::set_text_map_propagator(TraceContextPropagator::new());
        // Install stdout exporter pipeline to be able to retrieve the collected spans.
        // For the demonstration, use `Sampler::AlwaysOn` sampler to sample all traces. In a production
        // application, use `Sampler::ParentBased` or `Sampler::TraceIdRatioBased` with a desired ratio.
        stdout::new_pipeline()
            .with_trace_config(trace::config().with_default_sampler(Sampler::AlwaysOn))
            .install()
    }
    
    const FOO_KEY: Key = Key::from_static_str("ex.com/foo");
    const BAR_KEY: Key = Key::from_static_str("ex.com/bar");
    const LEMONS_KEY: Key = Key::from_static_str("ex.com/lemons");
    const ANOTHER_KEY: Key = Key::from_static_str("ex.com/another");
    
    lazy_static::lazy_static! {
        static ref COMMON_LABELS: [KeyValue; 4] = [
            LEMONS_KEY.i64(10),
            KeyValue::new("A", "1"),
            KeyValue::new("B", "2"),
            KeyValue::new("C", "3"),
        ];
    }
    
    #[tokio::main]
    async fn main() -> Result<(), Box<dyn Error + Send + Sync + 'static>> {
        let _tracer : (opentelemetry::sdk::trace::Tracer, Uninstall);
        let _guard : Result<(sdktrace::Tracer, opentelemetry_otlp::Uninstall), TraceError>;
        let _started : metrics::Result<PushController>;
        if ENDPOINT == "stdout"{
            println!("init_tracer_stdout");
            _tracer = init_tracer_stdout();
        }else {
            let _guard = init_tracer()?;
            let _started = init_meter()?;
        }
    
        let tracer = global::tracer("ex.com/basic");
        let meter = global::meter("ex.com/basic");
    
        let one_metric_callback = |res: ObserverResult<f64>| res.observe(1.0, COMMON_LABELS.as_ref());
        let _ = meter
            .f64_value_observer("ex.com.one", one_metric_callback)
            .with_description("A ValueObserver set to 1.0")
            .init();
    
        let value_recorder_two = meter.f64_value_recorder("ex.com.two").init();
    
        let another_recorder = meter.f64_value_recorder("ex.com.two").init();
        another_recorder.record(5.5, COMMON_LABELS.as_ref());
    
        let _baggage =
            Context::current_with_baggage(vec![FOO_KEY.string("foo1"), BAR_KEY.string("bar1")])
                .attach();
    
        let value_recorder = value_recorder_two.bind(COMMON_LABELS.as_ref());
    
        tracer.in_span("operation", |cx| {
            let span = cx.span();
            span.add_event(
                "Nice operation!".to_string(),
                vec![Key::new("bogons").i64(100)],
            );
            span.set_attribute(ANOTHER_KEY.string("yes"));
    
            meter.record_batch_with_context(
                // Note: call-site variables added as context Entries:
                &Context::current_with_baggage(vec![ANOTHER_KEY.string("xyz")]),
                COMMON_LABELS.as_ref(),
                vec![value_recorder_two.measurement(2.0)],
            );
    
            tracer.in_span("Sub operation...", |cx| {
                let span = cx.span();
                span.set_attribute(LEMONS_KEY.string("five"));
    
                span.add_event("Sub span event".to_string(), vec![]);
    
                value_recorder.record(1.3);
            });
        });
    
        // wait for 1 minutes so that we could see metrics being pushed via OTLP every 10 seconds.
        tokio::time::sleep(Duration::from_secs(60)).await;
    
    
        Ok(())
    }
    ```

    |Variable|Description|Example|
    |--------|-----------|-------|
    |$\{service\}|The name of the service. Enter a name based on the actual scenario.|payment|
    |$\{version\}|The version of the service. We recommend that you specify the version in the va.b.c format.|v0.1.2|
    |$\{host\}|The hostname of the server.|localhost|
    |$\{endpoint\}|The endpoint. The format is $\{project\}.$\{region-endpoint\}, where:    -   $\{project\}: the name of the Log Service project.
    -   $\{region-endpoint\}: the endpoint of the project. You can access Log Service by using an endpoint of the Internet, the classic network, or a virtual private cloud \(VPC\). For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).
    -   Port: the port number, which is set to 10010.
|test-project.cn-hangzhou.log.aliyuncs.com:10010|
    |$\{project\}|The name of the Log Service project.|test-project|
    |$\{instance\}|The name of the trace instance.|test-traces|
    |$\{access-key-id\}|The AccessKey ID of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a Resource Access Management \(RAM\) user who has only the write permissions on the Log Service project. An AccessKey pair consists of an AccessKey ID and an AccessKey secret. For information about how to grant the write permissions on a specific project to a RAM user, see [Use custom policies to grant permissions to a RAM user](/intl.en-US/Developer Guide/Access control RAM/Use custom policies to grant permissions to a RAM user.md). For information about how to obtain an AccessKey pair, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).

|None|
    |$\{access-key-secret\}|The AccessKey secret of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a RAM user who has only the write permissions on the Log Service project.

|None|


-   [View the details of a trace instance]()
-   [Query and analyze trace data]()

