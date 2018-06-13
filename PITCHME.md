# VFS
### Abbreviation
For "Virtual File System" or "Virtual Filesystem Switch"
---
# VFS
![VFS inside kernel](https://upload.wikimedia.org/wikipedia/commons/3/30/IO_stack_of_the_Linux_kernel.svg)
---

# VFS
### What is this ?
It is the **abstraction layer** between FS drivers and unified system calls.
VFS system calls open(2), stat(2), read(2), write(2), chmod(2) and so
on are called from a process context
---
# VFS
### register filesystem
Registers filesystem structures and operations in kernel registry ``
```
static int __init ext4_init_fs(void)
{
...
        err = register_filesystem(&ext4_fs_type);
        if (err)
                goto out;
...
}
```
---
# VFS
### register filesystem
 * Adds the file system passed to the list of file systems the kernel is aware of for mount and other syscalls
```
int register_filesystem(struct file_system_type * fs)
{
        int res = 0;
        struct file_system_type ** p;

        BUG_ON(strchr(fs->name, '.'));
        if (fs->next)
                return -EBUSY;
        write_lock(&file_systems_lock);
        p = find_filesystem(fs->name, strlen(fs->name));
        if (*p)
                res = -EBUSY;
        else
                *p = fs;
        write_unlock(&file_systems_lock);
        return res;
}
```

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
};

```
---
#

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
