# ansible-thanos

![image](https://user-images.githubusercontent.com/1196058/44577196-5cda4780-a788-11e8-956c-b045aa5f6ee5.png)

[Thanos is a highly-available metrics system](https://github.com/thanos-io/thanos) that touts 'unlimited' storage capacity. This role is able to configure the following components on **Debian systems, targetting AWS as a backend**:

- Thanos Sidecar (uploading metrics)
- Thanos Query (for querying)
- Thanos Store (for S3/etc)
- Thanos Compact (for compaction)

The role also has the requirement that you **create a debian package** for [Thanos](https://github.com/thanos-io/thanos). This is not going to change and is not in the scope of this repo. If you don't have a system for this, we recommend [FPM](https://github.com/jordansissel/fpm/) for getting something simple up and running quickly.

In future we will implement the following Thanos components:

- Downsample: which actually downsamples metrics for compaction
- Rules: for setting up alerting/recording rules.


## Defaults
```
---

thanos_sidecar_log_level: "debug"
thanos_sidecar_grpc_port: 19091
thanos_sidecar_http_port: 19191
thanos_sidecar_cluster_port: 19391
thanos_sidecar_config_file: "/etc/thanos-sidecar.yaml"
thanos_sidecar_cluster_disabled: False
thanos_sidecar_gossip_flags_enabled: True
thanos_sidecar_extra_cmd_opts: {}

thanos_compactor_log_level: "debug"
thanos_compactor_http_port: 19192
thanos_compactor_data_dir: ""
thanos_compactor_config_file: "/etc/thanos-compactor.yaml"
thanos_compactor_extra_cmd_opts: {}

thanos_downsample_config_file: "/etc/thanos-downsample.yaml"
thanos_downsample_data_dir: ""
thanos_downsample_log_level: "info"
thanos_downsample_extra_cmd_opts: {}

# Setting the following values to '0d' will disable them
# How long to retain raw samples in the bucket:
thanos_compactor_retention_raw: "0d"
# How long to retain samples of resolution 1 (5m)
thanos_compactor_retention_5m: "0d"
# How long to retain samples of resolution 2 (1h)
thanos_compactor_retention_1h: "0d"

thanos_query_log_level: "debug"
thanos_query_grpc_port: 19091
thanos_query_http_port: 19193
thanos_query_cluster_port: 19391
thanos_query_cluster_disabled: False
thanos_query_gossip_flags_enabled: True

thanos_query_stores: []
thanos_query_extra_cmd_opts: {}

thanos_store_log_level: "debug"
thanos_store_grpc_port: 19091
thanos_store_http_port: 19194
thanos_store_cluster_port: 19391
thanos_store_index_cache_size: "250MB"
thanos_store_chunk_pool_size: "2GB"
thanos_store_config_file: "/etc/thanos-store.yaml"
thanos_store_cluster_disabled: False
thanos_store_gossip_flags_enabled: True
# 0 == off
thanos_store_series_sample_limit: 0
thanos_store_series_max_concurrency: 20
thanos_store_extra_cmd_opts: {}

thanos_cluster_peers_addr: ""
thanos_prometheus_url: http://localhost:9090
thanos_prometheus_data_dir: /etc/prometheus/data
thanos_s3_bucket_name: ""
thanos_s3_endpoint: ""
thanos_default_retention_period: "90d"
thanos_package_version: "latest"

thanos_sidecar_enabled: True
thanos_query_enabled: False
thanos_compactor_enabled: False
thanos_store_enabled: False
thanos_downsample_enabled: False
```

## Variable Docs

| Variable | Required? | Description |
|-----------------------------------|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `thanos_sidecar_enabled` | `false` | Decides if the role should install thanos sidecar. Disabled by default |
| `thanos_sidecar_log_level` | `false` | The log level that Thanos will run. Set to `debug` by default. |
| `thanos_sidecar_grpc_port` | `false` | The GRPC port for the thanos sidecar cluster to communicate over. |
| `thanos_sidecar_http_port` | `false` | The HTTP port for the thanos sidecar cluster metrics. |
| `thanos_sidecar_cluster_port` | `false` | The port which the cluster will communicate over. |
| `thanos_sidecar_config_file` | `false` | Where the bucket configuration file will sit. Defaults to `/etc/thanos-sidecar.yaml`. |
| `thanos_sidecar_tracing_config_file` | `false` | Where the tracing configuration will sit. If it doesn't exist, tracing won't be enabled. |
| `thanos_sidecar_tracing_config` | `false` | Tracing conf as string  |
| `thanos_sidecar_cluster_disabled` | `false` | Disables Gossip cluster. `thanos_query_cluster_port` and `thanos_cluster_peers_addr` will be ignored if set to `True`. |
| `thanos_sidecar_gossip_flags_enabled` | `false` | Allows to disable setting gossip related command line flags, as newer versions of Thanos don't support them. |
| `thanos_sidecar_extra_cmd_opts` | `false` | Extra command line options key value map. This will just be appended when running Thanos binary. |
| `thanos_compactor_enabled` | `false` | Whether the role should install thanos compact. Disabled by default. |
| `thanos_compactor_log_level` | `false` | The log level that Thanos will run. Set to 'debug' by default. |
| `thanos_compactor_http_port` | `false` | The HTTP port for the thanos compactor cluster metrics. |
| `thanos_compactor_data_dir` | `true` | The data directory where Thanos compactor will store temporary files. Is necessary when setting up Thanos Compactor, but not so for anything else. |
| `thanos_compactor_config_file` | `false` | Where the bucket configuration file will sit. Defaults to `/etc/thanos-compactor.yaml`. |
| `thanos_compactor_tracing_config_file` | `false` | Where the tracing configuration will sit. If it doesn't exist, tracing won't be enabled. |
| `thanos_compactor_tracing_config` | `false` | Tracing conf as string  |
| `thanos_compactor_retention_raw` | `false` | How long Thanos will keep raw Prometheus metrics. Defaults to `0d`, which means it is disabled. |
| `thanos_compactor_retention_5m` | `false` | How long Thanos will keep Prometheus metrics with a resolution of 5m. Defaults to `0d`, which means it is disabled. |
| `thanos_compactor_retention_1d` | `false` | How long Thanos will keep Prometheus metrics with a resolution of 1h. Defaults to `0d`, which means it is disabled. |
| `thanos_compactor_extra_cmd_opts` | `false` | Extra command line options key value map. This will just be appended while running Thanos binary |
| `thanos_downsample_enabled` | `false` | Decides if the role should install thanos downsample. Disabled by default |
| `thanos_downsample_config_file` | `false` | Where the downsample configuration file should live. |
| `thanos_downsample_tracing_config_file` | `false` | Where the tracing configuration will sit. If it doesn't exist, tracing won't be enabled. |
| `thanos_downsample_tracing_config` | `false` | Tracing conf as string  |
| `thanos_downsample_data_dir` | `false` | The data directory that Downsample will store temporary data in. |
| `thanos_downsample_log_level` | `false` | The logging level thanos downsample will use. Defaults to 'info' |
| `thanos_downsample_extra_cmd_opts` | `false` | Extra command line options key value map. This will just be appended when running Thanos binary |
| `thanos_query_enabled` | `false` | Whether the role should install thanos query. Disabled by default. |
| `thanos_query_tracing_config_file` | `false` | Where the tracing configuration will sit. If it doesn't exist, tracing won't be enabled. |
| `thanos_query_tracing_config` | `false` | Tracing conf as string  |
| `thanos_query_log_level` | `false` | The log level that Thanos will run. Set to 'debug' by default. |
| `thanos_query_grpc_port` | `false` | The GRPC port for the thanos query cluster to communicate over |
| `thanos_query_http_port` | `false` | The HTTP port for the thanos query cluster metrics |
| `thanos_query_cluster_port` | `false` | The port which the cluster will communicate over |
| `thanos_query_cluster_disabled` | `false` | Disables Gossip cluster. `thanos_query_cluster_port` and `thanos_cluster_peers_addr` will be ignored if set to `True`. |
| `thanos_query_gossip_flags_enabled` | `false` | Allows to disable setting gossip related command line flags, as newer versions of Thanos don't support them. |
| `thanos_query_stores` | `false` | List of Thanos store API endpoints used by the Thanos Query component |
| `thanos_query_extra_cmd_opts` | `false` | Extra command line options key value map. This will just be appended when running Thanos binary |
| `thanos_store_enabled` | `false` | Whether the role should install thanos store. Disabled by default. |
| `thanos_store_log_level` | `false` | The log level that Thanos will run. Set to 'debug' by default. |
| `thanos_store_grpc_port` | `false` | The GRPC port for the thanos sidecar cluster to communicate over |
| `thanos_store_http_port` | `false` | The HTTP port for the thanos sidecar cluster metrics |
| `thanos_store_cluster_port` | `false` | The port which the cluster will communicate over |
| `thanos_store_index_cache_size` | `false` | The maximum size of items held in the index cache |
| `thanos_store_chunk_pool_size` | `false` | The maximum size of concurrently allocatable bytes for chunks |
| `thanos_store_config_file` | `false` | Where the bucket configuration file will sit. Defaults to `/etc/thanos-store.yaml`. |
| `thanos_store_tracing_config_file` | `false` | Where the tracing configuration will sit. If it doesn't exist, tracing won't be enabled. |
| `thanos_store_tracing_config` | `false` | Tracing conf as string  |
| `thanos_store_cluster_disabled` | `false` | Disables Gossip cluster. `thanos_query_cluster_port` and `thanos_cluster_peers_addr` will be ignored if set to `True`. |
| `thanos_store_gossip_flags_enabled` | `false` | Allows to disable setting gossip related command line flags, as newer versions of Thanos don't support them. |
| `thanos_store_series_sample_limit` | `false` | The limit of how many series samples will be fetched from the store. Defaults to `0`, which means 'disabled'. |
| `thanos_store_series_max_concurrency` | `false` | The max amount of concurrency for the store. Defaults to `20`. |
| `thanos_store_extra_cmd_opts` | `false` | Extra command line options key value map. This will just be appended when running Thanos binary |
| `thanos_cluster_peers_addr` | `false` | Either a static list of IPs or a DNS name that will be used find other sidecars |
| `thanos_prometheus_url` | `false` | The URL that Thanos will use to pull metrics from |
| `thanos_prometheus_data_dir` | `false` | The data directory that Thanos will upload Prometheus blocks from. |
| `thanos_s3_bucket_name` | `true` | The name of the S3 bucket that Thanos will upload blocks to. This is **required**. |
| `thanos_s3_endpoint` | `true` | The endpoint of the S3 bucket. [This is defined in the AWS docs](https://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region) and is **required**. |
| `thanos_default_retention_period` | `false` | The retention period of objects in the bucket. |
| `thanos_package_version` | `false` | Version of Thanos APT package. Special value 'latest' will install latest version from available repo. |

