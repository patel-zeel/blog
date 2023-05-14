---
aliases:
- /markdown/2022/01/15/py_over_ipynb
categories:
- Python
date: '2022-01-15'
description: Where .py files are better than .ipynb files?
layout: post
title: Why .py files are better than .ipynb files for ML codebase
toc: true

---

I have shifted from .ipynb files to .py files (and Jupyter to VS code) in the last couple of months. Here are some reasons why I feel .py files are better than .ipynb files:

## Fewer Errors
* .py files are easier to debug with a VS code like IDE, making it easier to find the errors.
* Execution of .py starts fresh, unlike some left out variables silently getting carried over from the last execution/deleted cells in .ipynb files.

## Better Usage of a Shared Server
* .py files release the resources (e.g., GPU memory) once executed. It could be inconvenient to repeatedly remind or be reminded by someone to release the resources manually from a Jupyter notebook.

## Increased Productivity
* You can make use of fantastic auto-complete, syntax-highlighting extensions in VS code to save a lot of time while working with .py files.

## Boost Collaboration
* .py do not take time to render on GitHub because they are just plain text files, unlike .ipynb files.
* It is a lot easier to see the changes made by others in a .py file than a .ipynb file.

## Increased Modularity
* Function and Class calls from other files are seamless with .py files.

Feel free to comment your views/suggestions/additions in the comment box.
