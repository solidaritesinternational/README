# PROMETHEUS

## Prometheus service

Prometheus is a monitoring service to check the status of all the servers you are administrating. You can monitor if your web applications are accessible through http for everytone, if your CPU consumption is too high, if your server response time is acceptable, how much memory is still available ...

To monitor our servers, we use the Blackbox Exporter module of Prometheus, to test http accessibility and DNS probe times.

The configuration of the prometheus service is at the server level and the relay level, and is handled in our NixOS configuration.

## Graphana Dashboards

The Graphana dashboards allow to view your Prometheus monitoring results in a user-friendly way, to help you know everything you want to know about your servers with a quick glance.

Dashboards are configured with .json files, many of which are available, open source, and ready to use.
The dashboards we use are the following:

- the official Blackbox Exporter dashboard (<https://grafana.com/grafana/dashboards/7587>): [prometheus-blackbox-exporter_rev3.json](prometheus-blackbox-exporter_rev3.json)
- A custom simplified version of the above dashboard, to view only the Http port status: [prometheus-blackbox-exporter_rev3_simplified.json](prometheus-blackbox-exporter_rev3_simplified.json)

The Graphana web application, configured with these dashboards, is running on our relay servers, acessible on the port **3000**.

<http://relay0.solis-demo.org:3000/>
<http://relay1.solis-demo.org:3000/>

## Documentation

<https://prometheus.io/>
<https://github.com/prometheus/blackbox_exporter>
<https://grafana.com/>
