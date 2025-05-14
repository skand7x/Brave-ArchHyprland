# Full Guide: Set Up GNOME Keyring for Brave on Arch + Hyprland + ly

## ✅ Step 1: Install Required Packages

```
sudo pacman -S gnome-keyring libsecret seahorse
```

- gnome-keyring: Secure storage daemon for passwords and secrets.

- libsecret: Library that lets apps like Brave access the keyring.

- seahorse: Optional GUI to view/edit keyring contents (can be helpful).

## ✅ Step 2: Enable GNOME Keyring PAM Integration for Ly

```
sudo nano /etc/pam.d/ly
```

- Add these lines at the end of the file:
```
auth       optional     pam_gnome_keyring.so
session    optional     pam_gnome_keyring.so auto_start
```

## ✅ Step 3: Launch gnome-keyring-daemon in Your Hyprland Session

```
mkdir -p ~/.config/hypr
nano ~/.config/hypr/autostart.sh
```

```
#!/bin/bash

# Start GNOME Keyring Daemon
eval $(/usr/bin/gnome-keyring-daemon --start)
export SSH_AUTH_SOCK

```

- Make it executable
```
chmod +x ~/.config/hypr/autostart.sh
```

- Edit your ~/.config/hypr/hyprland.conf and add under exec-once

```
exec-once = ~/.config/hypr/autostart.sh
```

## ✅ Step 4: Make Brave Use GNOME Keyring

```
nano /usr/share/applications/brave-browser.desktop 
```

- Find the Exec=

```
Exec=brave --password-store=gnome %U
```

