---
layout: post
title: Why This Website?
subtitle: Building a Community Hub for MPI Program Debugging
cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/community.png
share-img: /assets/img/path.jpg
tags: [annoucement]
author: Junchao Zhang
---

If you've landed here, you're probably already familiar with key MPI-related websites like [mpich.org](https://www.mpich.org), [open-mpi.org](https://www.open-mpi.org), and [mpi-forum.org](https://www.mpi-forum.org). As a [PETSc](https://www.petsc.org) developer, I write MPI code regularly—and debugging MPI programs is part of my daily routine. One day, I discovered that nobody had registered the domain *mpi-debug.org*. So, I bought it. That’s the simple reason this site exists :)


MPI (Message Passing Interface) is the _de facto_ standard programming model in high-performance computing (HPC), and it's used by nearly all major HPC software packages. As essential as debugging is in any software project—whether to fix bugs or better understand the code—debugging MPI programs is difficult. MPI applications often run with tens to millions of processes, making debugging not only complex but sometimes overwhelming.

Commercial MPI debuggers can be very powerful, but they’re often unavailable or unaffordable to many HPC researchers, developers or students. Free tools like GDB work well for sequential programs but are clumsy in the MPI world. Moreover, MPI debugging tips and techniques are scattered across mailing lists, GitHub issues, blogs, and old forum posts—making it hard to find consolidated, practical advice.


Supported by the 2025 [Better Scientific Software (BSSw)](https://bssw.io) Fellowship, I’ve launched this site to serve a broader purpose—not as a personal blog, but as a **community hub** for MPI debugging.

I hope this can become a space where HPC programmers come together to:

- Share debugging strategies and tools
- Write and contribute tutorials or walkthroughs
- Ask questions and exchange practical insights

In short, I believe the collective experience of the community can make MPI debugging less frustrating and more accessible for everyone.


To support interaction, I’ve integrated [Giscus](https://github.com/giscus/giscus), a commenting system based on GitHub Discussions. You can leave comments on any article or start a new topic directly on [GitHub Discussions](https://github.com/mpi-debug/mpi-debug.github.io/discussions).

Want to contribute an article? At the moment, you'll need to fork the repository and submit a pull request. I know—it’s not user-friendly. I'm actively looking for simpler ways to enable contributions. In addition, Giscus requires you to log in with GitHub to leave comments. I am not sure whether to enable anonymous commenting through the ad-supported [Disqus](https://disqus.com/).
If you have suggestions or preferences, I’d love to hear from you—feel free to leave a comment below.

Thanks for visiting. Let’s build this together!

