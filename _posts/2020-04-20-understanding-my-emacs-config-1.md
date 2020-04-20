---
layout: post
author: juanrgar
tags: [emacs]
---
Over the time I have been adding snippets to my Emacs configuration file, but I have never had the time to thinkg about these elips lines, what they actually do.

I plan to do a series of blog posts to fully describe my Emacs configuration at work (I write C, C++, Java, JavaScript, HTML, CSS, and JSP code). Each blog post will describe a single snippet.

Let's start with the first configuration snippet:

    (when (eq system-type 'darwin) ;; mac specific settings
      (setq mac-option-modifier 'alt)
      (setq mac-command-modifier 'meta)
      (global-set-key [kp-delete] 'delete-char)
      )

I added these lines after installing Emacs on macOS using macports. They basically map Apple's Cmd key to the Meta character, which is a more saner setup than having the Option key as Meta key; my thumbs contort much less this way.

I will go through every line. Let's start with the first one:

    (when (eq system-type 'darwin) ;; mac specific settings
      ...
      )

First of all, ;; introduces a comment in elisp.

To be continued...
