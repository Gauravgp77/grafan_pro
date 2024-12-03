
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









--------------------------------------------------------------------------------------------------------------------------------------------------


oot@krunal-vostro-3681:~# sudo nano /etc/loki/loki-config.yml
root@krunal-vostro-3681:~# sudo mkdir -p /var/lib/loki/index /var/lib/loki/cache /var/lib/loki/chunks
root@krunal-vostro-3681:~# sudo chown -R $(whoami):$(whoami) /var/lib/loki
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
level=warn ts=2024-12-03T11:55:55.496948238Z caller=loki.go:286 msg="per-tenant timeout not configured, using default engine timeout (\"5m0s\"). This behavior will change in the next major to always use the default per-tenant timeout (\"5m\")."
level=info ts=2024-12-03T11:55:55.497767724Z caller=main.go:108 msg="Starting Loki" version="(version=2.8.2, branch=HEAD, revision=9f809eda7)"
level=info ts=2024-12-03T11:55:55.4980497Z caller=server.go:323 http=[::]:3100 grpc=[::]:9095 msg="server listening on addresses"
level=info ts=2024-12-03T11:55:55.498201989Z caller=modules.go:894 msg="Ruler storage is not configured; ruler will not be started."
level=warn ts=2024-12-03T11:55:55.499014482Z caller=cache.go:114 msg="fifocache config is deprecated. use embedded-cache instead"
level=warn ts=2024-12-03T11:55:55.499029286Z caller=experimental.go:20 msg="experimental feature in use" feature="In-memory (FIFO) cache - chunksembedded-cache"
level=info ts=2024-12-03T11:55:55.499340799Z caller=table_manager.go:134 msg="uploading tables"
level=info ts=2024-12-03T11:55:55.499316571Z caller=table_manager.go:262 msg="query readiness setup completed" duration=1.091µs distinct_users_len=0
level=info ts=2024-12-03T11:55:55.499362341Z caller=shipper.go:131 msg="starting index shipper in RW mode"
level=info ts=2024-12-03T11:55:55.499465148Z caller=shipper_index_client.go:78 msg="starting boltdb shipper in RW mode"
level=info ts=2024-12-03T11:55:55.499545204Z caller=table_manager.go:166 msg="handing over indexes to shipper"
level=info ts=2024-12-03T11:55:55.501242079Z caller=worker.go:112 msg="Starting querier worker using query-scheduler and scheduler ring for addresses"
level=error ts=2024-12-03T11:55:55.50195136Z caller=log.go:171 msg="error running loki" err="mkdir : no such file or directory\nerror initialising module: compactor\ngithub.com/grafana/dskit/modules.(*Manager).initModule\n\t/drone/src/vendor/github.com/grafana/dskit/modules/modules.go:122\ngithub.com/grafana/dskit/modules.(*Manager).InitModuleServices\n\t/drone/src/vendor/github.com/grafana/dskit/modules/modules.go:92\ngithub.com/grafana/loki/pkg/loki.(*Loki).Run\n\t/drone/src/pkg/loki/loki.go:457\nmain.main\n\t/drone/src/cmd/loki/main.go:110\nruntime.main\n\t/usr/local/go/src/runtime/proc.go:250\nruntime.goexit\n\t/usr/local/go/src/runtime/asm_amd64.s:1598"
root@krunal-vostro-3681:~# sudo systemctl restart loki
Failed to restart loki.service: Unit loki.service not found.
root@krunal-vostro-3681:~# sudo systemctl status loki
Unit loki.service could not be found.
root@krunal-vostro-3681:~# 

































