
Usage:
 ipcmk [options]

Create various IPC resources.

Options:
 -M, --shmem <size>       create shared memory segment of size <size>
 -S, --semaphore <number> create semaphore array with <number> elements
 -Q, --queue              create message queue
 -p, --mode <mode>        permission for the resource (default is 0644)

 -h, --help               display this help
 -V, --version            display version

For more details see ipcmk(1).
