## Docker


## Jupyter Lab


## ssh

## htop, ps
- `ps -aux` : to see all processes (zombie, sleeping as well) (can use `grep` later on to filter out processes which you need)
- `htop`
	- Color coding in cpu section
		- Red = Kernel process  
		- Green = Normal user process  
		- Blue = Low priority process
		- **Orange** = CPU utilization by IRQ (interrupt request) time
	- Color coding in memory section (ram utilization by pages)
		- Green: memory pages  
		- Blue: buffer pages  
		- Yellow: cache pages
	- Load average: average of computational work performed by the CPU. 1.0 on a single core CPU would mean 100 percent utilization, similarly, 2.0 on a dual-core CPU represents 100% CPU usage. Contains 3 values
		- 1min avg
		- 5min avg
		- 15min avg
	- CPU process tags
		- |   |   |
|---|---|
|`S`|Sleeping|
|`T/S`|Traced/Stopped|
|`Z`|Zombies|


## (re)nice
- used to change priority of processes
- -20 to 19 (-20 being the highest priority while 19 being the lowest priority). default that will be given to a new process is 0
- with user privileges, one can only increase the niceness (i.e, decreasing the priority of the process). To decrease the niceness (i.e, increasing the priority of the process), one needs `sudo`privilege
- `ps axo pid,comm,nice,cls --sort=-nice`: to see priorities of all processes in a sorted order
- `ps -o pid,comm,nice 23990`: priority of a specific `pid`
- `nice -n <niceLevel> <command>`: to start a process with `<niceLevel>` using `<command>` 
- `renice -n <niceLevel> <pid>`: to alter the nicelevel of a running process with pid `<pid>`
- when we use `htop`, we will see two columns that indicate priority
	- `ni`: it indicates nice value of the process. nice is a user space concept
	- `pri`: it indicates the priority of the process as seen by linux kernel
		- = `20+ni`
		- an `rt` in `pri` column indicates running under real-time scheduling priority
## rsync
- used to sync folders, similar to cp but better because it can be used to exclude some files or folders based on various conditions
- similar to how a `makefile` is better (compiling only the updated files instead of the whole directory of cpp files etc)
- `rsync -av --exclude-from=exclude-file.list /source/dir/ /backups/dir`
	- this command behaves differently based on whether there's a slash at the `source` folder

## du/df
- `du -sh` - for getting storage space of all files and folders in `pwd`
- `du -h -x -d 1` - lists all top level folders’ size
- `df -h` - for usage of all mounted disks
## tee
The tee command, used with a pipe, reads standard input, then writes the output of a program to standard output and simultaneously copies it into the specified file or files
