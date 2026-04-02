# Configure Git to use `libsecreat` for credentials

First install git-credentials to use `libsecreat`

```bash
sudo dnf install git-credential-libsecret -y
```

Then configure git to use it

```bash
git config --global credential.helper libsecret
```

Next time to clone repo from GitHub or any other Git servers where login is required, git will ask you for username and password. Then it will store information and use it later.

If you want to see saved credentials install `seahorse`

```bash
sudo dnf install seahorse
```

