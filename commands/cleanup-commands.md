# Cleanup Commands

This document contains the main commands used during the Debian desktop migration and explains their purpose.

## Package Verification

Check whether the critical desktop components are installed:

```bash
dpkg -l | grep -E '^ii  (kdeconnect|xfce4|thunar|lightdm)'
```

This was used to verify that XFCE, Thunar, LightDM and KDE Connect remained installed.

## APT Simulation

Before performing automatic cleanup, APT was run in simulation mode:

```bash
sudo apt autoremove --purge -s
```

The `-s` option simulates the operation without changing the system.

This allows the proposed package removals to be reviewed before execution.

## Targeted Package Removal

Obsolete LXQt-related packages were removed explicitly when their role in the old environment had been identified.

For example:

```bash
sudo apt purge \
  libmenu-cache3 \
  libmenu-cache-bin \
  libfm-extra4t64 \
  libfm-qt-l10n \
  lximage-qt-l10n \
  pcmanfm-qt-l10n
```

The purpose of targeted removal was to avoid unnecessarily removing unrelated software.

## Automatic Cleanup

After the targeted cleanup, APT was used to remove dependencies that were no longer required:

```bash
sudo apt autoremove --purge
```

The operation was only performed after reviewing the simulated package list.

## Final Verification

The final state was verified with:

```bash
sudo apt autoremove --purge -s
```

The final result was:

```text
Summary:
  Upgrading: 0, Installing: 0, Removing: 0, Not Upgrading: 0
```

This confirmed that there were no remaining packages that APT considered automatically removable.

## Verification of Essential Packages

The final installed package set was checked again:

```bash
dpkg -l | grep -E '^ii  (kdeconnect|xfce4|thunar|lightdm)'
```

This confirmed that the required desktop environment and applications were still installed.

## Command-Line Tools Used

The project primarily used standard Debian/Linux tools:

| Tool        | Purpose                                                 |
| ----------- | ------------------------------------------------------- |
| `apt`       | Package installation, removal and dependency management |
| `apt-cache` | Package dependency investigation                        |
| `apt-mark`  | Package installation-state management                   |
| `dpkg`      | Installed package verification                          |
| `grep`      | Filtering package information                           |
| `git`       | Version control and project documentation               |

## Safety Approach

The main principle throughout the cleanup was:

> **Inspect first, simulate second, modify third, verify last.**

This approach reduces the risk of accidentally removing required desktop components during a system migration.
