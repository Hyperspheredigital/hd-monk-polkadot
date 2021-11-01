# Polkadot+Monk demo templates

This repo contains templates used in the [Polkadot](https://polkadot.network/)+[Monk](https://monk.io) demo.
Note that these templates are using official, unmodified Docker images from Polkadot and Acala.

These templates are for demonstration and development purposes and they are not intended for production use i.e. running secure validators for [Polkadot](https://polkadot.network/) or [Acala](https://acala.network/). While it is certainly possible with Monk, these templates are not implementing such functionality at this moment.

## Prep

You'll need Monk installed on your machine to run these templates: [Getting Monk](https://docs.monk.io/docs/get-monk).

Read if you want to run a Monk cluster: [Creating a Monk Cluster](https://docs.monk.io/docs/monk-in-10#creating-a-monk-cluster).

Read the [Documentation](https://docs.monk.io/docs/).

## Usage

First, load all templates in this order:

```
monk load polkadot.yaml acala.yaml monitoring-files.yaml monitoring.yaml
```

Run Polkadot node:

```
monk run polkadot/node              # to run on local machine, or
monk run -t mytag polkadot/node     # to run on the cluster (applies to all examples)
```

Run Polkadot node with persistent volume (cluster only):

```
monk run -t mytag polkadot/node-persistent     # to run on the cluster
```

Run Acala node:

```
monk run acala/node
```

Add monitoring to your polkadot node:

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
monitoring.yaml - Prometheus + Grafana configured to work against a substrate node
parachain-dev.yaml - experimental relaychain+parachain dev setup using Acala as an example
```

Load files locally in this order:

```
monk load polkadot.yaml acala.yaml monitoring-files.yaml monitoring.yaml parachain-dev.yaml
```

Run as described above. You'll need to reload and update when you change a file:

```
monk load <changed file>
monk update some/runnable
```

Logs and Shell access:

```
monk logs some/runnable
monk shell some/runnable
```

## Get in touch

-   Join Monk on [Discord](https://discord.gg/WxDzaKe)
-   Follow Monk on [Twitter](https://twitter.com/monk_io)
