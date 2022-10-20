# Smart Home

My smart home configuration, using Docker and Home Assistant.

## Usage

1. Create a file `dockerData/traefik/data/acme.json` and change its permissions to `600`. Traefik will write certificates to this file.
2. Run `docker network create traefik-network` to create a Docker network for traefik.
3. Run `docker network insepct traefik-network` and note the `IPAM > Config > Gateway` value. In `docker-compose.yml`, change the IP in `services > traefik > extra_hosts` to that value. In `dockerData/homeassistant/configuration.yaml`, change the first `http > trusted_proxies` to that value (in CIDR) notation.
3. Run `docker-compose up -d` to start the network stack.

## Post-Docker Setup Steps

* Heimdall
  1. Add password to `admin` user
  2. Add applications: Grafana, InfluxDB, and HomeAssistant (local and external)
* InfluxDB
  1. Login as admin and generate an API token that has read and write permission to the `homeassistant` bucket
* Home Assistant
  1. Login and setup
* Grafana
  1. Login and add InfluxDB as a data source
* Home Assistant
  1. Add the `MQTT` integration. Use `127.0.0.1` for the broker and `1883` for the port.
  2. Add the `AdguardHome` integration. Use `127.0.0.1` for the host and `3001` for the port.

## Notes

If using WSL2 for development, ports needs to be forwarded from Windows to the WSL2 VM using the following command:

`netsh interface portproxy add v4tov4 listenport=<PORT> listenaddress=0.0.0.0 connectport=<PORT> connectaddress=<IP OF VM>`

You can use `ifconfig` within WSL2 to find the VM's IP address.

## Useful Grafana dashboards

* <https://grafana.com/grafana/dashboards/15141>

## TODO

## Refernces

* <https://github.com/OliverHi/smarthomeserver>
* <https://thesmarthomejourney.com/2021/05/02/grafana-influxdb-home-assistant/>
* <https://thesmarthomejourney.com/2021/05/30/add-grafana-to-home-assistant/>
* <https://community.home-assistant.io/t/home-assistant-in-docker-hosts-mode-with-traefik-2-and-lets-encrypt-working-sample/190476/6>
* <https://www.reddit.com/r/homeassistant/comments/uvicqd/thanks_to_everyone_here_what_a_great_community/>
* <https://github.com/tribut/homeassistant-docker-venv>