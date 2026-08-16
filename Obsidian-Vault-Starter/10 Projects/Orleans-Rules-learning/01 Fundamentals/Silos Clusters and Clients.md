---
project: Orleans-Rules-learning
type: concept
tags: [orleans, silo, cluster]
---
# Silos, Clusters, and Clients

## Silo
A silo is a process that hosts Orleans grain activations.

## Cluster
A cluster is a group of silos cooperating as one Orleans system.

## Client
An Orleans client calls grains but normally does not host grain activations.

## Container
A container is only a deployment boundary. It may run a silo, but Orleans concepts and container concepts are not the same thing.

Related:
- [[Orleans Mental Model]]
- [[Grain Activation and Location]]
- [[../05 Observability/DotNET Aspire with Orleans]]
