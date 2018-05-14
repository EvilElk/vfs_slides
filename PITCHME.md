# VFS
### Abbreviation
For "Virtual File System" or "Virtual Filesystem Switch"
---

# VFS
### What is this ?
It is the abstraction layer between FS drivers and unified system calls.
VFS system calls open(2), stat(2), read(2), write(2), chmod(2) and so
on are called from a process context
---

# Process structure
### task_struct

```
struct task_struct {
    ...
    struct fs_struct *fs;
    struct files_struct *files;
    ...
}
```
---

# Process structure
### fs_struct
```
struct fs_struct {
        int users;
        spinlock_t lock;
        seqcount_t seq;
        int umask;
        int in_exec;
        struct path root, pwd;
} __randomize_layout;

```
---

# Process structure
### files_struct
```
struct files_struct {
        ...
        unsigned int next_fd;
        unsigned long close_on_exec_init[1];
        unsigned long open_fds_init[1];
        unsigned long full_fds_bits_init[1];
        ...
};

```
---
 
---
[linux kernel source tree: Documentation/filesystems/vfs.txt]
[http://www.win.tue.nl/~aeb/linux/vfs/trail-2.html]
