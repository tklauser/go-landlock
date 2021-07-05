# Go landlock library

The Go landlock library provides an interface to Linux 5.13's Landlock
kernel sandboxing features.

The library provides access to Landlock on two levels:

## High level interface

To restrict the current process to only see a given set of paths and
subdirectories, use `golandlock.RestrictPaths`.

**Example:** The following invocation will restrict the current thread
so that it can only read from `/usr`, `/bin` and `/tmp`, and only
write to `/tmp`.

```
err := golandlock.RestrictPaths(
    golandlock.RODirs("/usr", "/bin"),
    golandlock.RWDirs("/tmp"),
)
```

## Low level interface

The low level interface in `golandlock/syscall` provides access to the
raw Landlock syscalls.

## Caveats

Some filesystem operations can't currently be restricted with
Landlock. Familiarity with the Landlock kernel interface and its
limitations is assumed
([documentation](https://landlock.io/linux-doc/landlock-v34/userspace-api/landlock.html),
[filesystem
flags](https://landlock.io/linux-doc/landlock-v34/userspace-api/landlock.html#filesystem-flags):

> It is currently not possible to restrict some file-related actions
> accessible through these syscall families: `chdir(2)`,
> `truncate(2)`, `stat(2)`, `flock(2)`, `chmod(2)`, `chown(2)`,
> `setxattr(2)`, `utime(2)`, `ioctl(2)`, `fcntl(2)`, `access(2)`.
> Future Landlock evolutions will enable to restrict them.
