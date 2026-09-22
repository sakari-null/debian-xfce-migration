# Migration and Cleanup Process

## 1. Initial Situation

The Debian 13 system had previously been used with LXQt and later transitioned to XFCE.

Both desktop environments had left packages and dependencies installed on the system. The goal was to remove the obsolete LXQt components without affecting the working XFCE environment.

The following components were considered essential and had to remain functional:

* XFCE
* Thunar
* LightDM
* KDE Connect
* LibreOffice

The cleanup was therefore performed incrementally, with package removal simulated before potentially destructive operations.

## 2. Verifying Important Packages

Before continuing with the cleanup, the installed packages for the essential components were checked:

```bash
dpkg -l | grep -E '^ii  (kdeconnect|xfce4|thunar|lightdm)'
```

This confirmed that XFCE, Thunar, LightDM and KDE Connect were installed.

Examples of verified packages included:

```text
kdeconnect
kdeconnect-libs
lightdm
lightdm-gtk-greeter
thunar
xfce4
xfce4-session
xfce4-settings
xfce4-panel
xfce4-terminal
```

This verification provided a baseline before further cleanup.

## 3. Identifying LXQt Components

The system contained a number of packages associated with LXQt and its applications.

Examples included:

```text
lxqt-session
lxqt-panel
lxqt-config
lxqt-globalkeys
lxqt-powermanagement
lxqt-runner
lxqt-policykit
lxqt-theme-*
pcmanfm-qt
lximage-qt
lxqt-archiver
```

These packages were no longer required after moving to XFCE.

Instead of removing unrelated Qt libraries globally, the cleanup focused on packages that were clearly associated with the previous LXQt environment.

## 4. Simulating Package Removal

APT's simulation mode was used before applying package removal:

```bash
sudo apt autoremove --purge -s
```

The `-s` option allowed the proposed changes to be reviewed without modifying the system.

This was particularly useful because the initial autoremove list contained both obsolete desktop components and applications that might still be useful.

The proposed removals were therefore reviewed rather than blindly accepted.

## 5. Separating Applications from Obsolete Dependencies

The autoremove results included several applications that were not inherently part of LXQt.

Examples included:

```text
mpv
yt-dlp
smplayer
qmmp
gucharmap
hexchat
meteo-qt
```

These were treated differently from obsolete LXQt components.

This distinction prevented the cleanup from becoming an unnecessary removal of useful software.

## 6. Removing Remaining Obsolete Components

After the LXQt components had been removed, additional packages associated with the old file-management and menu infrastructure were identified.

Examples
