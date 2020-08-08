# Using multiple SSH keys for diffenrent users

> Tested with WSL2 Ubuntu 20.04 on Windows 10 Professional

Since GitHub, GitLab etc. don't allow the usage of the same SSH key for multiple accounts, we have to use mutitple SSH keys.

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
#github-user1
Host github.com-user1
    HostName github.com
    User git
    IdentityFile ~/.ssh/github-user1

#github-user2
Host github.com-user2
    HostName github.com
    User git
    IdentityFile ~/.ssh/github-user2

#gitlab-user1
Host gitlab.com-user1
    HostName gitlab.com
    User git
    IdentityFile ~/.ssh/gitlab-user1

#gitlab-user2
Host gitlab.com-user2
    HostName gitlab.com
    User git
    IdentityFile ~/.ssh/gitlab-user2
```

## Cloning a repository

Normally we would clone a repository only by copying its repository URL (from web GUI) and executing:

```bash
# Will fail
git clone git@gitlab.com:user1/repository.git
```

Now we have to take into account that git can't tell which SSH key to use. So we have to specify it when cloning a repository:

```bash
# Will work
git clone git@gitlab.com-user1:user1/repository.git
```

## Working with existing repository

```bash
cd existing-repository
vi .git/config
```

Change the `url` entry of `[remote "origin"]` section accordingly

```bash
[remote "origin"]
    url = git@github.com-user1:user1/repository.git
```

## Further reading

* <https://gist.github.com/jexchan/2351996>
* <https://stackoverflow.com/questions/9672975/switching-between-multiple-ssh-keys-in-git-on-windows>
