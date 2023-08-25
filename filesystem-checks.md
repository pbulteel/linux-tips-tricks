# Filesystem checks

Some of the basic disk utilities include commands like `du`, `df`, `mount` but these have several options that can be very helpful.

`ls` even has a few options that are useful but not always used.

``` bash
ls -A             # same a -a but excludes . and .. from the output
ls -lSr           # sort by size in reverse order - so the bottom is the largest
ls -lArt          # sort all by last modified date in reverse order - so you see latest at the bottom. 
ls -ld directory  # use to see the information on the directory instead of its contents
ls -lb            # use to print any characters in a filename you don't usually see like spaces, tabs, etc" 
```

With `du`

``` bash
# to get a summary of sizes of directories in the directory you're in
du -sh  *    
du -h -d 1 /dir   # similar to above, does a "depth" of 1 giving totals for the directories under /dir
```

However, if you have multiple filesystem types they can show a lot of data, but you can exclude (`-x`) them from your output. To see what type they are use the `-T` flag and then exclude that. If you have it wrong it won't exclude it (example: nfs vs nfs4)

``` bash
df -h -x squashfs -x nfs4 
```

And if you have a filesystem you want to remount

``` bash
mount -o remount
```

And who can forget `find`, `lsof` and others...

``` bash
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

``` bash
lsof 
lsof +D directory     # this tells you what applications are using files in that directory
```

Of course, if you now know that there are space issues and you need to expand disks you can look at [filesystem expansion](/filesystem-expansion.md) or maybe you need to move data across to another disk.

