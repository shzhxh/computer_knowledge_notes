
Usage:
 zramctl [options] <device>
 zramctl -r <device> [...]
 zramctl [options] -f | <device> -s <size>

Set up and control zram devices.

Options:
 -a, --algorithm lzo|lz4   compression algorithm to use
 -b, --bytes               print sizes in bytes rather than in human readable format
 -f, --find                find a free device
 -n, --noheadings          don't print headings
 -o, --output <list>       columns to use for status output
     --raw                 use raw status output format
 -r, --reset               reset all specified devices
 -s, --size <size>         device size
 -t, --streams <number>    number of compression streams

 -h, --help                display this help
 -V, --version             display version

Available output columns:
        NAME  zram device name
    DISKSIZE  limit on the uncompressed amount of data
        DATA  uncompressed size of stored data
       COMPR  compressed size of stored data
   ALGORITHM  the selected compression algorithm
     STREAMS  number of concurrent compress operations
  ZERO-PAGES  empty pages with no allocated memory
       TOTAL  all memory including allocator fragmentation and metadata overhead
   MEM-LIMIT  memory limit used to store compressed data
    MEM-USED  memory zram have been consumed to store compressed data
    MIGRATED  number of objects migrated by compaction
  MOUNTPOINT  where the device is mounted

For more details see zramctl(8).
