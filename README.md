# Grafana with SSL Certificate in a Docker Swarm

Configure Traefik and create secrets for storing the passwords on the Docker Swarm manager node before applying the configuration.

Traefik configuration: https://github.com/heyValdemar/traefik-ssl-certificate-docker-swarm

Create a secret for storing the password for Grafana database using the command:

`printf "YourPassword" | docker secret create grafana-postgres-password -`

Create a secret for storing the password for Grafana administrator using the command:

`printf "YourPassword" | docker secret create grafana-application-password -`

Create a secret for storing the password for Grafana email account using the command:

`printf "YourPassword" | docker secret create grafana-email-password -`

Clear passwords from bash history using the command:

`history -c && history -w`

Create a secret for storing the Grafana configuration using the command:

`docker secret create ldap.toml /path/to/ldap.toml`

Run `grafana-restore-application-data.sh` on the Docker Swarm worker node where the container for backups is running to restore application data if needed.

Run `docker stack ps grafana | grep grafana_backup | awk 'NR > 0 {print $4}'` on the Docker Swarm manager node to find on which node container for backups is running.

Deploy Grafana in a Docker Swarm using the command:

`docker stack deploy -c grafana-traefik-ssl-certificate-docker-swarm.yml grafana`
