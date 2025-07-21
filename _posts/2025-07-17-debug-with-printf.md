---
layout: post
title: Debugging MPI Programs with printf
cover-img: /assets/img/matrix-big.png
thumbnail-img: /assets/img/matrix-small.jpg
share-img: /assets/img/matrix-big.png
tags: [tools]
author: Junchao Zhang
---

What, debug with printf? Yes, though it is so basic, it works if you only want to check some variables or want to see if a
specific execution path is taken. It doesn't need any tool support, is independent of compiler flags,
 can be local or remote, and works with any MPI job scheduler.
It is perfect when it suffices!

But with MPI, because you likely run with multiple processes, you might like to know which MPI processes printed which values, and don't want to
see interleaved output from processes. We now see how to achieve the two goals.


Tag output with MPI ranks
-------------

The easiest approach is print the MPI rank manually in the printf, e.g., `printf("on rank %d: var = %d\n", myrank, var);`, where you get `myrank` either from an MPI communicator available in the routine you inserted the printf statement, or make `myrank`
a global variable and initialize it right after your MPI initialization statement. However, some MPI implementations
provide options to tag stdout/stderr of MPI processes. Specifically,


|MPI|Options| Description|
| ------------| -----------------|---------------------|
| **MPICH**   | *-prepend-rank*  | Prepend rank to output|
| **OpenMPI** | *-tag-output*    | Tag all output with [job,rank]|


For example, with MPICH

```bash
# MPICH
$ mpirun -n 3 -prepend-rank ./ex1
[1] Hello world from rank 1
[0] Hello world from rank 0
[2] Hello world from rank 2
```

and with OpenMPI
```bash
# OpenMPI
$ mpirun -n 3 -tag-output ./ex1
[1,0]<stdout>:Hello world from rank 0
[1,1]<stdout>:Hello world from rank 1
[1,2]<stdout>:Hello world from rank 2
```

Avoid interleaved output
-------------
If you have many printf's and run with more than a handful of processes,
it is likely their output will be interleaved on your screen,
making it difficult to group output from the same process.
First, think about whether you need to see output from all processes.
If not, guard `printf` with MPI rank expressions like `if (myrank == id) printf("var = %d\n", var);` to only print on rank `id`. Actually, in PETSc, there is
a utility routine `PetscPrintf(MPI_Comm comm, const char format[], ...)` that
only print on rank 0 of `comm`. Otherwise, you can still leverage options from MPI implementations. Specifically,


|MPI|Options| Description|Notes|
| ------------| -----------------|---------------------|----|
| **MPICH**   | *-outfile-pattern* \<filename\> <br> *-errfile-pattern* \<filename\>  | Direct stdout to file <br> Direct stderr to file| One can embed in filename `%r`(global MPI rank), `%h` (hostname), `%g`(process group), `%p`(proxy ID), e.g., `%h.%r.out`|
| **OpenMPI** | *-output-filename* \<dirname\>   | Redirect output from application processes into `dirname/<jobid>/rank.<id>/std[out,err]`. <br> A relative path value will be converted to an absolute path| |


For example, with MPICH
```bash
# MPICH
$ hostname
Alps-2.local

$ mpirun -n 3 -outfile-pattern %h.%r.out ./ex1

$ ls Alps-2.local.*
Alps-2.local.0.out Alps-2.local.1.out Alps-2.local.2.out

$ cat Alps-2.local.0.out
Hello world from rank 0

$ cat Alps-2.local.1.out
Hello world from rank 1
```
and with OpenMPI

```bash
# OpenMPI
$ mpirun -n 3 -output-filename mydir ./ex1
Hello world from rank 0
Hello world from rank 2
Hello world from rank 1

$ ls mydir/1/*
mydir/1/rank.0:
stderr  stdout

mydir/1/rank.1:
stderr  stdout

mydir/1/rank.2:
stderr  stdout

$ cat mydir/1/rank.0/stdout
Hello world from rank 0

$ cat mydir/1/rank.1/stdout
Hello world from rank 1
```

With that said, many printf statements could make your code messy, and later you still have to
remove or comment out these printf's.  Thus if you have other means, e.g., a debugger, you should
use a debugger instead to examine variables.

