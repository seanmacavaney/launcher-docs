# SSH Access

Pods created through Launcher can be accessed using [SSH](https://linux.die.net/man/1/ssh).
This is a convenient way to connect to your pod without needing to open a web browser.
It also lets you:

 - Use tools like `scp` and `rsync` to copy files to and from your pod.
 - Connect to IDEs such as VSCode and JetBrains.
 - Connect to services running on your pod, such as TensorBoard.

## Setting up SSH Access

Launcher provides a step-by-step guide to configure SSH access to your pod under the "SSH Access" panel:

<img srcset="_static/images/ssh-configure.png 2x" />

First follow the provided instructions. If you run into problems, try the troubleshooting steps below.

## Troubleshooting SSH Access

**Make sure ssh is installed on your machine.** Try running the command `ssh --version` from your terminal.
If you see an error message, install SSH using your package manager. For example, on Ubuntu you can run:

```bash
sudo apt install openssh-client
```

Or on MacOS:

```bash
brew install openssh
```

**Make sure `socat` is installed on your machine.** Try running the command `socat -V` from your terminal.
If you see an error message, install `socat` using your package manager. For example, on Ubuntu you can run:

```bash
sudo apt install socat
```

Or on MacOS:

```bash
brew install socat
```

**Make sure you have an SSH key.** You can check if you have an SSH key by running:

```bash
ls ~/.ssh/id_*.pub
```

If you see a file named `id_rsa.pub` or `id_ed25519.pub`, you have an SSH key. If not, follow
[GitHub's Guide on Generating a new SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key).
