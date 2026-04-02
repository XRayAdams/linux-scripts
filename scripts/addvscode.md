# Add VSCode repo into DNF repo's list

To add Visual Studio Code repo into list of repositories for DNF, execute this command

```bash
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc && echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\
sudo rpm --import nautorefresh=1\ntype=rpm-md\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" | sudo tee /etc/yum.repos.d/vscode.repo > /dev/null
```

It will create `vscode.repo` file in `/etc/yum.repos.d/` folder. 

To install VSCode, execute this command

```bash
sudo dnf install code
```



