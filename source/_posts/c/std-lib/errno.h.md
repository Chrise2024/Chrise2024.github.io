---
title: C标准库-errno.h
description: errno.h
tags: [Programing,C,C-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-03-11 21:19 +0800
---

该库定义了一个变量 `errno`，储存最后一次错误的错误代码，需搭配 `string.h` 中的 `strerror` 函数获取 错误代码对应的描述信息。

此外，还预定义了许多宏，对应不同的错误代码：

|宏|对应的描述文本|
|:-:|:-:|
|PERM|Operation not permitted|
|ENOENT|No such file or directory|
|ESRCH|No such process|
|EINTR|Interrupted function call|
|EIO|Input/output error|
|ENXIO|No such device or address|
|E2BIG|Arg list too long|
|ENOEXEC|Exec format error|
|EBADF|Bad file descriptor|
|ECHILD|No child processes|
|EAGAIN|Resource temporarily unavailable|
|ENOMEM|Not enough space|
|EACCES|Permission denied|
|EFAULT|Bad address|
|EBUSY|Resource device|
|EEXIST|File exists|
|EXDEV|Improper link|
|ENODEV|No such device|
|ENOTDIR|Not a directory|
|EISDIR|Is a directory|
|ENFILE|Too many open files in system|
|EMFILE|Too many open files|
|ENOTTY|Inappropriate I/O control operation|
|EFBIG|File too large|
|ENOSPC|No space left on device|
|ESPIPE|Invalid seek|
|EROFS|Read-only file system|
|EMLINK|Too many links|
|EPIPE|Broken pipe|
|EDOM|Domain error|
|EDEADLK|Resource deadlock avoided|
|ENAMETOOLONG|Filename too long|
|ENOLCK|No locks available|
|ENOSYS|Function not implemented|
|ENOTEMPTY|Directory not empty|
|EINVAL|Invalid argument|
|ERANGE|Result too large|
|EILSEQ|Illegal byte sequence
STRUNCATE|Unknown error|
|ENOTSUP|not supported|
|EAFNOSUPPORT|address family not supported|
|EADDRINUSE|address in use|
|EADDRNOTAVAIL|address not available|
|EISCONN|already connected|
|ENOBUFS|no buffer space|
|ECONNABORTED|connection aborted|
|EALREADY|connection already in progress|
|ECONNREFUSED|connection refused|
|ECONNRESET|connection reset|
|EDESTADDRREQ|destination address required|
|EHOSTUNREACH|host unreachable|
|EMSGSIZE|message size|
|ENETDOWN|network down|
|ENETRESET|network reset|
|ENETUNREACH|network unreachable|
|ENOPROTOOPT|no protocol option|
|ENOTSOCK|not a socket|
|ENOTCONN|not connected|
|ECANCELED|operation canceled|
|EINPROGRESS|operation in progress|
|EOPNOTSUPP|operation not supported|
|EWOULDBLOCK|operation would block|
|EOWNERDEAD|owner dead|
|EPROTO|protocol error|
|EPROTONOSUPPORT|protocol not supported|
|ETIMEDOUT|timed out|
|ELOOP|too many symbolic link levels|
|EPROTOTYPE|wrong protocol type|
|EOVERFLOW|value too large
