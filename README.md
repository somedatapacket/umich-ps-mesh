# Perfsonar 4.0 Access Layer Mesh
This is the code and configuration repository for operation of the Perfsonar 4.0 access layer mesh of low-cost endpoints at the University of Michigan.
## Configuration details
This mesh utilizes IP address authentication and requires DHCP reserved IP addresses delivered to each mesh endpoint. Mesh data is collected at the central measurement archive running in a MiServer VM. The central measurement archive aggregates the data from multiple meshes, including this one.

The central measurement archive uses Certbot for SSL, and a cron job for certbot renew --quiet is set to run twice daily. Certificate validation is currently not configured on mesh endpoints, but it should be.
## Useful commands
/usr/lib/perfsonar/bin/build_json -o /var/www/html/perfsonar_config.json perfsonar_config.conf
## Method for adding hosts to IP auth for esmond
cd /usr/lib/esmond sudo -s source /opt/rh/python27/enable /opt/rh/python27/root/usr/bin/virtualenv --prompt="(esmond)" . . bin/activate ./configure_esmond

python esmond/manage.py add_user_ip_address example_user 10.0.1.1 10.0.2.0/24

View ip auth host list at https://your_host/esmond/admin using django creds
