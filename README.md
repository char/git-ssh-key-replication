# git-ssh-key-replication

Template repository for a git hook setup for super simple SSH key management.
If you have push access to the repo, then you can add any new key to all your boxes at once.

## Requirements

- Git
- OpenSSH
- Bash

## Usage

```shell
$ # On the push target server
$ git clone 'https://github.com/char/git-ssh-key-replication.git' ~/ssh-keys && cd ~/ssh-keys
ssh-keys/ $ git config receive.denyCurrentBranch updateInstead
ssh-keys/ $ ssh-keygen -t ed25519 -C 'ssh-key-management' -f management-key
ssh-keys/ $ ln -sf ./hooks .git/hooks
```

Then you can clone the new repo and deploy keys with:

```shell
$ git clone 'management@push-target:~/ssh-keys' && cd ssh-keys
ssh-keys/ $ echo 'user@my-fancy-server.me' >> hosts
ssh-keys/ $ cat ~/.ssh/id_ed25519.pub >> keys
ssh-keys/ $ git commit -am "Add my new key and server"
ssh-keys/ $ git push
```

As long as any target login already has `management-key.pub` added to its `authorized_keys`.
