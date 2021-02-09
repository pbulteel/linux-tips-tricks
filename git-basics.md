# Git Basics for Linux administrator

Starting with a new repo. 

```
cd directory
git init
```

If you're going to do something like the `/etc` directory then you want to use something like etckeeper which keeps metadata like permissions, owner, etc of the files.

```
# as root on centos
yum install etckeeper
# as root on debian based
apt -y install etckeeper
cd /etc
etckeeper init
```

If you're simply using git, then you can continue with git commands, otherwise you're use etckeeper instead.

```
git add file.txt
git commit -m "My first file. Initial commit. And some meaningful message."
```

You can use branching to work on files or code.

```
git branch new-branch
git checkout new-branch
...
git add file2.txt
git commit -m "comment"
...
git checkout main
git merge new-branch
```

