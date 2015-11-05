---
layout: post
title:  "Notes on Vim"
date:   2015-10-21 15:57:38
categories: vim
comments: true
---

### Open file in separate split window in vim
```:sp filename``` - to open file in horisontal split

```:vertical new filename``` - to open file in vertical split

### Change active split
```Ctrl-w + w``` - circle between opened splits

### Resize splits
```:resize 60``` - changes height to 60 rows
```:vertical resize 80``` - changes width to 80 columns
```Ctrl-w +``` and ```Ctrl-w -``` — change height by one row
```Ctrl-w >``` and ```Ctrl-w <``` — change width by on column

### Make bang (!) command clear screen before output
To achive this you need to remap bang command for command line mode:

```:noremap :! :!clear; ``` - this would additinally run `clear` which will do the thing.

### Switch between last two opened files
```Ctrl-^``` - circle between last two opened files

### Go to line N
```3G``` - goes to third line
```:21``` - goes to line 21