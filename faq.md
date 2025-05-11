# Frequently Asked Questions

## How do I run a Jupyter Notebook?

Launch a pod and select the "Notebook" mode.

Once it starts, you'll get a link to the running notebook server:

<img srcset="_static/images/open-notebook.png 2x" />

If you close the tab, don't worry. You can always access the notebook again through the link on the home page:

<img srcset="_static/images/open-notebook-home.png 2x" />

----------------------------------------------------------------------------------------------

## How do I connect to my pod with VS Code?

You can connect to your pods with [Visual Studio Code](https://code.visualstudio.com/) by following these steps:

**Step 1:** (First time only) Configure SSH access.

**Step 2:** (First time only) Install the [Remote SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh) VS Code extension.

**Step 3:** Launch a pod.

**Step 4:** Run "Connect to Host" in VS Code. When prompted, specify your pod as `[pod_name].os` (e.g., `awesome-coo-23.os`).

<img srcset="_static/images/vscode-connect-to-host.png 2x" />

----------------------------------------------------------------------------------------------

## How do I connect to my pod with JetBrains IDEs?

You can connect to your pods with [JetBrains](https://www.jetbrains.com/) by following these steps:

**Step 1:** (First time only) Configure SSH access.

**Step 2:** (First time only) Ensure you have the Remote Development Gateway plugin enabled.

**Step 3:** On the welcome screen, select "Remote Development" or go to "File | Remote Development" in the main menu.

**Step 4:** Under SSH Connection, click New Connection

**Step 5:** Specify the remote server connection parameters with:
- Username: `root`
- Host: `[pod-name].os`
- Tick Specify private key and navigate to the file where the private key is located. Usually is `home/USER/.ssh/id_*******`

**Step 6:** Choose IDE you wish to use and select your project directory

**Step 7:** Download and start IDE

----------------------------------------------------------------------------------------------

## How do I push/pull to GitHub from a pod?

You can push and pull to remote repositories from your pods once you perform the following setup:

**Step 1: Add your pod SSH keys to your GitHub account.**

First, copy your automatically-generated (public) SSH key, which can be found on the home page under "Pod SSH Key":

<img srcset="_static/images/ssh-key.png 3x" />

Then paste this into a new [GitHub SSH Key](https://github.com/settings/keys):

<img srcset="_static/images/github-ssh-key-0.png 4x" />

<img srcset="_static/images/github-ssh-key-1.png 4x" />


**Step 2: Configure your local git settings.**

Launch a pod that runs the following script (filling in your email address and name accordingly).

```bash
echo 'git config --global user.email "my-email-address@somewhere.com"' >> ~/nfs/launcher-start.sh
echo 'git config --global user.name "My Name"' >> ~/nfs/launcher-start.sh
```

These options configure `git` to set your name and email address on commits you make.

**All set!**

New pods that you launch will now be able to push and pull from your GitHub repositories.

----------------------------------------------------------------------------------------------

## How can I copy files to/from my pod?

You'll often need to copy files (code, results, figures, etc.) between your local machine and the IDA cluster.
You can do this using tools like [`scp`](https://linux.die.net/man/1/scp) and [`rsync`](https://linux.die.net/man/1/rsync) once you configure SSH access. You should specify the server
as `[pod_name].os`. Example for a single file using `scp`:

```bash
scp my_file.py awesome-coo-23.os:nfs/my_file.py
```

Example for a directry using `rsync`:

```bash
rsync -rlz my_project/ awesome-coo-23.os:nfs/my_project/
```

Remember that files copied to NFS directories will be persisted across pods (but have slower access times).
Meanwhile, files copied elsewhere will be lost when the pod is restarted, but are faster to access.

----------------------------------------------------------------------------------------------

## How do I automaticaly run a script each time a pod starts?

Your `~/nfs/launcher-start.sh` script will automatically run each time a pod is launched.

----------------------------------------------------------------------------------------------

## (Advanced) How can I connect to a service on my pod?

Sometimes you may need to connect to a service that's running on your pod. For instance,
[TensorBoard](https://www.tensorflow.org/tensorboard) is a HTTP service that plots the
training progress of a neural network. You can connect to these services using SSH Tunneling.

**Step 1:** (First time only) Configure SSH access.

**Step 2:** Run `ssh [pod_name].os  -N -L [local_port]:localhost:[remote_port]`

 - `[pod_name]` is the name of your running pod
 - `[remote_port]` is the port number that is running the service on your pod
 - `[local_port]` is the port number that you want to to access the service on your local machine.
 - (You can add `-f` if you want this to run in the background.)

**Step 3:** You can now access the service running on `[remote_port]` on your local machine through `localhost:[local_port]`.

----------------------------------------------------------------------------------------------

## How can update/correct this documentation?

You can help improve this documentation by submitting a pull request to
[this repository](https://github.com/seanmacavaney/launcher-docs).
