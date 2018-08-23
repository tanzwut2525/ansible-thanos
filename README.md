# ansible-thanos
[Thanos is a highly-available metrics system](https://github.com/improbable-eng/thanos) that touts 'unlimited' storage capacity. This role is able to configure a Thanos Sidecar (uploading metrics), Thanos Query (for querying)

## Defaults
```
thanos_sidecar_log_level: "debug"
thanos_sidecar_grpc_port: 19091
thanos_sidecar_http_port: 19191
thanos_sidecar_cluster_port: 19391
thanos_sidecar_cluster_peers_addr: ""

thanos_sidecar_prometheus_url: http://localhost:9090
thanos_sidecar_prometheus_data_dir: /etc/prometheus/data
thanos_sidecar_s3_bucket_name: ""
thanos_sidecar_s3_endpoint: ""
```

- `thanos_sidecar_log_level` defines the log level that Thanos will run. Set to 'debug' by default.
- `thanos_sidecar_grpc_port` defines the GRPC port for the thanos sidecar cluster to communicate over
- `thanos_sidecar_http_port` defines the HTTP port for the thanos sidecar cluster metrics
- `thanos_sidecar_cluster_port` defines the port which the cluster will communicate over
- `thanos_sidecar_cluster_peers_addr` defines either a static list of IPs or a DNS name that will be used find other sidecars
- `thanos_sidecar_prometheus_url` defines the URL that Thanos will use to pull metrics from
- `thanos_sidecar_prometheus_data_dir` defines the data directory that Thanos will upload Prometheus blocks from.
- `thanos_sidecar_s3_bucket_name` defines the name of the S3 bucket that Thanos will upload blocks to. This is **required**.
- `thanos_sidecar_s3_endpoint` defines the endpoint of hte S3 bucket. [This is defined in the AWS docs](https://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region) and is **required**.

