# Package Management / Software 

There are various package managers which are different between distributions.

## RedHat/CentOS/Rocky/SUSE

These use rpm (Red-Hat Package Manager). 

```
rpm -i package                                              # to install software
rpm -q package                                              # to see if the software package is installed
rpm -qa                                                     # to list ALL software installed
rpm -qa --changelog package                                 # to search the changelog of the package 
rpm -qa --changelog openssh-server  | grep  CVE-2015-5600   # search the changelog of openssh-server for the vulnerability (to check it's patched)
rpm -U package                                              # Upgrades a package
rpm -e packages                                             # removes a package
```

However, RPM is notorious for having dependency issues when installing via `rpm` so instead there's wrappers around these that are more commonly used now

## RedHat/CentOS/Rocky

These use `yum` to manage the software packages. 

```
yum install package
yum update
yum remove package
```

## SUSE

```

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
