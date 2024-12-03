
global:
  scrape_interval: 15s # Default scrape interval.
  evaluation_interval: 15s # Default evaluation interval.

alerting:
  alertmanagers:
    - static_configs:
        - targets: ['alertmanager:9093'] # Update with your Alertmanager's address if needed.

scrape_configs:
  # Scrape Prometheus itself
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  # Scrape Node Exporter
  - job_name: 'node_exporter'
    static_configs:
      - targets: ['192.168.6.108:9100'] # Replace with your Node Exporter IP and port.









  
[Unit]
Description=PostgreSQL Exporter
After=network.target

[Service]
User =postgres
Group=postgres
Type=simple
Environment="DATA_SOURCE_NAME=postgres://postgres:postgres@192.168.6.208:5432/BaseMap_Multi?sslmode=disable"
ExecStart=/usr/local/bin/postgres_exporter --web.listen-address="0.0.0.0:9187" --web.telemetry-path=/metrics

[Install]
WantedBy=multi-user.target









root@krunal-vostro-3681:~# loki -config.file=/etc/loki/loki-config.yml
mkdir : no such file or directory
error initialising module: compactor
github.com/grafana/dskit/modules.(*Manager).initModule
	/drone/src/vendor/github.com/grafana/dskit/modules/modules.go:122
github.com/grafana/dskit/modules.(*Manager).InitModuleServices
	/drone/src/vendor/github.com/grafana/dskit/modules/modules.go:92
github.com/grafana/loki/pkg/loki.(*Loki).Run
	/drone/src/pkg/loki/loki.go:457
main.main
	/drone/src/cmd/loki/main.go:110
runtime.main
	/usr/local/go/src/runtime/proc.go:250
runtime.goexit
	/usr/local/go/src/runtime/asm_amd64.s:1598
level=warn ts=2024-12-03T10:36:06.446272186Z caller=loki.go:286 msg="per-tenant timeout not configured, using default engine timeout (\"5m0s\"). This behavior will change in the next major to always use the default per-tenant timeout (\"5m\")."
level=info ts=2024-12-03T10:36:06.447567299Z caller=main.go:108 msg="Starting Loki" version="(version=2.8.2, branch=HEAD, revision=9f809eda7)"
level=info ts=2024-12-03T10:36:06.447634097Z caller=modules.go:894 msg="Ruler storage is not configured; ruler will not be started."
level=info ts=2024-12-03T10:36:06.448263367Z caller=server.go:323 http=[::]:3100 grpc=[::]:9095 msg="server listening on addresses"
level=warn ts=2024-12-03T10:36:06.448743764Z caller=cache.go:114 msg="fifocache config is deprecated. use embedded-cache instead"
level=warn ts=2024-12-03T10:36:06.448759713Z caller=experimental.go:20 msg="experimental feature in use" feature="In-memory (FIFO) cache - chunksembedded-cache"
level=info ts=2024-12-03T10:36:06.448962609Z caller=table_manager.go:262 msg="query readiness setup completed" duration=827ns distinct_users_len=0
level=info ts=2024-12-03T10:36:06.449018691Z caller=shipper.go:131 msg="starting index shipper in RW mode"
level=info ts=2024-12-03T10:36:06.449063134Z caller=table_manager.go:134 msg="uploading tables"
level=info ts=2024-12-03T10:36:06.449106323Z caller=shipper_index_client.go:78 msg="starting boltdb shipper in RW mode"
level=info ts=2024-12-03T10:36:06.449164234Z caller=table_manager.go:166 msg="handing over indexes to shipper"
level=error ts=2024-12-03T10:36:06.450933065Z icaller=log.go:171 msg="error running loki" err="mkdir : no such file or directory\nerror initialising module: compactor\ngithub.com/grafana/dskit/modules.(*Manager).initModule\n\t/drone/src/vendor/github.com/grafana/dskit/modules/modules.go:122\ngithub.com/grafana/dskit/modules.(*Manager).InitModuleServices\n\t/drone/src/vendor/github.com/grafana/dskit/modules/modules.go:92\ngithub.com/grafana/loki/pkg/loki.(*Loki).Run\n\t/drone/src/pkg/loki/loki.go:457\nmain.main\n\t/drone/src/cmd/loki/main.go:110\nruntime.main\n\t/usr/local/go/src/runtime/proc.go:250\nruntime.goexit\n\t/usr/local/go/src/runtime/asm_amd64.s:1598"














storage_config:
  boltdb_shipper:
    active_index_directory: /var/lib/loki/index
    cache_location: /var/lib/loki/cache
    shared_store: filesystem
  filesystem:
    directory: /var/lib/loki/chunks
where is add this
ChatGPT said:
ChatGPT
You should add the storage_config section to your Loki configuration file, typically located at /etc/loki/loki-config.yml or similar, depending on your setup. Here's an example of how the file might look with your storage_config section included:

yaml
Copy code
auth_enabled: false

server:
  http_listen_port: 3100

ingester:
  lifecycler:
    ring:
      kvstore:
        store: inmemory
      replication_factor: 1
  chunk_idle_period: 5m
  chunk_retain_period: 30s

schema_config:
  configs:
    - from: 2023-01-01
      store: boltdb-shipper
      object_store: filesystem
      schema: v11
      index:
        prefix: index_
        period: 24h

storage_config:
  boltdb_shipper:
    active_index_directory: /var/lib/loki/index
    cache_location: /var/lib/loki/cache
    shared_store: filesystem
  filesystem:
    directory: /var/lib/loki/chunks

limits_config:
  enforce_metric_name: false
Steps to Update and Apply:
Edit the File: Open the configuration file with a text editor, such as nano:

bash
Copy code
sudo nano /etc/loki/loki-config.yml
Add the storage_config Section: Insert the storage_config section as shown above.

Create Necessary Directories: Ensure the directories specified in the configuration exist and have the correct permissions:

bash
Copy code
sudo mkdir -p /var/lib/loki/index /var/lib/loki/cache /var/lib/loki/chunks
sudo chown -R loki:loki /var/lib/loki
Restart Loki: Restart Loki to apply the changes:

bash
Copy code
sudo systemctl restart loki
Verify Loki is Running: Check Loki's status to ensure there are no errors:

bash
Copy code
sudo systemctl status loki
Let me know if you encounter any issues!



































