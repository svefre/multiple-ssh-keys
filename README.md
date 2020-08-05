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

Normally we would clone a repository only by copying its repository URL (from web GUI) and executing:

```bash
# Will fail
git clone git@gitlab.com:svefre/repository.git
```

Now we have to take into account that git can't tell which SSH key to use. So we have to specify it when cloning a repository:

```bash
# Will work
git clone git@gitlab.com-svefre:svefre/repository.git
```

## Working with existing repository

```bash
cd existing-repository/.git
vi config
```

Change the `url` entry of `[remote "origin"]` section accordingly

```bash
[remote "origin"]
    url = git@github.com-svefre:svefre/multiple-ssh-keys.git
```

## Further reading

* <https://gist.github.com/jexchan/2351996>
* <https://stackoverflow.com/questions/9672975/switching-between-multiple-ssh-keys-in-git-on-windows>
