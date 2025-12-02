# Hardware

This page provides information about the various hardware provided on the cluster.

## GPU

You can request a GPU with your pod, which can accelerate certain workloads. Each GPU has trade-offs
in terms of speed and memory capacity.

| GPU Model |    Memory | Relative Speed | Tensor Cores | CUDA Cores | Base Clock | Boost Clock | Year |
|-----------|----------:|---------------:|-------------:|-----------:|-----------:|------------:|-----:|
| `1080ti`  | **11 GB** |        **0.2** |            0 |      3,584 |    1.4 GHz |     1.5 GHz | 2017 |
| `titan`   | **24 GB** |        **1.3** |          576 |      4,608 |    1.3 GHz |     1.7 GHz | 2018 |
| `a5000`   | **24 GB** |        **1.4** |          256 |      8,192 |    1.1 GHz |     1.6 GHz | 2021 |
| `3090`    | **24 GB** |        **1.4** |          328 |     10,496 |    1.3 GHz |     1.6 GHz | 2020 |
| `4090`    | **24 GB** |        **2.4** |          512 |     16,384 |    2.2 GHz |     2.5 GHz | 2022 |
| `5000ada` | **32 GB** |        **1.5** |          400 |     12,800 |    1.1 GHz |     2.5 GHz | 2023 |
| `a6000`   | **48 GB** |        **1.6** |          336 |     10,752 |    1.4 GHz |     1.8 GHz | 2020 |
| `l40s`    | **48 GB** |        **2.1** |          ??? |     ??,??? |    ?.? GHz |     ?.? GHz | ???? |
| `6000bw`  | **96 GB** |        **4.6** |          752 |     24,064 |    1.6 GHz |     2.6 GHz | 2025 |
| `h100`    | **96 GB** |        **5.4** |          ??? |     ??,??? |    ?.? GHz |     ?.? GHz | ???? |

<!--
| `2080ti`  | **11 GB** |       **?.??** |          544 |      4,352 |   1350 MHz |    1545 MHz | 2018 |
| `5090`    | **32 GB** |       **?.??** |          512 |     16,384 |   2010 MHz |    2410 MHz | 2025 |
-->

 - **Memory** is the amount of memory available on the GPU. This is important for large models, as
   they may not fit into the memory of smaller GPUs. Note that the GPU's memory is separate from the
   CPU's memory.
 - **Relative Speed** is an estimate of the speed of the GPU. The value is estimated using a benchmark
   of forward and backward passes over a transformer network at various batch sizes and sequence lengths.
   This represnts a typical use case for text processing, but may not be representative of other workloads.
 - **Tensor Cores** are specialized cores designed to accelerate deep learning tasks. They are particularly
   useful for matrix operations, which are common in deep learning workloads.
 - **CUDA Cores** are the general-purpose parallel processing units on the GPU. A higher number of
   CUDA cores generally means more parallel processing power.
 - **Base Clock** the minimum guaranteed speed that the GPU will run at under normal conditions. It’s the
   baseline performance level the card should maintain without overheating or drawing too much power.
 - **Boost Clock** the maximum speed the GPU can temporarily reach under optimal conditions, such as
   sufficient cooling and available power. It’s not always sustained and varies depending on workload,
   temperature, and power limits.

You can request multiple GPUs under "Advanced Options". However, it can be difficult to use multiple
GPUs effectively, so please carefully check that both GPUs are in use when you request multiple.

<!--
Relative speeds using <https://github.com/seanmacavaney/gpu-benchmark>

1080ti
    Pod                     : smiling-crow-98
    Timestamp               : 2025-10-09T11:51:35.421524
    Pytorch version         : 2.6.0+cu124
    CUDA version            : 12.4
    Transformers version    : 4.51.3
    GPU                     : NVIDIA GeForce GTX 1080 Ti
    0.21

