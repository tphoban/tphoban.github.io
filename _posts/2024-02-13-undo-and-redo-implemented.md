---
layout: post
title:  "Undo and Redo implemented"
date:   2024-02-13 11:08:42 +0000
---

Both undo and redo functionalities have now been implemented into the project. The StateManager class handles this, which acts as a doubly linked list. Linewise selection (CTRL + A) has also been added, meaning that the user can now select multiple lines for selection. We took inspiration from Vim's selection mode. CTRL + A is probably not the most intuitive binding, but that is not high on our priorities for now. In the meantime, we plan to figure out how to handle tabs and add linting mode (CTRL + L), which notifies the user about potential code errors. Due to linters being highly language-specific, we have decided to place focus on linting in Python, since this is the language used by beginners. The decision of linter was between Pylint, Flake8 and Ruff, but we belive that Ruff suits our project needs best due to its extreme speed. Once we have completed these tasks, we can then focus on ironing out known issues and writing the associated documentation for TTENS.