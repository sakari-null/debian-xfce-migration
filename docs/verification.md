# System Verification

The final stage of the migration was to verify that the required desktop components were still installed and that APT had no remaining automatically removable packages.

## Critical Package Verification

The following command was used to verify the main desktop components:

```bash
dpkg -l | grep -E '^ii  (kdeconnect|xfce4|thunar|lightdm)'
```

The verification confirmed that the following components remained installed:

| Component   | Version | Purpose                    |
| ----------- | ------: | -------------------------- |
| XFCE        |  4.20.1 | Desktop environment        |
| Thunar      |  4.20.2 | File manager               |
| LightDM     |  1.32.0 | Display manager            |
| KDE Connect | 25.04.2 | Mobile/desktop integration |

Additional XFCE components such as the panel, session manager, settings manager, terminal, power manager and plugins were also present.

## Package Cleanup Verification

The final APT simulation was:

```bash
sudo apt autoremove --purge -s
```

Result:

```text
Summary:
  Upgrading: 0, Installing: 0, Removing: 0, Not Upgrading: 0
```

This confirmed that no packages remained that APT considered automatically removable.

## LibreOffice Verification

During the cleanup, `libreoffice-qt5` was identified as an unused automatically installed package.

The Qt5 integration package was removed while the main LibreOffice installation remained installed.

This illustrates the importance of distinguishing an application from an optional integration package.

## Verification Principles

The final verification followed three principles:

1. **Check critical packages directly.**
2. **Review package-manager cleanup results.**
3. **Do not assume that successful package removal means the system is correct.**

The system was therefore checked both before and after cleanup.

## Final State

The system is now running XFCE as its desktop environment with:

* XFCE desktop environment
* Thunar file manager
* LightDM display manager
* KDE Connect
* LibreOffice
* No remaining APT autoremove candidates

The migration and cleanup were completed without removing the required desktop functionality.
