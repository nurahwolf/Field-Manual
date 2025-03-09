# GIT (Distributed Version Control)

#### Sign Commits with SSH Public Key (Last updated 2025-03-09)
[Sign commits with your SSH key](https://docs.gitlab.com/user/project/repository/signed_commits/ssh)

Prerequisites:

- You’ve created an SSH key.
- You’ve added the SSH key to the 'server' holding the repository (GitHub, GitLab).
- You’ve configured Git to sign commits with your SSH key.

First, ensure git is configured to use SSH keys for signing, and that a key is used:

```shell
git config --global gpg.format ssh
git config --global user.signingkey $HOME/.ssh/id_ed25519.pub
```

To sign a commit:

```shell
# Use the -S flag when signing your commits:
git commit -S -m "Hello World"
```

If you don’t want to type the -S flag every time you commit, tell Git to sign your commits automatically:

```shell
git config --global commit.gpgsign true
```

#### Undo a staged change

To unstage a specific file:

`git reset <file>`

That will remove the file from the current index (the "about to be committed" list) without changing anything else.
To unstage all files from the current change set:

`git reset`

In old versions of Git, the above commands are equivalent to git reset HEAD <file> and git reset HEAD respectively, and will fail if HEAD is undefined (because you haven't yet made any commits in your repository) or ambiguous (because you created a branch called HEAD, which is a stupid thing that you shouldn't do). This was changed in Git 1.8.2, though, so in modern versions of Git you can use the commands above even prior to making your first commit:

    "git reset" (without options or parameters) used to error out when you do not have any commits in your history, but it now gives you an empty index (to match non-existent commit you are not even on).

Documentation: [Git SCM - git reset](https://git-scm.com/docs/git-reset)
