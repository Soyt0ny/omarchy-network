# Omarchy Network Plugin

A public-installable Omarchy bar widget for Wi-Fi and network management. It
preserves the standard network panel and adds a per-interface selector,
user-defined adapter aliases, explicit connect/disconnect actions, and
monitor-mode controls for eligible TP-Link/RTL8188EUS adapters.

## Installation

Install from the plugin repository URL:

```bash
omarchy plugin add https://github.com/soytony/omarchy-network.git --enable
```

If the plugin is already installed, update it with:

```bash
omarchy plugin update io.github.soytony.network --yes
```

Restart the shell if the widget does not appear immediately:

```bash
omarchy restart shell
```

## Uninstallation

Disable and remove the installed plugin:

```bash
omarchy plugin disable io.github.soytony.network
omarchy plugin remove io.github.soytony.network --yes
```

The helper stores adapter aliases in
`${XDG_CONFIG_HOME:-$HOME/.config}/omarchy/network/interfaces.tsv`; remove that
file separately if you also want to delete saved aliases.

## Permissions and security

The widget invokes NetworkManager tools (`nmcli`) and the bundled helper.
Changing Wi-Fi mode uses `pkexec` for the required `ip` and `iw` operations,
so the desktop policy agent may ask for authentication. Do not run the helper
from an untrusted checkout: it can disconnect interfaces, change NetworkManager
autoconnect settings, and change wireless mode. Passwords are sent over stdin
to NetworkManager and are not placed in process arguments.

## Monitor-mode prerequisites

Monitor mode is offered only for adapters identified as TP-Link TL-WN722N
variants or RTL8188EUS devices using the `rtl8188eu` or `rtl8xxxu` driver. The
adapter must be visible to NetworkManager, and `iw`, `ip`, `nmcli`, `udevadm`,
and a working polkit agent must be available. Monitor mode disconnects the
interface and disables autoconnect while active; switching back to managed
mode reconnects a saved Wi-Fi profile.

## Validation

From the plugin directory:

```bash
omarchy plugin validate .
bash -n wifi-helper
node --check Model.js
! grep -nE '/home/|/root/' manifest.json Panel.qml Model.js wifi-helper
```

The plugin uses its own `wifi-helper` path resolved relative to `Panel.qml`,
and runtime state follows `XDG_CONFIG_HOME`.
