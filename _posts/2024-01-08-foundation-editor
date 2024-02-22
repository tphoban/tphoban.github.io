---
layout: post
title:  "Foundation Editor"
date:   2024-01-08 17:12:07 +0000
---

The development process has proceeded smoothly and we have managed to implement a foundational version of the editor. It has core features such as creating, editing and saving files. Currently, we are trying to fix some strange behaviour caused by the arrow keys. This has proven difficult to solve. We have also realised that implementing shortcuts such as copy (CTRL + C), paste (CTRL + V) and undo (CTRL + Z) is not a straightforward process, since ncurses does not directly support reading the clipboard. We believe that external libraries such as xclip and xsel could solve this problem. Moreover, both CTRL + C and CTRL + Z are special key combinations which are used to forcefully shut down programs, making development a lot more difficult.