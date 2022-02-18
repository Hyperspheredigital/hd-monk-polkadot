# Polkadot+Monk demo templates

This repo contains templates used in the [Polkadot](https://polkadot.network/)+[Monk](https://monk.io) deployment.
Note that these templates are using official, unmodified Docker images.

## Prep

You'll need Monk installed on your machine to run these templates: [Getting Monk](https://docs.monk.io/docs/get-monk).

Read if you want to run a Monk cluster: [Creating a Monk Cluster](https://docs.monk.io/docs/monk-in-10#creating-a-monk-cluster).

Read the [Documentation](https://docs.monk.io/docs/).

## Basic Usage

First, load all templates in this order:

```
monk load polkadot.yaml acala.yaml monitoring-files.yaml basic-monitoring.yaml
```

- Run Polkadot node:

```
monk run polkadot/node              # to run on local machine, or
monk run -t mytag polkadot/node     # to run on the cluster (applies to all examples)
```

- Run Polkadot node with persistent volume (cluster only):

```
monk run -t mytag polkadot/node-persistent     # to run on the cluster
```

- Run Polkadot validator with persistent volume (cluster only):

```
monk run -t mytag polkadot/validator-node     # to run on the cluster
```

- Run Acala node:

```
monk run acala/node
```

- Add monitoring to your polkadot node:

```
monk run -t mytag polkadot/monitoring
```

This will start grafana & prometheus pre-configured to read stats off your `polkadot/node`.

## Caveats

Attempting to run both Polkadot and Acala on the same machine at the same time will likely fail because of port collisions. Just stop the previous one or select another tag when running the second one.

## Hacking

Feel free to pull these templates and modify them to your liking. PRs are welcome as well :)

File contents:

```
polkadot.yaml   - basic Polkadot node, node with persistent volume, westend node
acala.yaml      - basic Acala and Karura collator nodes
monitoring-files.yaml - Grafana config for the Polkadot dashboard
basic-monitoring.yaml - Prometheus + Grafana configured to work against a substrate node. Basic configuration
parachain-dev.yaml - experimental relaychain+parachain dev setup using Acala as an example
```

### Use configurations as templates for private deployments

Load files locally in this order:

```
monk load polkadot.yaml acala.yaml monitoring-files.yaml basic-monitoring.yaml monitoring-config.yaml custom-monitoring.yaml
```
1. Deploy custom validator
- create `<name>.yaml` file:
```
namespace: custom

validator-node:
  defines: runnable
  inherits: polkadot/node-persistent
  variables:
    options: --validator --ws-port 9944 --unsafe-ws-external --unsafe-pruning --pruning 1000 --rpc-methods=Unsafe -l'sync=warn,afg=warn,babe=warn' --telemetry-url 'wss://telemetry.polkadot.io/submit/ 1'
    name: hd-validator-node-on-monk-io
    image_tag: v0.9.16
```
- import and deploy:
```
monk load <name>.yaml
monk run -t mytag custom/validator-node
```

- If you need to update configuration, just edit the file and reload it
```
monk load <changed file>
monk update some/runnable
```

Logs and Shell access:

```
monk logs some/runnable
monk shell some/runnable
```

2. Deploy Monitoring with alerting
- Pre-requisites:
  - Slack Channel created

- Process:
  - create `monitoring.yaml` file:
```
namespace: custom

prometheus:
  defines: runnable
  inherits: polkadot/prometheus-custom
  variables:
    node-addr: <- get-hostname($runnable, "node") ":9615" ", 192.117.1.17:9615" concat

alertmanager:
  defines: runnable
  inherits: polkadot/alertmanager
  variables:
    slack-api-url: https://hooks.slack.com/services/XXXXXXXXX/XXXXXXXXX/XXXXXXXXXdasdaddas
    slack-channel: "alerts"

monitoring:
  defines: process-group
  runnable-list:
    - custom/alertmanager
    - custom/prometheus
    - polkadot/grafana-custom
  variables:
    defines: variables
    runnable: polkadot/validator-node
```
  - import and deploy:
```
monk load monitoring.yaml
monk run -t mytag custom/monitoring
```

## Get in touch

-   Join Monk on [Discord](https://discord.gg/WxDzaKe)
-   Follow Monk on [Twitter](https://twitter.com/monk_io)
-   Hypersphere main [Site](https://hypersphere.ventures/)