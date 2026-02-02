---
layout: post
title: Debug with One (MPI) Process When You Can!
tags: [tools]
author: Junchao Zhang
date: 2026-01-27
---

It may feel *counterintuitive* to debug an MPI program using a single process, yet this is often the very first approach you should take.

There are two scenarios that this can be used.

## 1. Run with one process

In many cases, bugs in parallel codes are not caused by MPI itself. Instead, they stem from common programming errors such as out-of-bounds array access, dangling pointers, or uninitialized variables—issues that can frequently be reproduced even with only one process. Therefore, the first step should be to run your code with a single MPI process and observe whether the error persists.

If the issue disappears, or if your test case cannot be run with a single process as written, it is worth reconsidering the underlying assumptions. With a small adjustment, you may be able to restructure the code so that it runs correctly with one process while still reproducing the bug. If you succeed, that is a major win.

At that point, you may not even need `mpiexec` to run the program. This, in turn, means you can often debug directly on a cluster login node without dealing with job schedulers or batch queues. You are then free to use whatever debugging tools are most convenient.

In my own workflow, the choice of tool depends on the task. If I simply need to inspect the value of a variable, I often start the program under `gdb` in a terminal. For more complex investigations, I use VS Code’s “Run and Debug” feature, which provides a graphical interface that I find particularly effective, especially since VS Code is my primary editor.


## 2. Run with multiple processes but only debug one of them

In this case, you really need to run with multiple processes, but you don't need to jump around the processes
to examine data, you just want to focus on one process and this can already give you enough information to diagnose
the problems in your code, then you can *split* the `mpiexec`. For instance, I want to run my test `ex2` with four MPI processes, and looking at rank 0 is enough for me. I would do

```bash
$ mpiexec -n 1 gdb --args ./ex2 -mat_type aij : -n 3 ./ex2 -mat_type aij
```

This will start one `gdb` instance and I can continue my debugging on rank 0. Note `--args` tells arguments (i.e., `-mat_type aij`) after the executable (`ex2`) are passed in as its arguments.

To summarize, if you can avoid debugging multiple processes of an MPI program, it will be a tremendous relief. So, whenever possible, start by debugging with a single process.