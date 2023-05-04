<pre>NODOS PROMETEUS

wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz
tar xvf node_exporter-1.5.0.linux-amd64.tar.gz
cd node_exporter-1.5.0.linux-amd64/

./node_exporter &

SERVIDOR PROMETHEUS

wget https://github.com/prometheus/prometheus/releases/download/v2.44.0-rc.0/prometheus-2.44.0-rc.0.linux-amd64.tar.gz

tar xvf prometheus-2.44.0-rc.0.linux-amd64.tar.gz
cd prometheus-2.44.0-rc.0.linux-amd64/

EDITAR fichero yaml ---- ejemplo

# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
          # - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "prometheus"

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
      - targets: ["localhost:9090"]
  - job_name: "node1"
    static_configs:
      - targets: ["node1:9100"]

  - job_name: "node2"
    static_configs:
      - targets: ["node2:9100"]
      
GRAFANA SERVER
wget https://dl.grafana.com/enterprise/release/grafana-enterprise-9.5.1.linux-amd64.tar.gz
tar xvf grafana-enterprise-9.5.1.linux-amd64.tar.gz 
cd grafana-9.5.1/
./bin/grafana-server &

AGREGAR DATASOURCE
prometheus --- http://localhost:9090



INSTALANDO MEDIANTE DOCKER
en el directorio de ejecución ubucar prometheus.yml OJO CON EL DIRECCIONAMIENTO!!!!

docker run -d -p 9090:9090 --name prome -v $PWD:/etc/prometheus prom/prometheus

docker run -d --name=grafana -p 3000:3000 grafana/grafana

Docker compose 


version: '3.8'
services:
  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
  prometheus:
    image: prom/prometheus
    ports:
      - "9090:9090"
    volumes:
      - $PWD:/etc/prometheus
      
URLS - para crear servicios systemd https://nextgentips.com/2022/07/09/how-to-install-prometheus-server-on-ubuntu-22-04/?utm_source=rss&utm_medium=rss&utm_campaign=how-to-install-prometheus-server-on-ubuntu-22-04
</pre>
