# Using multiple SSH keys for diffenrent users

> Tested with WSL2 Ubuntu 20.04 on Windows 10 Professional

Since unfortunately providers such as GitHub, GitLab etc. don't allow to have the same SSH key for multiple accounts, therefore we have to use mutitple SSH keys.

## Create SSH keys

```bash
cd ~/.ssh
# If ~/.ssh directory is missing:
# mkdir ~/.ssh && chmod 700 ~/.ssh
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

## Create `.ssh/config`

```bash
cd ~/.ssh
touch config
```

Put the following content into `~/.ssh/config`:

```bash
#github-svefre
Host github.com-svefre
    HostName github.com
    User git
    IdentityFile ~/.ssh/github-svefre

#github-fremue85
Host github.com-fremue85
    HostName github.com
    User git
    IdentityFile ~/.ssh/github-fremue85

#gitlab-svefre
Host gitlab.com-svefre
    HostName gitlab.com
    User git
    IdentityFile ~/.ssh/gitlab-svefre

#gitlab-fremue85
Host gitlab.com-fremue85
    HostName gitlab.com
    User git
    IdentityFile ~/.ssh/gitlab-fremue85
```

## Cloning a repository

Normally we would clone a repo only by copying its repo URL (from web GUI) and executing:

```bash
# Will fail
git clone git@gitlab.com:svefre/repo.git
```

Now we have to take into account that git can't tell which SSH key to use. So we have to specify it when cloning a repo:

```bash
# Will work
git clone git@gitlab.com-svefre:svefre/repo.git
```
