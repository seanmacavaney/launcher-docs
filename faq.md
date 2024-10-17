# Frequently Asked Questions

## How do I run a Jupyter Notebook?

[Launch a pod](launch) and select the "Notebook" mode.

Once it starts, you'll get a link to the running notebook server:

<img srcset="_static/images/open-notebook.png 2x" />

If you close the tab, don't worry. You can always access the notebook again through the link on the home page:

<img srcset="_static/images/open-notebook-home.png 2x" />

----------------------------------------------------------------------------------------------

## How do I connect to my pod with VS Code?

You can connect to your pods with [Visual Studio Code](https://code.visualstudio.com/) by following these steps:

**Step 1:** (First time only) Configure SSH access by following [this guide](ssh).

**Step 2:** (First time only) Install the [Remote SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh) VS Code extension.

**Step 3:** [Launch a pod](launch).

**Step 4:** Run "Connect to Host" in VS Code. When prompted, specify your pod as `[pod_name].ida` (e.g., `awesome-coo-23.ida`).

<img srcset="_static/images/vscode-connect-to-host.png 2x" />

----------------------------------------------------------------------------------------------

## How can I copy files to/from my pod?

You'll often need to copy files (code, results, figures, etc.) between your local machine and the IDA cluster.
You can do this using tools like [`scp`](https://linux.die.net/man/1/scp) and [`rsync`](https://linux.die.net/man/1/rsync) once you [enable SSH access](ssh). You should specify the server
as `[pod_name].ida`. Example for a single file using `scp`:

```bash
scp my_file.py awesome-coo-23.ida:nfs/my_file.py
```

Example for a directry using `rsync`:

```bash
rsync -rlz my_project/ awesome-coo-23.ida:nfs/my_project/
```

Remember that files copied to NFS directories will be persisted across pods (but have slower access times).
Meanwhile, files copied elsewhere will be lost when the pod is restarted, but are faster to access. Read more
[here](storage).

----------------------------------------------------------------------------------------------

## How do I connect to my pod with SSH?

See [this page](ssh).

----------------------------------------------------------------------------------------------

## (Advanced) How can I connect to a service on my pod?

Sometimes you may need to connect to a service that's running on your pod. For instance,
[TensorBoard](https://www.tensorflow.org/tensorboard) is a HTTP service that plots the
training progress of a neural network. You can connect to these services using SSH Tunneling.

**Step 1:** (First time only) Configure SSH access by following [this guide](ssh).

**Step 2:** Run `ssh [pod_name].ida  -N -L [local_port]:localhost:[remote_port]`

 - `[pod_name]` is the name of your running pod
 - `[remote_port]` is the port number that is running the service on your pod
 - `[local_port]` is the port number that you want to to access the service on your local machine.
 - (You can add `-f` if you want this to run in the background.)

**Step 3:** You can now access the service running on `[remote_port]` on your local machine through `localhost:[local_port]`.

----------------------------------------------------------------------------------------------

## How can update/correct this documentation?

You can help improve this documentation by submitting a pull request to
[this repository](https://github.com/seanmacavaney/launcher-docs).
