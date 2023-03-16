# Example Java Gradle Application featuring MQTT and Grafana

## Requirements
 - Java JDK 17

## Recommendations
 - Use IntelliJ IDEA

## Getting started
- Clone the repository
```bash
git clone https://github.com/alptbz/mqttgrafanademo
```
- Change into repository
```bash
cd mqttgrafanademo
```
- Copy config example and edit accordingly
```bash
cp config.properties.example config.properties
```
- Open Folder with IntelliJ IDEA
- Run gradle
- Fix gradle errors (mostly wrong version, java 17 requires gradle 7.2)
- Go to `src/main/ch.alptbz.mqttgrafanademo/Main`, right click file, select run

## Run Grafana
### Requirements
 - Install podman from https://github.com/containers/podman/releases
 - Check out installation instructions: https://podman.io/getting-started/installation
 - Open PowerShell CLI
 - change into repository directory `podman`
 - `podman play kube .\influxdb-grafana.yaml`
 - InfluxDB Dashboard: `http://localhost:8086/`
 - Grafana Dashboard: `http://localhost:3000` 

### Example Query for influxDB
```
from(bucket: "test")
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
  |> filter(fn: (r) => r["_measurement"] == "temperature")
```

## Links
- https://github.com/eclipse/paho.mqtt.java
- https://github.com/pengrad/java-telegram-bot-api

## Miscellaneous

### podman machine not starting
executing `podman machine start` returns
"Error: invalid character '\x00' looking for beginning of value".
 - Remove VM with `wsl --unregister podman-machine-default`
 - Recreate using `podman machine init`

### Run container directly



# Run Grafana, InfluxDB and Telegraf in ubuntu

```bash
podman pod create -n grafana-influx -h grafana-influx -p 3000:3000,8086:8086
podman run -dt --pod=grafana-influx --name=grafana docker.io/grafana/grafana
podman run -dt --pod=grafana-influx --name=influx docker.io/influxdb:latest 
```

## Telegraf
Get Telegraf config within influx WebUI

Install according to https://docs.influxdata.com/telegraf/v1.21/introduction/installation/

