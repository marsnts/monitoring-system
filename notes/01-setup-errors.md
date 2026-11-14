
**Exporter Issues:**
##### Ensure the Exporters is present on configured PATH

`status=203/EXEC` means that systemd tried to run the exporter but failed to execute the binary file. This can be triggered by different reasons.

Check whether the binary file is present on the configured path. 
```ini
[Service]
ExecStart=/usr/local/bin/nginx-prometheus-exporter
```
Always make sure that the exporter file is present on `/usr/local/bin/`

##### Permission Errors

`Special user nobody configured` is present on the log line because the service is running as the `nobody` user. 

Check directory/file permissions and ownerships.
```ini
[Service]
User=nobody
; This induicates that nobody user can run it 
```

This can be fixed by granting execution permissions for all users or changing the service user to `root`

###### Granting Execution permission to all users:
```bash
sudo chmod 755 /usr/local/bin/nginx-prometheus-exporter
```
then run `daemon-reload` and restart the exporter service

###### Changing Service User to `root`
Navigate to `/usr/local/bin/_exporter`:
```bash
sudo nano etc/systemd/system/nginx-prometheus-exporter
# or
sudo systemctl edit --full nginx-prometheus-exporter.service
```
Find the `[Service]` section and change the `User` to `root`:
```ini
[Service]
User=root
```
then run `daemon-reload` and restart the exporter service

