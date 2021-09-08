# Polkadot+Monk demo templates

This repo contains templates used in the [Polkadot](https://polkadot.network/)+[Monk](https://monk.io) demo. The templates are published on [monkhub.io](https://monkhub.io) so that you can run them without pulling this repo.

Note that these templates are using official, unmodified Docker images from Polkadot and Acala.

These templates are for demonstration and development purposes and they are not intended for production use i.e. running secure validators for [Polkadot](https://polkadot.network/) or [Acala](https://acala.network/). While it is certainly possible with Monk, these templates are not implementing such functionality at this moment.

## Getting Monk

You'll need Monk installed on your machine to run these templates: [Getting Monk](https://docs.monk.io/docs/get-monk).

Read if you want to run a Monk cluster: [Creating a Monk Cluster](https://docs.monk.io/docs/monk-in-10#creating-a-monk-cluster).

Read the [Documentation](https://docs.monk.io/docs/).

## Usage

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

Run Polkadot+Acala collator parachain dev setup:

```
monk run parachain/system
monk run -t mytag parachain/system
```

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
