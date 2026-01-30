---
layout: post
title: Debug with One (MPI) Process When You Can!
tags: [tools]
author: Junchao Zhang
date: 2026-01-27
---

It may feel *counterintuitive* to debug an MPI program using a single process, yet this is often the very first approach you should take.

In many cases, bugs in parallel codes are not caused by MPI itself. Instead, they stem from common programming errors such as out-of-bounds array access, dangling pointers, or uninitialized variables—issues that can frequently be reproduced even with only one process. Therefore, the first step should be to run your code with a single MPI process and observe whether the error persists.

If the issue disappears, or if your test case cannot be run with a single process as written, it is worth reconsidering the underlying assumptions. With a small adjustment, you may be able to restructure the code so that it runs correctly with one process while still reproducing the bug. If you succeed, that is a major win.

At that point, you may not even need `mpiexec` to run the program. This, in turn, means you can often debug directly on a cluster login node without dealing with job schedulers or batch queues. You are then free to use whatever debugging tools are most convenient.

In my own workflow, the choice of tool depends on the task. If I simply need to inspect the value of a variable, I often start the program under `gdb` in a terminal. For more complex investigations, I use VS Code’s “Run and Debug” feature, which provides a graphical interface that I find particularly effective, especially since VS Code is my primary editor.

If you can eliminate MPI from the debugging process of an MPI program, it is a tremendous relief. So, whenever possible, start by debugging with a single process.