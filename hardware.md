# Hardware

This page provides information about the various hardware provided on the cluster.

## CPUs

The specification of each node's CPU depends on what was available when the node was added to the cluster.

| CPU Model      | Vendor | Number of Cores | Clock Speed |
|----------------|--------|-----------------|-------------|
| `xeon-5222`    | Intel  | 16              | 3.8 GHz     |
| `ryzen-3975wx` | AMD    | 64              | 3.5 GHz     |
| `ryzen-3955wx` | AMD    | 32              | 3.9 GHz     |
| `ryzen-5965wx` | AMD    | 64              | 3.8 GHz     |
| `ryzen-7950x`  | AMD    | 32              | 4.5 GHz     |

## GPUs

All nodes have multiple GPUs, each of which provide trade-offs in terms of speed and memory capacity.

| GPU Model | Memory | Clock Speed | Relative Speed (approx) |
|-----------|--------|-------------|-------------------------|
| `titan`   | 24 GB  |  837 MHz    | 0.42                    |
| `a5000`   | 24 GB  | 1170 MHz    | 0.51                    |
| `3090`    | 24 GB  | 1395 MHz    | 0.60                    |
| `4090`    | 24 GB  | 2235 MHz    | 0.86                    |
| `5000ada` | 32 GB  | 1155 MHz    | 0.69                    |
| `a6000`   | 48 GB  | 1410 MHz    | 0.51                    |

<!-- Relative Speeds from: https://technical.city/en/video/Quadro-RTX-A6000-vs-RTX-A5000 -->
