8
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

If you're experiencing issues with Loki's configuration and encountering problems while accessing it via the browser (e.g., http://10.5.56.67:3100/loki doesn't load), follow these steps to troubleshoot and resolve the issue:

1. Verify Loki Service
Check if the Loki service is running correctly:
bash
Copy code
systemctl status loki
If it's inactive or has errors, check the logs:
bash
Copy code
journalctl -u loki.service
2. Verify Network and Port Accessibility
Ensure the port 3100 is open and listening:
bash
Copy code
netstat -tulnp | grep 3100
Verify Loki is bound to the correct IP address. Look for:
Copy code
tcp 0 0 10.5.56.67:3100 ...
If not, check the Loki configuration file for:
yaml
Copy code
server:
  http_listen_port: 3100
  http_listen_address: 0.0.0.0  # Ensure it's listening on all interfaces or the specific IP
3. Check Firewall Rules
Ensure the firewall allows traffic to port 3100:
bash
Copy code
firewall-cmd --zone=public --list-ports
If port 3100 is missing, add it:
bash
Copy code
firewall-cmd --zone=public --add-port=3100/tcp --permanent
firewall-cmd --reload
4. Verify Browser Accessibility
Test connectivity from your machine:
bash
Copy code
curl http://10.5.56.67:3100/ready
If this returns HTTP/1.1 200 OK, Loki is running properly.
If not, there might be an issue with Loki itself or network connectivity.
5. Check Loki Logs
Check for errors in Loki's logs:
bash
Copy code
sudo tail -f /var/log/loki.log
6. Test Access Locally
If you're accessing Loki from a remote machine, test it locally first:
bash
Copy code
curl http://localhost:3100/ready
If it works locally but not remotely, the issue might be in the firewall or network settings.
7. Validate Configuration
Double-check the loki-local-config.yaml file for errors, especially in these sections:
server: Ensure correct IP and port.
storage_config: Verify that directories like /loki/chunks exist and have the right permissions:
bash
Copy code
sudo chown -R loki_user:loki_group /loki
8. Restart Loki
Restart the Loki service after any configuration changes:
bash
Copy code
systemctl restart loki
systemctl status loki
9. Cross-check Promtail
Ensure Promtail is sending logs to Loki:
bash
Copy code
systemctl status promtail
Check Promtail logs for issues:
bash
Copy code
journalctl -u promtail
10. Debug with Logs
If the issue persists, share the Loki logs for further troubleshooting:

bash
Copy code
journalctl -u loki | tail -n 50
Once you've verified all the above, your setup should work as expected. Let me know if you encounter further issues!






You said:
this  is  documention ip i  need to puting  my  live database ip in my loki configration  where is puting this  ip  192.168.6.208 this  my database ip  where is   puting  in  loki configration  you  can guide me 
ChatGPT said:
ChatGPT
To integrate your live database (192.168.6.208) with Loki for log collection, you should update the Promtail configuration file (config-promtail.yml) to specify the location of the database logs. Loki itself does not directly interact with the database; instead, Promtail collects and pushes logs to Loki.

Here's where and how to configure it:

Update Promtail Configuration
Locate the Promtail Configuration File
Open the config-promtail.yml file:

bash
Copy code
nano /opt/promtail/config-promtail.yml
Specify the Database Logs Path
Add or modify the scrape_configs section to include the path to your database logs. Update the IP and log file path accordingly:

yaml
Copy code
scrape_configs:
  - job_name: database_logs
    static_configs:
      - targets:
          - 192.168.6.208
        labels:
          job: database
          __path__: /path/to/database/logs/*.log
Replace /path/to/database/logs/*.log with the actual path to your database's log files on 192.168.6.208. For example:
bash
Copy code
/var/log/postgresql/*.log
Save the Configuration
Save and exit the file.

Restart Promtail
Restart Promtail to apply the new configuration:

bash
Copy code
systemctl restart promtail
systemctl status promtail
Ensure Network Connectivity
Promtail must have access to the database machine (192.168.6.208). Check the following:

Ensure that 192.168.6.208 is reachable:
bash
Copy code
ping 192.168.6.208
Verify the log directory on the database server is shared and accessible. If Promtail runs on a different machine, you may need to:
Use a network file share (e.g., NFS, SMB).
Install Promtail directly on the database machine.
Configure Loki in Grafana
In Grafana, add the Loki data source with the URL:

arduino
Copy code
http://<Loki-IP>:3100
For example:

arduino
Copy code
http://192.168.6.208:3100
Final Notes
Loki itself does not require the database IP, but Promtail needs to know the database logs' location.
If your database logs are centralized (stored remotely or accessible via a shared folder), ensure the path is correctly set in the Promtail configuration.
Restart both Loki and Promtail after making changes:
bash
Copy code
systemctl restart loki
systemctl restart promtail
Let me know if you need further assistance!






You said:

mena  this  configration  where puting  in username password in  postgresql passworld  ip in loki yml file loki server and  promyail yml file  guide me setp by setp
ChatGPT said:
ChatGPT
I cannot directly extract text or understand the exact details of your uploaded configuration screenshot. However, I can guide you step-by-step on how to integrate PostgreSQL logs into Promtail and adjust your Loki and Promtail YAML files:

1. Promtail Configuration for PostgreSQL Logs
You need to modify your config-promtail.yml file to include:

Your PostgreSQL IP address (192.168.6.208).
The path to your PostgreSQL log files.
Credentials for accessing PostgreSQL logs (if required).
Steps:
Open config-promtail.yml:

bash
Copy code
nano /opt/promtail/config-promtail.yml
Edit the Configuration File: Add a scrape_configs block for PostgreSQL logs. Update the IP, file path, and labels accordingly:

yaml
Copy code
scrape_configs:
  - job_name: postgresql_logs
    static_configs:
      - targets:
          - 192.168.6.208
        labels:
          job: postgresql
          host: 192.168.6.208
          __path__: /var/log/postgresql/*.log
Replace /var/log/postgresql/*.log with the actual path to your PostgreSQL log files.
Save and Exit: Save the file and exit the editor.

Restart Promtail: Apply the updated configuration:

bash
Copy code
systemctl restart promtail
systemctl status promtail
2. Adding PostgreSQL Credentials (If Required)
If accessing the PostgreSQL server requires credentials, add them to the Promtail configuration under basic_auth in the clients section:

yaml
Copy code
clients:
  - url: 'http://<Loki-IP>:3100/loki/api/v1/push'
    basic_auth:
      username: <your_postgresql_username>
      password: <your_postgresql_password>
Replace <Loki-IP> with your Loki server IP and provide the correct PostgreSQL username and password.

3. Loki Configuration
Loki itself does not directly interface with PostgreSQL logs; Promtail pushes logs to Loki. However, ensure Loki is set up to receive logs from Promtail:

Open Loki Config File:

bash
Copy code
nano /opt/loki/loki-local-config.yaml
Check the Port (Default: 3100): Verify Loki is listening on port 3100. Look for this block:

yaml
Copy code
server:
  http_listen_port: 3100
Save and Restart Loki:

bash
Copy code
systemctl restart loki
systemctl status loki
4. Configure Grafana
Log in to Grafana.
Add Loki as a Data Source:
URL: http://<Loki-IP>:3100
Basic Auth: If enabled, use the same credentials configured in Loki.
Query logs with labels like job="postgresql".
Let me know if you need further clarification!












am # Commend this also not run stake 
ChatGPT said:
ChatGPT
If commenting out the unsupported directives still causes the grafana-server service to fail, you may need to debug further and ensure other configurations are correct. Here's how to proceed step-by-step:

1. Verify Service File Syntax
Make sure there are no syntax errors in the service file. If you've commented out lines like this:

text
Copy code
# ProtectKernelLogs=true
# ProtectClock=true
# ProtectProc=invisible
The file should save properly.

2. Reload systemd
After modifying the service file, always reload the systemd configuration:

bash
Copy code
sudo systemctl daemon-reload
3. Check Logs for Errors
Restart the service and immediately check the logs for errors:

bash
Copy code
sudo systemctl restart grafana-server
sudo journalctl -u grafana-server -xe
Look for messages indicating why the service fails.

4. Debug Potential Configuration Issues
If grafana-server still doesn’t run, verify these configurations:

a) Correct Binary Path
Ensure that the ExecStart path is correct in the service file:

text
Copy code
ExecStart=/usr/share/grafana/bin/grafana-server
Test running the command directly:

bash
Copy code
/usr/share/grafana/bin/grafana-server --config=/etc/grafana/grafana.ini
If it fails, ensure Grafana is installed properly and the binary exists.

b) File Permissions
Ensure that the following directories exist and have the correct permissions:

bash
Copy code
sudo chmod 0750 /usr/share/grafana
sudo chown -R grafana:grafana /usr/share/grafana
sudo chown -R grafana:grafana /var/lib/grafana /etc/grafana
c) Environment File
Check if /etc/default/grafana-server exists and has correct variables. Recreate it if missing:

bash
Copy code
sudo nano /etc/default/grafana-server
Add:

text
Copy code
CONF_FILE=/etc/grafana/grafana.ini
PID_FILE_DIR=/var/run/grafana
LOG_DIR=/var/log/grafana
DATA_DIR=/var/lib/grafana
PLUGINS_DIR=/var/lib/grafana/plugins
5. Reinstall Grafana (Last Resort)
If none of the above works, consider reinstalling Grafana to ensure a clean configuration:

bash
Copy code
sudo apt purge grafana
sudo apt install grafana
6. Verify Network and Port
Check if the default Grafana port (3000) is available:

bash
Copy code
sudo netstat -tuln | grep 3000
Kill any process using the port if necessary.

Let me know if you're still facing issues after trying these steps.






You said:

ChatGPT said:
ChatGPT
The errors in the image highlight multiple issues that need to be addressed:

Key Observations:
Unknown Protect* Values:

Systemd is rejecting the Protect* options (ProtectKernelLogs, ProtectClock, ProtectProc, etc.) as they are either invalid or not supported in your version of systemd.
Solution:

Comment out these directives in the grafana-server.service file:
ini
Copy code
# ProtectKernelLogs=true
# ProtectClock=true
# ProtectProc=invisible
Save the file and reload systemd:
bash
Copy code
sudo systemctl daemon-reload
Deprecation Warning:

The warning suggests that the standalone grafana-server binary is deprecated.
Solution:

Use the systemd service to start Grafana instead of running the binary manually:
bash
Copy code
sudo systemctl start grafana-server
"Could not find config defaults" Error:

The grafana-server binary is unable to locate its configuration file or homepath.
Solution:

Explicitly set the homepath parameter when running the binary:
bash
Copy code
/usr/share/grafana/bin/grafana-server --homepath=/usr/share/grafana --config=/etc/grafana/grafana.ini
Alternatively, ensure the ExecStart directive in the service file specifies --homepath:
ini
Copy code
ExecStart=/usr/share/grafana/bin/grafana-server \
  --homepath=/usr/share/grafana \
  --config=/etc/grafana/grafana.ini
Running as Root:

The message indicates the server is running with elevated privileges, which is not recommended.
Solution:

Ensure that the grafana-server.service file specifies the correct user:

ini
Copy code
User=grafana
Group=grafana
Verify the user exists:

bash
Copy code
id grafana
If the user doesn't exist, create it:

bash
Copy code
sudo useradd -r -s /sbin/nologin grafana
sudo chown -R grafana:grafana /usr/share/grafana /etc/grafana /var/lib/grafana
Steps to Fix:
Edit the service file to address the issues mentioned:

bash
Copy code
sudo nano /lib/systemd/system/grafana-server.service
Update as necessary.

Reload and restart the service:

bash
Copy code
sudo systemctl daemon-reload
sudo systemctl restart grafana-server
Check the status:

bash
Copy code
sudo systemctl status grafana-server
Inspect logs if further issues persist:

bash
Copy code
sudo journalctl -u grafana-server -xe
Let me know how it goes!





-------------------------------------*****************************************************************88



grafana-server.service - Grafana instance
   Loaded: loaded (/usr/lib/systemd/system/grafana-server.service; enabled; vendor preset: enabled)
  Drop-In: /etc/systemd/system/grafana-server.service.d
           └─override.conf
   Active: inactive (dead) (Result: exit-code) since Wed 2024-12-04 14:08:05 IST; 18s ago
     Docs: http://docs.grafana.org
  Process: 20310 ExecStart=/usr/share/grafana/bin/grafana server --config=${CONF_FILE} --pidfile=${PID_FILE_DIR}/grafana-server.pid --packaging=deb cfg:default.paths.logs=${LOG_DIR} cfg:default.paths.data
 Main PID: 20310 (code=exited, status=1/FAILURE)

Dec 04 14:08:05 krunal-vostro-3681 systemd[1]: grafana-server.service: Service hold-off time over, scheduling restart.
Dec 04 14:08:05 krunal-vostro-3681 systemd[1]: grafana-server.service: Scheduled restart job, restart counter is at 4.
Dec 04 14:08:05 krunal-vostro-3681 systemd[1]: Stopped Grafana instance.
lines 1-12/12 (END)







The error indicates that the Grafana server is failing to start. Here's how you can troubleshoot and resolve the issue to start the Grafana instance:

1. Check Logs for Detailed Errors
Run the following command to view detailed logs for the Grafana service:

bash
Copy code
sudo journalctl -u grafana-server -xe
This will give more context on why the service is failing.

2. Verify Configuration File
Ensure that the configuration file /etc/grafana/grafana.ini exists and is properly configured.

Check if the file exists:

bash
Copy code
ls -l /etc/grafana/grafana.ini
If missing, re-install or copy the default config:

bash
Copy code
sudo cp /usr/share/grafana/conf/defaults.ini /etc/grafana/grafana.ini
Validate the file permissions:

bash
Copy code
sudo chown grafana:grafana /etc/grafana/grafana.ini
3. Correct ExecStart in the Service File
Edit the service file to ensure it points to the correct paths and arguments. Use:

bash
Copy code
sudo nano /lib/systemd/system/grafana-server.service
Update the ExecStart directive:

ini
Copy code
ExecStart=/usr/share/grafana/bin/grafana-server \
  --homepath=/usr/share/grafana \
  --config=/etc/grafana/grafana.ini \
  --pidfile=/var/run/grafana/grafana-server.pid
Then reload systemd:

bash
Copy code
sudo systemctl daemon-reload
4. Fix File Permissions
Ensure the Grafana directories have the correct permissions:

bash
Copy code
sudo chown -R grafana:grafana /var/lib/grafana /etc/grafana /usr/share/grafana
5. Restart Grafana
Try restarting the Grafana server:

bash
Copy code
sudo systemctl restart grafana-server
6. Check Grafana Status
Verify if the service is running:

bash
Copy code
sudo systemctl status grafana-server
7. Manually Test the Grafana Binary
If the service still fails, try running Grafana manually to check for errors:

bash
Copy code
/usr/share/grafana/bin/grafana-server --homepath=/usr/share/grafana --config=/etc/grafana/grafana.ini
Additional Notes:
Ensure no port conflicts on 3000 (Grafana's default port). Check using:
bash
Copy code
sudo netstat -tuln | grep 3000
Let me know if you need further help!







ReadObject: expect { or , or } or n, but found <, error found in #1 byte of ...|<html> <he|..., bigger context ...|<html> <head><title>Postgres exporter</title></hea|... - There was an error returned querying the Prometheus API.







------------------------------today 4/12/2024 -------------------------------------


root@krunal-vostro-3681:/usr/share/grafana/plugins-bundled# grafana-cli plugins ls
installed plugins:
grafana-lokiexplore-app @ 1.0.4
root@krunal-vostro-3681:/usr/share/grafana/plugins-bundled# sudo systemctl restart grafana-server.service
root@krunal-vostro-3681:/usr/share/grafana/plugins-bundled# sudo systemctl status  grafana-server.service
● grafana-server.service - Grafana instance
   Loaded: loaded (/lib/systemd/system/grafana-server.service; enabled; vendor preset: enabled)
  Drop-In: /etc/systemd/system/grafana-server.service.d
           └─override.conf
   Active: active (running) since Wed 2024-12-04 17:28:44 IST; 7s ago
 Main PID: 32556 (grafana)
    Tasks: 23 (limit: 4915)
   CGroup: /system.slice/grafana-server.service
           └─32556 grafana server --homepath=/usr/share/grafana --config=/etc/grafana/grafana.ini --pidfile=/var/run/grafana/grafana-server.pid

Dec 04 17:28:44 hp grafana-server[32556]: logger=ngalert.scheduler t=2024-12-04T17:28:44.826980099+05:30 level=info msg="Starting scheduler" tickInterval=10s maxAttempts=1
Dec 04 17:28:44 hp grafana-server[32556]: logger=ticker t=2024-12-04T17:28:44.827022031+05:30 level=info msg=starting first_tick=2024-12-04T17:28:50+05:30
Dec 04 17:28:44 hp grafana-server[32556]: logger=http.server t=2024-12-04T17:28:44.82855852+05:30 level=info msg="HTTP Server Listen" address=[::]:3001 protocol=http subUrl= socket=
Dec 04 17:28:45 hp grafana-server[32556]: logger=grafana-apiserver t=2024-12-04T17:28:45.050769635+05:30 level=info msg="Adding GroupVersion playlist.grafana.app v0alpha1 to ResourceManager"
Dec 04 17:28:45 hp grafana-server[32556]: logger=grafana-apiserver t=2024-12-04T17:28:45.051190292+05:30 level=info msg="Adding GroupVersion featuretoggle.grafana.app v0alpha1 to ResourceManager"
Dec 04 17:28:45 hp grafana-server[32556]: logger=grafana-apiserver t=2024-12-04T17:28:45.052075401+05:30 level=info msg="Adding GroupVersion iam.grafana.app v0alpha1 to ResourceManager"
Dec 04 17:28:45 hp grafana-server[32556]: logger=provisioning.dashboard t=2024-12-04T17:28:45.068514115+05:30 level=info msg="starting to provision dashboards"
Dec 04 17:28:45 hp grafana-server[32556]: logger=provisioning.dashboard t=2024-12-04T17:28:45.068550781+05:30 level=info msg="finished to provision dashboards"
Dec 04 17:28:45 hp grafana-server[32556]: logger=grafana.update.checker t=2024-12-04T17:28:45.392409908+05:30 level=info msg="Update check succeeded" duration=565.71883ms
Dec 04 17:28:45 hp grafana-server[32556]: logger=plugins.update.checker t=2024-12-04T17:28:45.459397632+05:30 level=info msg="Update check succeeded" duration=632.676724ms
root@krunal-vostro-3681:/usr/share/grafana/plugins-bundled# cd 
root@krunal-vostro-3681:~# grafana-cli plugins update grafana-lokiexplore-app
INFO [12-04|17:29:32] Starting Grafana                         logger=settings version=11.3.1 commit=9225f4a1cbd1cfe8b69f1aa2d62309a9700533a5 branch=HEAD compiled=2024-12-04T17:29:32+05:30
INFO [12-04|17:29:32] Config loaded from                       logger=settings file=/usr/share/grafana/conf/defaults.ini
INFO [12-04|17:29:32] Config loaded from                       logger=settings file=/etc/grafana/grafana.ini
INFO [12-04|17:29:32] Config overridden from command line      logger=settings arg="default.paths.data=/var/lib/grafana"
INFO [12-04|17:29:32] Config overridden from command line      logger=settings arg="default.paths.logs=/var/log/grafana"
INFO [12-04|17:29:32] Config overridden from command line      logger=settings arg="default.paths.plugins=/var/lib/grafana/plugins"
INFO [12-04|17:29:32] Config overridden from command line      logger=settings arg="default.paths.provisioning=/etc/grafana/provisioning"
INFO [12-04|17:29:32] Target                                   logger=settings target=[all]
INFO [12-04|17:29:32] Path Home                                logger=settings path=/usr/share/grafana
INFO [12-04|17:29:32] Path Data                                logger=settings path=/usr/share/grafana/data
INFO [12-04|17:29:32] Path Logs                                logger=settings path=/usr/share/grafana/data/log
INFO [12-04|17:29:32] Path Plugins                             logger=settings path=/usr/share/grafana/data/plugins
INFO [12-04|17:29:32] Path Provisioning                        logger=settings path=/usr/share/grafana/conf/provisioning
INFO [12-04|17:29:32] App mode production                      logger=settings
✔ grafana-lokiexplore-app is up to date 
root@krunal-vostro-3681:~# grafana-cli plugins remove grafana-lokiexplore-app
Removing plugin: grafana-lokiexplore-app
Please restart Grafana after installing or removing plugins. Refer to Grafana documentation for instructions if necessary.

root@krunal-vostro-3681:~# sudo systemctl restart grafana-server.service
root@krunal-vostro-3681:~# sudo systemctl restart grafana
root@krunal-vostro-3681:~# sudo systemctl status  grafana
● grafana.service - Grafana
   Loaded: loaded (/etc/systemd/system/grafana.service; enabled; vendor preset: enabled)
   Active: failed (Result: exit-code) since Wed 2024-12-04 17:30:14 IST; 3s ago
  Process: 560 ExecStart=/usr/share/grafana/bin/grafana-server --config=/etc/grafana/grafana.ini --homepath=/usr/share/grafana (code=exited, status=1/FAILURE)
 Main PID: 560 (code=exited, status=1/FAILURE)

Dec 04 17:30:14 hp systemd[1]: grafana.service: Service hold-off time over, scheduling restart.
Dec 04 17:30:14 hp systemd[1]: grafana.service: Scheduled restart job, restart counter is at 5.
Dec 04 17:30:14 hp systemd[1]: Stopped Grafana.
Dec 04 17:30:14 hp systemd[1]: grafana.service: Start request repeated too quickly.
Dec 04 17:30:14 hp systemd[1]: grafana.service: Failed with result 'exit-code'.
Dec 04 17:30:14 hp systemd[1]: Failed to start Grafana.
root@krunal-vostro-3681:~# journalctl -u grafana.service -f
-- Logs begin at Thu 2023-03-02 18:28:05 IST. --
Dec 04 17:30:14 hp grafana-server[560]: Error: ✗ *api.HTTPServer run error: failed to open listener on address 0.0.0.0:3001: listen tcp 0.0.0.0:3001: bind: address already in use
Dec 04 17:30:14 hp grafana-server[560]: logger=grafana-apiserver t=2024-12-04T17:30:14.774096578+05:30 level=info msg="StorageObjectCountTracker pruner is exiting"
Dec 04 17:30:14 hp systemd[1]: grafana.service: Main process exited, code=exited, status=1/FAILURE
Dec 04 17:30:14 hp systemd[1]: grafana.service: Failed with result 'exit-code'.
Dec 04 17:30:14 hp systemd[1]: grafana.service: Service hold-off time over, scheduling restart.
Dec 04 17:30:14 hp systemd[1]: grafana.service: Scheduled restart job, restart counter is at 5.
Dec 04 17:30:14 hp systemd[1]: Stopped Grafana.
Dec 04 17:30:14 hp systemd[1]: grafana.service: Start request repeated too quickly.
Dec 04 17:30:14 hp systemd[1]: grafana.service: Failed with result 'exit-code'.
Dec 04 17:30:14 hp systemd[1]: Failed to start Grafana.
^C^C
root@krunal-vostro-3681:~# sudo netstat -tuln | grep 3000
root@krunal-vostro-3681:~# /usr/share/grafana/bin/grafana-server --config=/etc/grafana/grafana.ini --homepath=/usr/share/grafana
Deprecation warning: The standalone 'grafana-server' program is deprecated and will be removed in the future. Please update all uses of 'grafana-server' to 'grafana server'
Grafana server is running with elevated privileges. This is not recommended
INFO [12-04|17:31:38] Starting Grafana                         logger=settings version=11.3.1 commit=9225f4a1cbd1cfe8b69f1aa2d62309a9700533a5 branch=HEAD compiled=2024-12-04T17:31:38+05:30
INFO [12-04|17:31:38] Config loaded from                       logger=settings file=/usr/share/grafana/conf/defaults.ini
INFO [12-04|17:31:38] Config loaded from                       logger=settings file=/etc/grafana/grafana.ini
INFO [12-04|17:31:38] Target                                   logger=settings target=[all]
INFO [12-04|17:31:38] Path Home                                logger=settings path=/usr/share/grafana
INFO [12-04|17:31:38] Path Data                                logger=settings path=/usr/share/grafana/data
INFO [12-04|17:31:38] Path Logs                                logger=settings path=/usr/share/grafana/data/log
INFO [12-04|17:31:38] Path Plugins                             logger=settings path=/usr/share/grafana/data/plugins
INFO [12-04|17:31:38] Path Provisioning                        logger=settings path=/usr/share/grafana/conf/provisioning
INFO [12-04|17:31:38] App mode production                      logger=settings
INFO [12-04|17:31:38] FeatureToggles                           logger=featuremgmt nestedFolders=true influxdbBackendMigration=true cloudWatchRoundUpEndTime=true lokiMetricDataplane=true logsExploreTableVisualisation=true lokiQuerySplitting=true ssoSettingsApi=true managedPluginsInstall=true cloudWatchCrossAccountQuerying=true angularDeprecationUI=true recoveryThreshold=true dataplaneFrontendFallback=true logsInfiniteScrolling=true publicDashboards=true dashgpt=true groupToNestedTableTransformation=true tlsMemcached=true exploreMetrics=true kubernetesPlaylists=true lokiStructuredMetadata=true autoMigrateXYChartPanel=true correlations=true promQLScope=true alertingInsights=true alertingSimplifiedRouting=true dashboardSceneSolo=true cloudWatchNewLabelParsing=true notificationBanner=true prometheusConfigOverhaulAuth=true alertingNoDataErrorExecution=true topnav=true transformationsVariableSupport=true dashboardScene=true awsAsyncQueryCaching=true formatString=true accessControlOnCall=true publicDashboardsScene=true logRowsPopoverMenu=true dashboardSceneForViewers=true lokiQueryHints=true openSearchBackendFlowEnabled=true addFieldFromCalculationStatFunctions=true logsContextDatasourceUi=true recordedQueriesMulti=true panelMonitoring=true prometheusMetricEncyclopedia=true annotationPermissionUpdate=true pinNavItems=true transformationsRedesign=true prometheusAzureOverrideAudience=true
INFO [12-04|17:31:38] Connecting to DB                         logger=sqlstore dbtype=sqlite3
INFO [12-04|17:31:39] Locking database                         logger=migrator
INFO [12-04|17:31:39] Starting DB migrations                   logger=migrator
INFO [12-04|17:31:39] migrations completed                     logger=migrator performed=0 skipped=611 duration=420.161µs
INFO [12-04|17:31:39] Unlocking database                       logger=migrator
INFO [12-04|17:31:39] Envelope encryption state                logger=secrets enabled=true current provider=secretKey.v1
INFO [12-04|17:31:39] Restored cache from database             logger=plugin.angulardetectorsprovider.dynamic duration=294.593µs
INFO [12-04|17:31:39] Loading plugins...                       logger=plugin.store
ERROR[12-04|17:31:39] Could not register plugin                logger=plugins.registration pluginId=xychart error="plugin xychart is already registered"
ERROR[12-04|17:31:39] Could not initialize plugin              logger=plugins.initialization pluginId=xychart error="plugin xychart is already registered"
INFO [12-04|17:31:39] Plugin registered                        logger=plugins.registration pluginId=grafana-lokiexplore-app
INFO [12-04|17:31:39] Plugins loaded                           logger=plugin.store count=55 duration=51.902537ms
INFO [12-04|17:31:39] Query Service initialization             logger=query_data
INFO [12-04|17:31:39] Live Push Gateway initialization         logger=live.push_http
INFO [12-04|17:31:39] Applying new configuration to Alertmanager logger=ngalert.notifier.alertmanager org=1 configHash=d2c56faca6af2a5772ff4253222f7386
INFO [12-04|17:31:39] Running in alternative execution of Error/NoData mode logger=ngalert.state.manager
INFO [12-04|17:31:39] registering usage stat providers         logger=infra.usagestats.collector usageStatsProvidersLen=2
INFO [12-04|17:31:39] starting to provision alerting           logger=provisioning.alerting
INFO [12-04|17:31:39] finished to provision alerting           logger=provisioning.alerting
INFO [12-04|17:31:39] Storage starting                         logger=grafanaStorageLogger
INFO [12-04|17:31:39] Warming state cache for startup          logger=ngalert.state.manager
INFO [12-04|17:31:39] Starting MultiOrg Alertmanager           logger=ngalert.multiorg.alertmanager
ERROR[12-04|17:31:39] Stopped background service               logger=server service=*api.HTTPServer reason="failed to open listener on address 0.0.0.0:3001: listen tcp 0.0.0.0:3001: bind: address already in use"
ERROR[12-04|17:31:39] Update check failed                      logger=grafana.update.checker error="failed to get stable version from grafana.com: Get \"https://grafana.com/api/grafana/versions/stable\": *api.HTTPServer run error: failed to open listener on address 0.0.0.0:3001: listen tcp 0.0.0.0:3001: bind: address already in use" duration=1.118663ms
INFO [12-04|17:31:39] Closing tracing                          logger=tracing
ERROR[12-04|17:31:39] Unable to fetch orgIds                   logger=ngalert.state.manager error="context canceled"
INFO [12-04|17:31:39] State cache has been initialized         logger=ngalert.state.manager states=0 duration=13.001598ms
INFO [12-04|17:31:39] Starting scheduler                       logger=ngalert.scheduler tickInterval=10s maxAttempts=1
INFO [12-04|17:31:39] starting                                 logger=ticker first_tick=2024-12-04T17:31:40+05:30
WARN [12-04|17:31:39] Failed to get usage metrics              logger=serviceaccounts error="context canceled"
WARN [12-04|17:31:39] Failed to delete orphaned provisioned dashboards logger=provisioning.dashboard err="context canceled"
ERROR[12-04|17:31:39] Server lock for secret migration already exists logger=secret.migration
INFO [12-04|17:31:39] starting to provision dashboards         logger=provisioning.dashboard
INFO [12-04|17:31:39] finished to provision dashboards         logger=provisioning.dashboard
ERROR[12-04|17:31:39] Error downloading plugin manifest keys   logger=plugin.signature.key_retriever error="kv get: context canceled"
INFO [12-04|17:31:39] Adding GroupVersion playlist.grafana.app v0alpha1 to ResourceManager logger=grafana-apiserver
INFO [12-04|17:31:39] Adding GroupVersion featuretoggle.grafana.app v0alpha1 to ResourceManager logger=grafana-apiserver
INFO [12-04|17:31:39] Adding GroupVersion iam.grafana.app v0alpha1 to ResourceManager logger=grafana-apiserver
Error: ✗ *api.HTTPServer run error: failed to open listener on address 0.0.0.0:3001: listen tcp 0.0.0.0:3001: bind: address already in use
root@krunal-vostro-3681:~# 






----------------------------------------------------------------------------------------------------------------------------------------





root@hp:/etc# curl http://localhost:9187/metrice
<html>
	<head><title>Postgres exporter</title></head>
	<body>
	<h1>Postgres exporter</h1>
	<p><a href='/metrics'>Metrics</a></p>
	</body>
	</html>
	root@hp:/etc# ^C



ReadObject: expect { or , or } or n, but found <, error found in #1 byte of ...|<html> <he|..., bigger context ...|<html> <head><title>Postgres exporter</title></hea|... - There was an error returned querying the Prometheus API.






https://www.howtoforge.com/how-to-monitor-postgresql-with-prometheus-and-grafana/





