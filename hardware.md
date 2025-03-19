# Hardware

This page provides information about the various hardware provided on the cluster.

## CPU and Memory

When selecting a pod to launch, you can select the amount of CPU and Memory resoures allocated to your pod through
the "size" of the pod (xs-xxl). Try to select a size that is appropriate for your workload to ensure that there
are enough resources for everybody.

| Size                      | CPU Cores    | Memory     |
|---------------------------|-------------:|-----------:|
| `xs` (Extra-Small)        |  1 CPU Core  |   1 GB RAM |
| `sm` (Small)              |  2 CPU Cores |   8 GB RAM |
| `md` (Medium)             |  2 CPU Cores |  16 GB RAM |
| `lg` (Large)              |  4 CPU Cores |  32 GB RAM |
| `xl` (Extra-Large)        |  8 CPU Cores |  64 GB RAM |
| `xxl` (Extra-Extra-Large) | 16 CPU Cores | 128 GB RAM |


Each node's CPU model depends on what was available when the node was added to the cluster. You can check which
CPU your pod has access to by checking the [Cluster Usage Dashboard](http://usage.ida.dcs.gla.ac.uk/) or by running
`cat /proc/cpuinfo | grep "model name" | head -1`. Below is a list of the CPUs currently on the cluster.


| CPU Model      | Vendor | Number of Cores | Clock Speed |
|----------------|--------|----------------:|------------:|
| `xeon-5222`    | Intel  | 16              | 3.8 GHz     |
| `ryzen-3975wx` | AMD    | 64              | 3.5 GHz     |
| `ryzen-3955wx` | AMD    | 32              | 3.9 GHz     |
| `ryzen-5965wx` | AMD    | 64              | 3.8 GHz     |
| `ryzen-7950x`  | AMD    | 32              | 4.5 GHz     |

## GPU

You can request a GPU with your pod, which can accelerate certain workloads. Each GPU has trade-offs
in terms of speed and memory capacity.

| GPU Model | Memory | Clock Speed | CUDA Cores | Tensor Cores    | Relative Speed (approx) |
|-----------|-------:|------------:|-----------:|----------------:|------------------------:|
| `titan`   | 24 GB  |  837 MHz    |  4,608     | 576             | 0.42                    |
| `a5000`   | 24 GB  | 1170 MHz    |  8,192     | 256             | 0.51                    |
| `3090`    | 24 GB  | 1395 MHz    | 10,496     |  92             | 0.60                    |
| `4090`    | 24 GB  | 2235 MHz    | 16,384     | 512             | 0.86                    |
| `5000ada` | 32 GB  | 1155 MHz    | 12,800     | 400             | 0.69                    |
| `a6000`   | 48 GB  | 1410 MHz    | 10,752     |  84             | 0.51                    |

You can request multiple GPUs under "Advanced Options". However, it can be difficult to use multiple
GPUs effectively, so please carefully check that both GPUs are in use when you request multiple.

<!-- Relative Speeds from: https://technical.city/en/video/Quadro-RTX-A6000-vs-RTX-A5000 -->
