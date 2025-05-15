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

## GPU

You can request a GPU with your pod, which can accelerate certain workloads. Each GPU has trade-offs
in terms of speed and memory capacity.

| GPU Model |    Memory | Relative Speed | Tensor Cores | CUDA Cores | Base Clock | Boost Clock | Year |
|-----------|----------:|---------------:|-------------:|-----------:|-----------:|------------:|-----:|
| `1080ti`  | **11 GB** |       **0.82** |            0 |      3,584 |   1480 MHz |    1582 MHz | 2017 |
| `3090`    | **24 GB** |       **2.42** |          328 |     10,496 |   1395 MHz |    1695 MHz | 2020 |
| `titan`   | **24 GB** |       **2.69** |          576 |      4,608 |   1350 MHz |    1770 MHz | 2018 |
| `a5000`   | **24 GB** |       **3.02** |          256 |      8,192 |   1170 MHz |    1695 MHz | 2021 |
| `4090`    | **24 GB** |       **5.57** |          512 |     16,384 |   2235 MHz |    2520 MHz | 2022 |
| `5000ada` | **32 GB** |       **3.77** |          400 |     12,800 |   1155 MHz |    2550 MHz | 2023 |
| `a6000`   | **48 GB** |       **3.05** |          336 |     10,752 |   1410 MHz |    1860 MHz | 2020 |

<!--
| `2080ti`  | **11 GB** |       **?.??** |          544 |      4,352 |   1350 MHz |    1545 MHz | 2018 |
| `5090`    | **32 GB** |       **?.??** |          512 |     16,384 |   2010 MHz |    2410 MHz | 2025 |
| `6000bw`  | **96 GB** |       **?.??** |          752 |     24,064 |   1590 MHz |    2617 MHz | 2025 |
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
    Pod : shy-butterfly-52
    Timestamp : 2025-05-06T13:39:30.384809
    Pytorch version : 2.5.1+cu124
    CUDA version : 12.4
    Transformers version : 4.47.1
    GPU : NVIDIA GeForce GTX 1080 Ti
    0.82

3090
    Pod : joyous-snake-29
    Timestamp : 2025-05-06T13:40:02.352003
    Pytorch version : 2.5.1+cu124
    CUDA version : 12.4
    Transformers version : 4.47.1
    GPU : NVIDIA GeForce RTX 3090
    2.42

titan
    Pod : good-jaguar-21
    Timestamp : 2025-05-06T13:39:44.452430
    Pytorch version : 2.5.1+cu124
    CUDA version : 12.4
    Transformers version : 4.47.1
    GPU : NVIDIA TITAN RTX
    2.69

4090
    Pod : nutty-bat-19
    Timestamp : 2025-05-06T14:00:50.318387
    Pytorch version : 2.5.1+cu124
    CUDA version : 12.4
    Transformers version : 4.47.1
    GPU : NVIDIA GeForce RTX 4090
    5.57

5000ada
    Pod : annoying-goat-10
    Timestamp : 2025-05-06T16:46:56.506455
    Pytorch version : 2.5.1+cu124
    CUDA version : 12.4
    Transformers version : 4.47.1
    GPU : NVIDIA RTX 5000 Ada Generation
    3.77

a6000
    Pod : mysterious-frog-79
    Timestamp : 2025-05-07T04:41:00.165299
    Pytorch version : 2.5.1+cu124
    CUDA version : 12.4
    Transformers version : 4.47.1
    GPU : NVIDIA RTX A6000
    3.05

a5000
    Pod : talented-hawk-22
    Timestamp : 2025-05-09T15:35:06.434332
    Pytorch version : 2.5.1+cu124
    CUDA version : 12.4
    Transformers version : 4.47.1
    GPU : NVIDIA RTX A5000
    3.02
-->
