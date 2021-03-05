# Filesystem checks

You're familiar with some of the basic commands like `du`, `df`, `mount` but these have several options that can be very helpful.

Good old `ls` even has a few options that are useful but not always used.
```
ls -A             # same a -a but excludes . and .. from the output
ls -lSr           # sort by size in reverse order - so the bottom is the largest
ls -lArt          # sort all by last modified date in reverse order - so you see latest at the bottom. 
ls -ld directory  # use to see the information on the directory instead of its contents
ls -lb            # use to print any characters in a filename you don't usually see like spaces, tabs, etc" 

```
Now back to `du` 
```
# to get a summary of sizes of directories in the directory you're in
du -sh  *    

```
If you have multiple filesystem types you can exclude (`-x`) them from your output. To see what type they are use the `-T` flag and then exclude that. If you have it wrong it won't exclude it (example: nfs vs nfs4)
```
df -h -x squashfs -x nfs4 
```

```
mount -o remount
```

And who can forget `find`, `lsof` and ...

```
# find file or directory with case insensitive name
find /directory -iname "*name*"
# find file with case insensitive filename
find /directory -type f -iname "*filename*"
# find directory with case insensitive name
find /directory -type d -iname "*name*"
# find file (not directory) older than (+) 3 days.
find /directory -type f -mtime +3
```
If you've deleted a file but he application is still writing to it you can still read the data off of the file.
```
sudo lsof 
```


