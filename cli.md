# Launcher CLI

The `launcher` command lets you launch and manage pods without the web GUI. It works on both the
IDA and OKD clusters and uses your normal `oc` login, so there is no separate password.

## Getting Started

Install it with pip:

```bash
$ pip install https://launcher.apps.os.dcs.gla.ac.uk/static/launcher_cli-0.1.2-py3-none-any.whl
```

Log in to the cluster as usual (this is needed about once a day), then trust the launcher's
certificate once:

```bash
$ oc login ...
$ launcher trust
launcher.apps.os.dcs.gla.ac.uk
  SHA-256 52:9b:3e:...
Trust this certificate? [y/N] y
```

To use the OKD cluster, pick its kubeconfig context: `launcher --context <context> list`.

## Launching a Pod

```bash
$ launcher launch
awesome-coo-27
✓ Pod Created: https://console.../pods/awesome-coo-27
✓ Pod Scheduled: idagpu-14
✓ Container Started
OKD Terminal:
  https://console.../pods/awesome-coo-27/terminal
SSH Access:
  ssh awesome-coo-27.os
```

Choose the hardware (`GPU-size`, e.g. `3090-lg`), timeout and name:

```bash
$ launcher launch 3090-md --timeout 2h --name my-pod
$ launcher notebook a6000-lg
$ launcher run 3090-md ./job.sh        # runs a script, then the pod ends
$ launcher run 3090-md ./job.sh --lr 0.01 --out "my run"   # everything after the script is passed to it as $1, $2, ...
```

Put launcher's own options (such as `--gpus` or `--name`) before the script name.

Request several GPUs with `--gpus` (up to 8, if your account allows it):

```bash
$ launcher launch a6000-lg --gpus 2
```

See what is free right now with `launcher available`, or `launcher available --gpus 2` for pods with two GPUs.

## Other Commands

| Command | What it does |
|---------|--------------|
| `launcher list` | list your pods |
| `launcher delete NAME...` | delete pods |
| `launcher note NAME "text"` | set the note shown next to a pod |
| `launcher extend NAME` | reset a pod's idle timer |
| `launcher logs NAME [-f]` | show logs (uses `oc`/`kubectl`) |
| `launcher exec NAME -- CMD` | run a command in a pod, or open a shell |
| `launcher whoami` | show the cluster, user and namespace in use |

Cognition pods are not supported; use the web GUI for those.

## Scripts and AI Agents

Every command accepts `--json`. Output is then exactly one JSON document on stdout, nothing ever
prompts, and failures are a JSON object `{"error": ..., "message": ...}` with a non-zero exit code
(3 = not logged in, 4 = no such pod, 5 = cannot connect or certificate not trusted, 6 = pod did
not become ready). The first connection under `--json` fails with the certificate's fingerprint;
run `launcher trust --yes` once you have checked it, or pass `--trust-cert`.

```bash
$ launcher available --json | jq '.types[] | select(.allowed and .available > 0)'
$ launcher run --json 3090-md ./job.sh | jq -r .name
```
