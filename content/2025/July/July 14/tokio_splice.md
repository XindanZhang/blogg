## linux splice(2)

Before talking about tokio_splice, we need to understand what linux splice(2) is.

### Description
[`splice()`](https://linux.die.net/man/2/splice) moves data between two file descriptors without copying between kernel address space and user address space. It transfers up to len bytes of data from the file descriptor fd_in to the file descriptor fd_out, where one of the descriptors must refer to a pipe.

The traditional way of moving data between file descriptors involves reading data from one descriptor into a buffer in user space and then writing that buffer to another descriptor. This process requires copying data between kernel space and user space, which can be inefficient.

- Read data from source fd into userspace buffer
- Write data from userspace buffer to destination fd

With splice, the kernel handles this transfer internally:

- Data moves directly from source fd to destination fd within kernel space
- No need for user space copying, reducing overhead

## tokio_splice

[**tokio_splice**](https://docs.rs/tokio-splice/latest/tokio_splice/) is a Rust library that provides asynchronous file descriptor splicing capabilities using the tokio runtime. It allows for efficient data transfer between file descriptors without the overhead of copying data to user space.

### The Pipe-in-the-Middle Solution

Instead, [`zero_copy_bidirectional`](https://docs.rs/tokio-splice/latest/tokio_splice/fn.zero_copy_bidirectional.html) uses pipes as intermediaries. Think of pipes as temporary "staging areas" in kernel memory. Here's how it works conceptually:
For data flowing from Socket A to Socket B:

Create a pipe (let's call it `pipe_ab`)
```rust
splice(socket_a, pipe_ab) - moves data from socket to pipe
splice(pipe_ab, socket_b) - moves data from pipe to socket
```

For the reverse direction (Socket B to Socket A):

Create another pipe (`pipe_ba`)
```rust
splice(socket_b, pipe_ba) - moves data from socket to pipe
splice(pipe_ba, socket_a) - moves data from pipe to socket
```

### Why This Still Achieves Zero-Copy

You might wonder: "Doesn't using pipes as intermediaries defeat the purpose?".

The beautiful thing is that pipes in Linux are implemented entirely in kernel memory. When you splice data into a pipe and then splice it out, the data never crosses the kernel-userspace boundary. The pipe acts like a kernel-space buffer that can be efficiently managed without memory copies.
