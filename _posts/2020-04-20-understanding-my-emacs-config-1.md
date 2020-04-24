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

Then, it comes the *when* macro [0], which has the form *(when condition a b c)*. What it does is to execute the expressions following the condition, if the condition evaluates to true.

In this case, the condition is *(eq system-type 'darwin)*, which is a call to the *eq* function [1], which accepts two objects as arguments and returns *t* if both objects are the same. In this expression, *system-type* is a variable that indicates the type of operating system [2], and *'darwin* is the value we want to compare to. Well, understanding this expression requires some background on Lisp symbols, but I'll reserve that for another post.

There are three expressions in the body of the *when* macro. The *setq* special form set a variable [3]. In this case I'm chaning the behaviour of the option and command keys. The function *global-set-key* assigns the *delete-char* function to the fn+delete key combination [4]. The function *delete-char* deletes characters after the point [5]; since we are not providing any parameter, it will delete a single character. The brackets around *kp-delete* seem to be required to indicate that the key combination involves the fn key.

- [0] [https://www.gnu.org/software/emacs/manual/html_node/elisp/Conditionals.html](https://www.gnu.org/software/emacs/manual/html_node/elisp/Conditionals.html)
- [1] [https://www.gnu.org/software/emacs/manual/html_node/elisp/Equality-Predicates.html](https://www.gnu.org/software/emacs/manual/html_node/elisp/Equality-Predicates.html)
- [2] [https://www.gnu.org/software/emacs/manual/html_node/elisp/System-Environment.html](https://www.gnu.org/software/emacs/manual/html_node/elisp/System-Environment.html)
- [3] [https://www.gnu.org/software/emacs/manual/html_node/elisp/Setting-Variables.html](https://www.gnu.org/software/emacs/manual/html_node/elisp/Setting-Variables.html)
- [4] [https://www.gnu.org/software/emacs/manual/html_node/elisp/Key-Binding-Commands.html](https://www.gnu.org/software/emacs/manual/html_node/elisp/Key-Binding-Commands.html)
- [5] [https://www.gnu.org/software/emacs/manual/html_node/elisp/Deletion.html](https://www.gnu.org/software/emacs/manual/html_node/elisp/Deletion.html)
