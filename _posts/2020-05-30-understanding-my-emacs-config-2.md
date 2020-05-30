---
layout: post
author: juanrgar
tags: [emacs]
---
Let's continue with another snippet of my GNU Emacs configuration. This snippet obviously tries to define global settings for indenting code.

    (setq c-default-style "linux"
          c-basic-offset 4)
    (setq-default tab-width 4)
    (setq-default indent-tabs-mode nil)

The *c-default-style* variable defines the indentation style to be used in new buffers. According to the documentation [0], the style is set before running any language hooks. This variable is part of the emacs way to indent C code, you can define styles that are just a bunch of settings.

The *c-basic-offset* variable refers to the number of spaces for each indentation level [1]. I set it just after *c-default-style* because indentation styles set their own values for indentation level.

Then, with the *tab-width* and *indent-tabs-mode* variables, I configure the amount of whitespace inserted by a TAB character, and I force that to insert blank characters, instead of a single TAB (i.e., \t) character.

What is most important in this snippet is that I use both *setq* and *setq-default*. *tab-width* and *indent-tabs-mode* are buffer-local variables; by using *setq-default* with them, I set their global default values, instead of their values in the current buffer. On the other hand, *c-default-style* is not buffer-local; its value is used to set the indentation style of every new buffer.


- [0] [https://www.gnu.org/software/emacs/manual/html_node/ccmode/Choosing-a-Style.html](https://www.gnu.org/software/emacs/manual/html_node/ccmode/Choosing-a-Style.html)
- [1] [https://www.gnu.org/software/emacs/manual/html_node/ccmode/Customizing-Indentation.html](https://www.gnu.org/software/emacs/manual/html_node/ccmode/Customizing-Indentation.html)
- [2] [https://www.gnu.org/software/emacs/manual/html_node/efaq/Changing-the-length-of-a-Tab.html](https://www.gnu.org/software/emacs/manual/html_node/efaq/Changing-the-length-of-a-Tab.html)
- [3] [https://www.gnu.org/software/emacs/manual/html_node/eintr/Indent-Tabs-Mode.html](https://www.gnu.org/software/emacs/manual/html_node/eintr/Indent-Tabs-Mode.html)
