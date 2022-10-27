# Linux Tips and Tricks
Not everyone is familiar with all the things that can be done on Linux. However, I'm going to try to collect useful tips and tricks.

## Cheat sheets
[Screen cheat sheet](https://gist.github.com/jctosta/af918e1618682638aa82)

[Markdown cheat sheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

[Git cheat sheet](https://education.github.com/git-cheat-sheet-education.pdf)

[VIM cheat sheet](https://vim.rtorr.com/)

[Zypper 1](https://en.opensuse.org/images/1/17/Zypper-cheat-sheet-1.pdf) 
[Zypper 2](https://en.opensuse.org/images/3/30/Zypper-cheat-sheet-2.pdf)

## Debian and Ubuntu derivatives

Debian and derivatives, like Ubuntu, Linux Mint, etc all use `apt-get`, and now `apt` to install 'deb' packages. This makes it easier than using `dpkg` which is still the more powerful of the 3 debian package installers.

```
dpkg -i package                                             # to install software 
apt-get install package                                     # to install software 
apt install package                                         # to install software 
apt-get update                                              # to update the data on the package repositories
apt update                                                  # to update the data on the package repositories
apt-get upgrade                                             # to upgrade the packages on the system
apt upgrade                                                 # to upgrade the packages on the system
```


## Git basics for system administrator
[Git for System Administrator (LinkedInLearning.com)](https://www.linkedin.com/learning/git-for-system-administration/system-administration-with-git?u=74650706)

More in depth information can be found in multiple places. [For example this here](https://www.atlassian.com/git/tutorials/what-is-version-control)

[Basic git commands](/git-basics.md)

## Troubleshooting tools
[Processes](/processes.md)

[Network troubleshooting tools](/networking.md)

[Filesystem utilities and checks](/filesystem-checks.md)

## Package management tools
[Package management](/packagemanagement.md)
