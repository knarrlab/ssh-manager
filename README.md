# ssh-manager

A simple SSH connection manager that uses tmux, fzf, and tailscale to provide a quick and easy way to manage and connect to your SSH hosts.

## Features

- **Quickly connect to SSH hosts:** Uses `fzf` to provide a fuzzy-searchable menu of your SSH hosts.
- **Tmux integration:** Opens each SSH session in a new tmux window, keeping your sessions organized.
- **Tailscale integration:** Automatically sync your Tailscale hosts to your SSH config.
- **Easy configuration:** Add, edit, and manage your SSH hosts with simple commands.

## Requirements

- `tmux`
- `fzf`
- `awk`
- `sed`
- `tailscale` (optional, for syncing hosts)
- `bash`
- `ssh`

## Installation

1.  **Download the script and make it executable:**
    ```bash
    git clone https://github.com/knarrlab/ssh-manager.git
    cd ssh-manager
    chmod +x ssh-manager
    ```
    
2.  **Run the installer:**
    ```bash
    ./ssh-manager --install
    ```
    The script will prompt you to install it to `/usr/local/bin/ssh-manager`. If you abort the installation, you will be prompted to manually delete the downloaded script's directory.

## Uninstallation

To uninstall `ssh-manager`, run the following command:

```bash
sudo ssh-manager --uninstall
```
The script will prompt you to confirm the uninstallation. You will also be asked separately if you wish to delete your `~/.ssh/config` file. **Be aware that deleting this file is irreversible and will remove all your SSH host configurations.**

## Usage

### Running the manager

To start the ssh-manager, simply run the script:

```bash
ssh-manager
```

This will start a new tmux session (or attach to an existing one) and display a menu of your SSH hosts.

### Syncing hosts from Tailscale

To sync your Tailscale hosts to your SSH config, run:

```bash
ssh-manager --sync
```

This will add any new Tailscale hosts to your `~/.ssh/config` file.

### Adding a new host

To manually add a new host, run:

```bash
ssh-manager --add
```

You will be prompted to enter the host's alias, hostname/IP, user, and port.

### Editing the SSH config

To edit your SSH config file (`~/.ssh/config`), run:

```bash
ssh-manager --edit
```

This will open your SSH config file in your default editor (`$EDITOR`).

## Configuration

The script uses the `~/.ssh/config` file to store your SSH hosts. The format is the standard SSH config format.

### Example

```
Host my-server
  HostName 1.2.3.4
  User myuser
  Port 2222
```

## How it works

The script first checks if a tmux session named `ssh` is running. If not, it creates one. It then creates a new window named `manager` and runs the `ssh-manager` script inside it.

The script then builds a menu of your SSH hosts from your `~/.ssh/config` file and pipes it to `fzf`. When you select a host, a new tmux window is created for that host, and an SSH session is started.

You can detach from the tmux session at any time by selecting the `!!DETACH!!` option in the menu.
