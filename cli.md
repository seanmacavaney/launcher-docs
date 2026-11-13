# Launcher CLI

The `launcher` command lets you launch and manage pods without the web GUI. It works on both the
IDA and OKD clusters and uses your normal `oc` login, so there is no separate password.

## Getting Started

### 1. Install the OpenShift CLI (`oc`)

The launcher uses `oc` to log you in. Download it from the
[Command Line Tools page](https://console-openshift-console.apps.os.dcs.gla.ac.uk/command-line-tools)
of the OpenShift console (or the **?** menu at the top right of the console), unpack it, and put the
`oc` binary somewhere on your `PATH`. Check that it works:

```bash
$ oc version --client
```

### 2. Install `launcher`

You need Python 3.8 or newer. Install the CLI with pip (a virtual environment or `pip install --user`
is fine):

```bash
$ pip install https://launcher.apps.os.dcs.gla.ac.uk/static/launcher_cli-0.1.2-py3-none-any.whl
$ launcher --version
```

If you get `command not found`, pip put `launcher` in a directory that is not on your `PATH`
(often `~/.local/bin`). Add that directory to your `PATH`, or run it with `python -m launcher_cli`.

### 3. Log in (about once a day)

1. Open the [OpenShift console](https://console-openshift-console.apps.os.dcs.gla.ac.uk/) and sign in.
2. Click your name at the top right, then **Copy login command**, then **Display Token**.
3. Copy the `oc login --token=... --server=https://api.os.dcs.gla.ac.uk:6443` line and run it in
   your terminal.

The token expires after about a day. When `launcher` says you are not logged in, repeat these steps.
Keep the token private: anyone who has it can use your account.

Check that everything is connected:

```bash
$ launcher whoami
```

### 4. Trust the launcher's certificate (once)

The launcher uses a self-signed certificate. The first time you connect, pin it:

```bash
$ launcher trust
launcher.apps.os.dcs.gla.ac.uk
  SHA-256 52:9b:3e:...
Trust this certificate? [y/N] y
```

After that, `launcher` accepts only that exact certificate. If it ever changes, you get an error
until you run `launcher trust` again.

### Using the OKD cluster

The OKD cluster has its own login. Get its login command from the
[OKD console](https://console-openshift-console.apps.okd.dcs.gla.ac.uk/) the same way and run it.
`oc login` switches your current context to the cluster you just logged in to, and `launcher`
follows that context. To see your contexts and switch between them:

```bash
$ oc config get-contexts
$ oc config use-context <context-name>
```

You can also pick a context for a single command: `launcher --context <context-name> list`.

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

Pod types are written `GPU-size`, e.g. `cpu-xs`, `3090-md` or `a6000-lg` (see
[Hardware](hardware.md)). With no type, `launcher launch` starts a small CPU-only pod (`cpu-xs`).
Use `launcher available` to see which types you can use and how many could start right now.

A pod is deleted once it has been idle for its timeout: `10m` (the default) or `2h`. Run
`launcher extend NAME` to reset the timer. Anything you want to keep must be saved outside the
pod's `/tmp`.

Choose the hardware, timeout and name:

```bash
$ launcher launch 3090-md --timeout 2h --name my-pod
$ launcher notebook a6000-lg
$ launcher run 3090-md ./job.sh        # runs a script, then the pod ends
$ launcher run 3090-md ./job.sh --lr 0.01 --out "my run"   # everything after the script is passed to it as $1, $2, ...
```

Put launcher's own options (such as `--gpus` or `--name`) before the script name. The script can
be a file, script text in quotes, or `-` to read it from stdin. It runs in bash, and the pod ends
when the script finishes. Use `launcher logs NAME -f` to follow its output.

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

`launcher exec NAME` with no command opens a bash shell in the pod. Run `launcher <command> --help`
for every option.

Cognition pods are not supported; use the web GUI for those.

## A Typical Session

```bash
$ launcher available                  # what can start right now?
$ launcher launch 3090-md -t 2h       # wait for the pod, print how to connect
$ launcher exec awesome-coo-27        # open a shell in it (or use the SSH command printed)
$ launcher list                       # your pods and how long each has been idle
$ launcher delete awesome-coo-27      # free the GPU for others when you are done
```

Please delete pods you no longer need, and use one GPU unless you really need more.

## Troubleshooting

| Message | What to do |
|---------|------------|
| not logged in / token rejected | Your `oc` token expired. Repeat step 3 (Log in) above. |
| certificate not trusted | Run `launcher trust`. |
| cannot reach the launcher | Check your connection; off campus, try the university VPN. |
| no such pod | Check the name with `launcher list`. The pod may have hit its idle timeout. |
| pod did not become ready | Probably no free hardware of that type. Try another type from `launcher available`. |
| talking to the wrong cluster | Run `launcher whoami`, then `oc config use-context` to switch. |

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
