# Perfsonar 4.0 Access Layer Mesh
This is the code and configuration repository for operation of the Perfsonar 4.0 access layer mesh of low-cost endpoints at the University of Michigan.
## Configuration details
This mesh utilizes IP address authentication and requires DHCP reserved IP addresses delivered to each mesh endpoint. Mesh data is collected at the central measurement archive running in a MiServer VM. The central measurement archive aggregates the data from multiple meshes, including this one. The data is rendered out in MadDash, which is installed separately.

The central measurement archive uses Certbot for SSL, and a cron job for certbot renew --quiet is set to run twice daily. Certificate validation is currently not configured on mesh endpoints, but it should be.
## Method for rendering conf file to json from repo
/usr/lib/perfsonar/bin/build_json -o /var/www/html/perfsonar_config.json perfsonar_config.conf
## Method for adding hosts to IP auth for esmond
cd /usr/lib/esmond sudo -s 
. bin/activate

python esmond/manage.py add_user_ip_address example_user 10.0.1.1 10.0.2.0/24

View ip auth host list at https://your_host/esmond/admin using django creds
## Files that must be configured
/etc/perfsonar/meshconfig-agent.conf
/etc/perfsonar/meshconfig-guiagent.conf (central measurement archive only)
/etc/bwctl-server/bwctl-server.limits (this file is hosted in the git repo and should be cp to the host's /etc/bwctl-server directory)
/etc/maddash/maddash-server/maddash.yaml (weed default meshes if desired)
/etc/pscheduler/limit.conf (remove udp match:false)
## Method for adding user to configure MadDash web interface
The administrator web interface requires a username and password through HTTP BASIC authentication to gain access to the page. By default no users are created. You may use Apache’s htpasswd command to add users to the file /etc/maddash/maddash-webui/admin-users. The command to add a user (e.g. myadmin) is shown below:

htpasswd /etc/maddash/maddash-webui/admin-users myadmin
