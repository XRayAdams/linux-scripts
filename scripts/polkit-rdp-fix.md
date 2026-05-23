# Configure Polkit for Fedora to avoid asking for password via RDP session to update metadata

If you use RDP to connect to Fedora, you might see too many, and I mean it, a way too many, popups asking for a password to update metadata. Ubuntu does not do that, but Fedora do. 

To fix it, create a file named `00-local.rules` in `/etc/polkit-1/rules.d` folder and paste this content. Remember, this folder required to use sudo top create a file:

```
polkit.addRule(function(action, subject) {
    if ((action.id == "org.freedesktop.Flatpak.app-install" ||
         action.id == "org.freedesktop.Flatpak.runtime-install"||
         action.id == "org.freedesktop.Flatpak.app-uninstall" ||
         action.id == "org.freedesktop.Flatpak.runtime-uninstall" ||
         action.id == "org.freedesktop.Flatpak.modify-repo" ||
         action.id == "org.freedesktop.Flatpak.metadata-update" ||
         action.id == "org.projectatomic.rpmostree1.repo-refresh" ||
         action.id == "org.projectatomic.rpmostree1.upgrade") &&
        subject.active == true &&
        subject.isInGroup("wheel")) {
            return polkit.Result.YES;
    }

    return polkit.Result.NOT_HANDLED;
});
```

You can use this command to do it

```bash
sudo nano /etc/polkit-1/rules.d/00-local.rules
```

After that restart polkit or reboot your system

```bash
sudo systemctl restart polkit.service
```

Why RDP sessions is askind for a password in a first place? RDP sessions considered remote session and by default Polkit forces authentication. By applying those rules you are overriding rules for the members of `wheel` (administrators) to bypass the prompts. 