titan
    Pod                     : grotesque-wombat-81
    Timestamp               : 2025-10-09T11:51:28.654079
    Pytorch version         : 2.6.0+cu124
    CUDA version            : 12.4
    Transformers version    : 4.51.3
    GPU                     : NVIDIA TITAN RTX
    1.28

a5000
    Pod                     : troubled-okapi-57
    Timestamp               : 2025-10-09T11:51:29.587613
    Pytorch version         : 2.6.0+cu124
    CUDA version            : 12.4
    Transformers version    : 4.51.3
    GPU                     : NVIDIA RTX A5000
    1.37

3090
    Pod                     : cloudy-silkworm-63
    Timestamp               : 2025-10-09T11:51:25.201872
    Pytorch version         : 2.6.0+cu124
    CUDA version            : 12.4
    Transformers version    : 4.51.3
    GPU                     : NVIDIA GeForce RTX 3090
    1.39

4090
    Pod : 4090-benchmark
    Timestamp : 2025-10-09T13:13:20.383442
    Pytorch version : 2.6.0+cu124
    CUDA version : 12.4
    Transformers version : 4.51.3
    GPU : NVIDIA GeForce RTX 4090
    2.36

5000ada
    Pod : 5000ada-benchmark
    Timestamp : 2025-10-09T13:13:08.834081
    Pytorch version : 2.6.0+cu124
    CUDA version : 12.4
    Transformers version : 4.51.3
    GPU : NVIDIA RTX 5000 Ada Generation
    1.52

a6000
    Pod                     : tame-ostrich-11
    Timestamp               : 2025-10-09T11:51:25.780812
    Pytorch version         : 2.6.0+cu124
    CUDA version            : 12.4
    Transformers version    : 4.51.3
    GPU                     : NVIDIA RTX A6000
    1.65

l40s
    Pod                     : login1.cognition.gla.alces.network
    Timestamp               : 2025-12-02T17:27:05.391800
    Pytorch version         : 2.9.1+cu130
    CUDA version            : 13.0
    Transformers version    : 4.51.3
    GPU                     : NVIDIA L40S
    2.13

6000bw
    Pod                     : N/A
    Timestamp               : 2025-12-01T18:41:09.199349
    Pytorch version         : 2.9.1+cu130
    CUDA version            : 13.0
    Transformers version    : 4.51.3
    GPU                     : NVIDIA RTX PRO 6000 Blackwell Workstation Edition
    4.59

h100
    Pod                     : login1.cognition.gla.alces.network
    Timestamp               : 2025-12-02T17:31:09.214118
    Pytorch version         : 2.9.1+cu130
    CUDA version            : 13.0
    Transformers version    : 4.51.3
    GPU                     : NVIDIA H100 NVL
    5.38
-->

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
CPU your pod has access to by checking the [Cluster Usage Dashboard](http://usage.apps.os.dcs.gla.ac.uk/) or by running
`cat /proc/cpuinfo | grep "model name" | head -1`. Below is a list of the CPUs currently on the cluster.

| Model          | Vendor | Base Clock  | Boost Clock | Threads | Year |
|----------------|--------|------------:|------------:|--------:|-----:|
| `xeon-2620`    | Intel  | **2.1 GHz** |     3.0 GHz |      32 | 2016 |
| `ryzen-3975wx` | AMD    | **3.5 GHz** |     4.7 GHz |      64 | 2020 |
| `xeon-6244`    | Intel  | **3.6 GHz** |     4.4 GHz |      32 | 2019 |
| `xeon-5222`    | Intel  | **3.8 GHz** |     3.9 GHz |      16 | 2019 |
| `ryzen-5965wx` | AMD    | **3.8 GHz** |     4.5 GHz |      48 | 2022 |
| `ryzen-3955wx` | AMD    | **3.9 GHz** |     4.3 GHz |      32 | 2020 |
| `ryzen-5955wx` | AMD    | **4.0 GHz** |     4.5 GHz |      32 | 2022 |
| `ryzen-7950x`  | AMD    | **4.5 GHz** |     5.7 GHz |      32 | 2022 |
