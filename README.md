
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




root@krunal-vostro-3681:~# sudo netstat -tuln | grep 9187 
tcp6       0      0 :::9187                 :::*                    LISTEN     
root@krunal-vostro-3681:~# ps -l 
F S   UID   PID  PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
4 S     0 15931  5811  0  80   0 - 16462 poll_s pts/0    00:00:00 sudo
4 S     0 15932 15931  0  80   0 - 16285 wait   pts/0    00:00:00 su
4 S     0 15946 15932  0  80   0 -  5724 wait   pts/0    00:00:01 bash
4 R     0 27390 15946  0  80   0 -  7230 -      pts/0    00:00:00 ps
root@krunal-vostro-3681:~# sudo nano /etc/systemd/system/postgres_exporter.service
root@krunal-vostro-3681:~# sudo lsof -i :9187
COMMAND     PID     USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
postgres_ 26883 postgres    3u  IPv6 252903      0t0  TCP *:9187 (LISTEN)
root@krunal-vostro-3681:~# ps -lh
4     0  4787     1  20   0  16188  1968 poll_s Ss+  tty6       0:00 /sbin/agetty -o -p -- \u --noclear tty6 linux
4     0 15931  5811  20   0  65848  4488 poll_s S    pts/0      0:00 sudo su -
4     0 15932 15931  20   0  65140  3840 wait   S    pts/0      0:00 su -
4     0 15946 15932  20   0  22896  5480 wait   S    pts/0      0:01 -su
4     0 27497 15946  20   0  28920  1560 -      R+   pts/0      0:00 ps -lh
root@krunal-vostro-3681:~# 









