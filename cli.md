# Launcher CLI

<div class="alert alert-warning">
  The CLI is still being ported over to the new cluster. Please use the web GUI for now.
</div>

<!--
The Launcher Command Line Interface (CLI) allows you to run launcher commands without the web GUI.

## Getting Started

You can install the Launcher CLI using the following command:

```bash
$ pip install http://launcher.ida.dcs.gla.ac.uk/static/ida-0.0.1-py3-none-any.whl
```

You'll also need to log in using your cluster credentials:

```bash
$ ida login
Cluster Username: myusername
Cluster Password: 
```

Tip: you can use `ida login --save` to avoid re-entering your credentials every day.

## Launching a Pod

You can launch a pod using the `ida launch` command:

```bash
$ ida launch
awesome-coo-27
✅ Pod Created: https://console.ida.dcs.gla.ac.uk/k8s/ns/myusername/pods/awesome-coo-27
✅ Pod Scheduled: idagpu-14.dcs.gla.ac.uk
✅ Container Started             
OKD Terminal:
  https://console.ida.dcs.gla.ac.uk/k8s/ns/myusername/pods/awesome-coo-27/terminal
SSH Access:
  ssh awesome-coo-27.ida
```

You can also specify the node type, timeout, and name. See `ida launch --help` for more details.

```bash
$ ida launch --help
usage: ida launch [-h] [--timeout t] [--name n] [type]

positional arguments:
  type               [default=cpu-xs] Hardware specification of the pod, e.g., cpu-md or 3090-lg.

optional arguments:
  -h, --help         show this help message and exit
  --timeout t, -t t  [default=10m] Auto-timeout limit, either 10m or 2h.
  --name n, -n n     Name of the pod. If omitted, auto-generates a name.
```

## Other Commands

There are other commands available too. You can check them out with `ida --help`:

```bash
$ ida --help
usage: ida [-h] {login,logout,available,launch,run,notebook,delete,list} ...

positional arguments:
  {login,logout,available,launch,run,notebook,delete,list}
    available           Prints the number of available pod types.
    launch              Launches a new pod in console mode.
    run                 Launches a new pod in script mode.
    notebook            Launches a new pod in notebook mode.
    delete              Deletes the specified pods.
    list                Lists your pods.

optional arguments:
  -h, --help            show this help message and exit
```
-->
