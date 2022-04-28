
VAST Prometheus Exporter
========================

The Prometheus exporter connects to VMS and leverages its REST API to extract state and metric information.
It listens to port 8000 by default. This can be changed using the --port parameter.

Content
-------

1. Cluster capacity and states.
2. Physical component states (NIC, node, drive, etc').
2. View path and capacity information.
4. Performance metrics (BW/IOPS/latency/etc').

Pre-requisities
---------------

Cluster of version 4.2 and up. Contact support in case you need an older version supported.

Installation Using Docker
-------------------------

    # build a local docker image
    $ ./build.sh
    # run a docker container in the background
    $ ./run.sh --user=<USER> --password=<PASSWORD> --address=<ADDRESS>

Installation Using Python
-------------------------

    $ pip install -r requirements.txt
    $ ./vast_exporter.py --user=<USER> --password=<PASSWORD> --address=<ADDRESS>


Testing
-------

    $ ./vast_exporter.py --user=<USER> --password=<PASSWORD> --address=<ADDRESS> --test
    2022-04-28 11:36:47,045 MainThread INFO: VAST Exporter started running. Listening on port 8001
    2022-04-28 11:36:58,658 MainThread INFO: Collection is successful!


Monitoring
----------

Besides the obious stdout output where errors will land, the following metric will be incremeneted upon an error:

    vast_collector_errors_total

Prometheus Config
-----------------

Full explanation of Prometheus configuration here: https://prometheus.io/docs/prometheus/latest/configuration/configuration/

The following snippet shows how to add the VAST exporter to Prometheus:

    # A scrape configuration containing exactly one endpoint to scrape:
    # Here it's Prometheus itself.
    scrape_configs:
      # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
      - job_name: "vast"

        # metrics_path defaults to '/metrics'
        # scheme defaults to 'http'.

        static_configs:
          - targets: ["<EXPORTER HOST>:8000"]

How to run Prometheus using docker:

    docker run -p 9090:9090 -v /path/to/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus
