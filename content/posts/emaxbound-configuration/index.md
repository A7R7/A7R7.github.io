---
title: "EmaxBound Configuration"
authors: ["A7R7"]
description: "My GNU Emacs's literate config"
publishDate: 2023-07-24
tags: ["Emacs", "Org-mode"]
draft: false
featuredImage: "Emacsbound.png"
---

<!--more-->

---

<a href="https://www.gnu.org/software/emacs/"><img alt="GNU Emacs" src="https://img.shields.io/badge/emacs-29.1-8A2BF2?logo=gnuemacs&logoColor=white"/></a>

<a href="https://github.com/emacscollective/borg"><img alt="Package Manager" src="https://img.shields.io/badge/package_manager-borg-green"/></a>

<a href="https://en.wikipedia.org/wiki/Linux"><img alt="Linux" src="https://img.shields.io/badge/linux-FCC624?logo=linux&logoColor=black"/></a>

{{< figure src="dashboard.png" >}}


## Introduction {#introduction}


### How to deploy this config {#how-to-deploy-this-config}

This config file is still in its early stages of development, **Everything is unstable!**
To play around with my config files, clone this repo anywhere you like on your system.

```bash
git clone --depth=1 https://github.com/A7R7/EmaxBound.git
```

Then, simply execute these make rules inside the repo directory.

```bash
make bootstrap-borg  # bootstrap borg itself
make bootstrap       # bootstrap collective or new drones
```

Last, run `emacs --init-directory=<path-of-the-repo>` (replace the path).


### How I built this config from the seed {#how-i-built-this-config-from-the-seed}

The whole config structure is built upon [emacscollective/emacs.g](https://github.com/emacscollective/emacs.g).
It is a starter-kit using borg as the package manager, which utilizes git submodules to maintain all its packages.
Check [Bootstrapping-using-a-seed](https://emacsmirror.net/manual/borg/Bootstrapping-using-a-seed.html) from Borg's manual to see how to build the config structure.

-   First, I generated the structure from the seed.
-   Then, I copied and pasted all the original code from init.el and early-init.el to this org file.
-   Last, most further config does not go beyond the following 3 steps:
    -   Run [M-x borg-assimilate](< borg-assimilate>) and input the name of the package to install this package.
    -   Write config codes into this org file, then run [M-x org-babel-tangle](org-babel-tangle) so that emacs writes the codes into the corresponding file. (the org-auto-tangle package that I installed enables auto tangling on saving org file)
    -   For some packages, specify its load path or build method in `.borgconfig` for them to be load or built as expected. This is auto tangled as well.


### Why it's called EmaxBound {#why-it-s-called-emaxbound}

The name "EmaxBound" is inspired by "Starbound", a game I used to play and I love it.
Why it is "EmaxBound" but not "EmacsBound" ?

-   "Emax" has the same number of characters of "Star"
-   So that the logo I drawed (mimicing the style of starbound) looks balanced
-   "Emaxbound" means "Explore the maxium bound of emacs"


### Why not other popular emacs distros {#why-not-other-popular-emacs-distros}

-   Doom emacs, spacemacs and other emacs "distros", are pretty good. They did a good job in "working out of the box". However, their complexities is blocking me from gaining a further understanding of emacs and elisp.
-   Meanwhile, it's much harder to do customizations on those complex systems, especially when you do not know what it already have. However, self-configured emacs does not have this problem.
-   Those distros get updates from time to time. What if one update breaks your workflow?


### Why a literate config {#why-a-literate-config}

Using literate configuration in org-mode has many benefits.

-   Literate programming just feels so good in emacs.
    -   You know what you code does, because there're descriptions of different styles around the code. Code comments cannot achieve 100% of this.
    -   Easy jumping to the target code among those foldable outlines. No need to split a file into more.
    -   Looks good, feels good.
-   The org file can be used as readme in github/gitlab, which shows all your codes.
-   The org file can be published as blogs. For example, I use ox-hugo to generate markdown files, and later hugo renders it to html files. This is where this blog comes from.


### References {#references}

Credits goes to the authors of those emacs configs that I referenced during the build-up of my emacs config file.

-   [Luca's vanilla-emacs](https://github.com/lccambiaghi/vanilla-emacs) (2023) detailed org config file.
-   [DistroTube's Configuring Emacs](https://gitlab.com/dwt1/configuring-emacs) (2023) easy to follow.
-   [seagle0128's Centaur Emacs](https://github.com/seagle0128/.emacs.d) (2023) be morden.
-   [Daviwell's Emacs from scratch](https://github.com/daviwil/emacs-from-scratch) (2021) intuitive.
-   [Doom Emacs](https://github.com/doomemacs/doomemacs) (2023) Some best practices.
-   [Dakra's Dmacs](https://github.com/dakra/dmacs) (2023) Another Emacs Literate configuration with borg
-   [DogLooksGood's Meomacs](https://github.com/DogLooksGood/meomacs) (2023) Meow modal editing, emacs native friendly

**NOTE**: the year number after link equals to
min (last time the config get's updated, last time I refered to the config)


## Early Init {#early-init}

**NOTE**: elisp codes under this headline are tangled to early-init.el.

Emacs load early-init.el before init.el.

Enable lexical-binding. Disable byte-compile for early-init.el.

According to the [emacs manual](https://www.gnu.org/software/emacs/manual/html_node/emacs/Init-File.html), byte-compiling the init does not startup very much, and often leads to problems when you forget to recompile the file. Also, from my experience, it may lead to bugs that do not happen when not using byte-compile.

```elisp
;;; -*- lexical-binding: t; no-byte-compile: t -*-
```

Defer garbage collection in the startup process.

```elisp
(setq gc-cons-threshold most-positive-fixnum)
;; copied from lazycat
(setq gc-cons-percentage 0.6)
```

Prevent unwanted runtime compilation for native-comp.

```elisp
(setq native-comp-deferred-compilation nil ;; obsolete since 29.1
      native-comp-jit-compilation nil)
```

Disable tool-bar, menu-bar and scroll-bar before they're loaded.

```elisp
(push '(menu-bar-lines . 0) default-frame-alist)
(push '(tool-bar-lines . 0) default-frame-alist)
(push '(vertical-scroll-bars) default-frame-alist)
;; Prevent flashing of unstyled modeline at startup
(setq-default mode-line-format nil)
```

Smooth window on startup

```elisp
(setq frame-inhibit-implied-resize t)
```

Config use-package before loading use-package

```elisp
;(setq use-package-enable-imenu-support t)
(setq use-package-verbose t)
```

Below codes belongs to the original Borg seed.

```elisp
(setq load-prefer-newer t)

(let ((dir (file-name-directory (or load-file-name buffer-file-name))))
  (add-to-list 'load-path (expand-file-name "lib/compat" dir))
  (add-to-list 'load-path (expand-file-name "lib/packed" dir))
  (add-to-list 'load-path (expand-file-name "lib/auto-compile" dir)))
(require 'auto-compile)
(auto-compile-on-load-mode)
(auto-compile-on-save-mode)

(setq package-enable-at-startup nil)

(with-eval-after-load 'package
  (add-to-list 'package-archives (cons "melpa" "https://melpa.org/packages/") t))
```


## Init {#init}


### Begin of init {#begin-of-init}

**NOTE**: Starting from here, elisp codes are tangled to init.el

After loading early-init.el, emacs begin to load init.el.

Disable byte compile for init.el, same reason of early-init.el.

```elisp
;;; -*- lexical-binding: t; no-byte-compile: t -*-
```

Calculating time used loading emacs excutable, as well as setting some variables.

```elisp
(progn ;     startup
  (defvar before-user-init-time (current-time)
    "Value of `current-time' when Emacs begins loading `user-init-file'.")
  (message "Loading Emacs...done (%fs)"
       (float-time (time-subtract before-user-init-time
          before-init-time)))
  (setq user-init-file (or load-file-name buffer-file-name))
  (setq user-emacs-directory (file-name-directory user-init-file))
  (message "Loading %s..." user-init-file)
)
```

Set some defaults of emacs

```elisp
(progn
  (setq inhibit-startup-buffer-menu t)
  (setq inhibit-startup-screen t)
  (setq inhibit-startup-echo-area-message "locutus")
  (setq initial-buffer-choice t)
  (setq initial-scratch-message "")
  ;; This improves performance for some fonts
  (setq inhibit-compacting-font-cache t)
  (setq confirm-kill-emacs 'y-or-n-p)
)
```


### Borg {#borg}

[Borg](https://github.com/emacscollective/borg) assimilate Emacs packages as Git submodules. Core of the core units.
	`borg-initialize` should be called in init.el for borg to initialize assimilated drones using `borg-activate`.
	 To skip the activation of the drone named DRONE, temporarily disable it by setting the value of the Git variable submodule.DRONE.disabled to true in ~/.config/emacs/.gitmodules.

```elisp
(use-package borg
:init
  (add-to-list 'load-path
    (expand-file-name "lib/borg" user-emacs-directory))
:config
  (borg-initialize)
)
```


### Dash {#dash}

[Dash](https://github.com/magnars/dash.el) is a modern list library for Emacs. See its overview at [dash.el - functions](https://github.com/magnars/dash.el#functions).
	 `Dash-Fontify mode` is a buffer-local minor mode intended for Emacs Lisp buffers.  Enabling it causes the special variables bound in anaphoric Dash macros to be fontified.  These anaphoras include ‘it’, ‘it-index’, ‘acc’, and ‘other’.  In older Emacs versions which do not dynamically detect macros, Dash-Fontify mode additionally fontifies Dash macro calls.

```elisp
(use-package dash
  :config (global-dash-fontify-mode))
```

Dash needs some tweaks to be built

```cfg
[submodule "dash"]
  no-byte-compile = dash-functional.el
  no-makeinfo = dash-template.texi
```


### EIEIO {#eieio}

[EIEIO](https://www.gnu.org/software/emacs) is a series of Lisp routines which implements a subset of CLOS, the Common Lisp Object System. In addition, EIEIO also adds a few new features which help it integrate more strongly with the Emacs running environment.

```elisp
(use-package eieio)
```


### Auto-Compile {#auto-compile}

[Auto-Compile](https://github.com/emacscollective/auto-compile) automatically compile Emacs Lisp libraries
   Suppress comp warnings.

```elisp
(use-package auto-compile
  :config
  (setq auto-compile-display-buffer             nil
    auto-compile-mode-line-counter            t
    auto-compile-source-recreate-deletes-dest t
    auto-compile-toggle-deletes-nonlib-dest   t
    auto-compile-update-autoloads             t
    warning-suppress-log-types        '((comp))
  )
)
```


### Epkg {#epkg}

[Epkg](https://github.com/emacscollective/epkg) allows you browse the Emacsmirror package database. We're using emacs &gt;= 29 which has builtin support for sqlite, so we let epkg-database-connector to use builtin sqlite.

```elisp
(use-package epkg
  :defer t
  :bind
     ([remap describe-package] . epkg-describe-package)
  :init
  (setq epkg-repository
  (expand-file-name "var/epkgs/" user-emacs-directory))
  (setq epkg-database-connector 'sqlite-builtin ))
```


### Custom {#custom}

[Custom](https://www.emacswiki.org/emacs/CustomizingAndSaving#Customize) is a built-in package, the customize system of emacs. Set the file path used for storing customization information here.

```elisp
(use-package custom
  :no-require t
  :config
  (setq custom-file (expand-file-name "custom.el" user-emacs-directory))
  (setf custom-safe-themes t) ;Treat all themes as safe
  (when (file-exists-p custom-file)
    (load custom-file)))
```


### Server {#server}

Server allows Emacs to operate as a server for other processes. Built in.

```elisp
(use-package server
  :commands (server-running-p)
  :config (or (server-running-p) (server-mode)))
```


### Org {#org}

[Org-9.7](https://git.tecosaur.net/tec/org-mode) did some overhaul to org-latex-preview in org mode. Load External org before the built in org is loaded.

```elisp
(use-package org)
```

All the .el files are placed in the ./lisp/ folder.
According to the installation manual of org, we need to make autoloads before compile.

```cfg
[submodule "org"]
  load-path = lisp
  build-step = make autoloads
  build-step = borg-update-autoloads
  build-step = borg-compile
  build-step = borg-maketexi
  build-step = borg-makeinfo
```


### End of core units {#end-of-core-units}

Calculate loading time of core units.

```elisp
(progn ;     startup
  (message "Loading core units...done (%fs)"
     (float-time (time-subtract (current-time) before-user-init-time))))
```


## Libraries {#libraries}


### S {#s}

[S](https://github.com/magnars/s.el) is the long lost Emacs string manipulation library.


### F {#f}

[F](https://github.com/rejeep/f.el) is a modern API for working with files and directories in Emacs.


### Annalist {#annalist}

[annalist.el](https://github.com/noctuid/annalist.el) is a library that can be used to record information and later print that information using org-mode headings and tables. It allows defining different types of things that can be recorded (e.g. keybindings, settings, hooks, and advice) and supports custom filtering, sorting, and formatting. annalist is primarily intended for use in other packages like general and evil-collection, but it can also be used directly in a user’s configuration.


### Shrink path {#shrink-path}

[Shrink path](https://github.com/zbelial/shrink-path.el) is a small utility functions that allow for fish-style trunctated directories in eshell and for example modeline.

```elisp
(use-package shrink-path :demand t)
```


### Emacsql {#emacsql}

tweaks to buiild emacsql

```cfg
[submodule "emacsql"]
no-byte-compile = emacsql-pg.el
```


### Sqlite3 {#sqlite3}

```cfg
[submodule "sqlite3"]
  build-step = make
```


## Basic Kbd {#basic-kbd}

We setup keybinding framworks and basic keybindings at this place. Note that not all keybindings are set here. Some package specific keybinding configs are set under where the package is configured.


### Evil {#evil}

I guess evil surround and evil nerd commentor should be better to put under Coding.
I do not use evil mode anymore because of Meow Edit.


#### Evil mode {#evil-mode}

[Evil mode](https://github.com/emacs-evil/evil) that turns you into an evil.

```elisp
(use-package evil
  :disabled
  :init
    (setq evil-want-integration t) ;; t by default
    (setq evil-want-keybinding nil)
    (setq evil-vsplit-window-right t)
    (setq evil-split-window-below t)
    (setq evil-want-C-u-scroll t)

  :config
    (evil-mode 1)
   ;; Use visual line motions even outside of visual-line-mode buffers
    (evil-global-set-key 'motion "j" 'evil-next-visual-line)
    (evil-global-set-key 'motion "k" 'evil-previous-visual-line)
    (evil-set-initial-state 'messages-buffer-mode 'normal)
    (evil-set-initial-state 'dashboard-mode 'normal)
    (evil-set-undo-system 'undo-redo)
    (evil-define-key 'normal 'foo-mode "e" 'baz)
)
```

```cfg
[submodule "evil"]
  info-path = doc/build/texinfo
```


#### Evil collection {#evil-collection}

[Evil-collection](https://github.com/emacs-evil/evil-collection) automatically configures various Emacs modes with Vi-like keybindings.

```elisp
(use-package evil-collection
  ;; :demand t
  :disabled
  :after evil
  :custom (evil-collection-setup-minibuffer t)
  :config
  ;(setq evil-collection-mode-list '(dashboard dired ibuffer))
  (evil-collection-init))

(use-package evil-tutor
  :demand t)

(use-package emacs
  :config (setq ring-bell-function #'ignore)
)
```


#### Evil Surround {#evil-surround}

```elisp
(use-package evil-surround
:after evil
:disabled
:config
  (global-evil-surround-mode 1))
```


#### Evil Nerd commenter {#evil-nerd-commenter}

[Evi Nerd Commenter](https://github.com/redguardtoo/evil-nerd-commenter) helps you comment code efficiently!

```elisp
(use-package evil-nerd-commenter
:after evil
:disabled
:config
)
```


### Meow {#meow}

[Meow](https://github.com/meow-edit/meow) is yet another modal editing. Meow's freedom allows my setup to be very weird.

```elisp
(use-package meow
  :custom-face
  (meow-cheatsheet-command ((t (:height 180 :inherit fixed-pitch))))
  :config
  ;; Replicate the behavior of vi's
  (defun my-meow-append ()
    "Move to the end of selection, switch to INSERT state."
    (interactive)
    (if meow--temp-normal
      (progn
        (message "Quit temporary normal mode")
        (meow--switch-state 'motion))
      (if (not (region-active-p))
        (when (and (not (use-region-p))
         (< (point) (point-max)))
          (forward-char 1))
          (meow--direction-forward)
          (meow--cancel-selection))
      (meow--switch-state 'insert))
  )
  (advice-add 'meow-append :override #'my-meow-append)

  (setq meow-keypad-self-insert-undefined nil)
  (setq meow-selection-command-fallback '(
        (meow-change . meow-mark-word)
        (meow-kill . meow-delete)
        (meow-cancel-selection . keyboard-quit)
        (meow-pop-selection . meow-pop-grab)
        (meow-beacon-change . meow-beacon-change-char)
        (meow-replace . meow-yank)
        (meow-reverse . negative-argument)
  ))
  (defun meow-setup ()
    (setq meow-cheatsheet-layout meow-cheatsheet-layout-qwerty)

    (meow-motion-overwrite-define-key
     '("i" . meow-prev)
     '("k" . meow-next)
     '("<escape>" . ignore))

    (meow-leader-define-key
     ;; SPC j/k will run the original command in MOTION state.
     '("m" . meow-change-char)
     ;; Use SPC (0-9) for digit arguments.
     '("1" . meow-digit-argument) '("2" . meow-digit-argument)
     '("3" . meow-digit-argument) '("4" . meow-digit-argument)
     '("5" . meow-digit-argument) '("6" . meow-digit-argument)
     '("7" . meow-digit-argument) '("8" . meow-digit-argument)
     '("9" . meow-digit-argument) '("0" . meow-digit-argument)
     '("/" . meow-keypad-describe-key) '("?" . meow-cheatsheet))

    (meow-normal-define-key
     '("1" . meow-expand-1) '("2" . meow-expand-2)
     '("3" . meow-expand-3) '("4" . meow-expand-4)
     '("5" . meow-expand-5) '("6" . meow-expand-6)
     '("7" . meow-expand-7) '("8" . meow-expand-8)
     '("9" . meow-expand-9) '("0" . meow-expand-0)
     '("-" . negative-argument) '(";" . meow-reverse)
     '("," . meow-inner-of-thing) '("." . meow-bounds-of-thing)
     '("/" . meow-visit)
     '("[" . meow-beginning-of-thing) '("]" . meow-end-of-thing)
     '("b" . meow-block) '("B" . meow-to-block)
     '("c" . meow-save) '("C" . meow-sync-grab)
     '("D" . meow-open-below)
     '("E" . meow-open-above)
     '("f" . meow-next-word) '("F" . meow-next-symbol)
     '("g" . meow-find) '("G" . meow-grab)
     '("h" . meow-line) '("H" . meow-goto-line)
     '("i" . meow-prev) '("I" . meow-prev-expand)
     '("j" . meow-left) '("J" . meow-left-expand)
     '("k" . meow-next) '("K" . meow-next-expand)
     '("l" . meow-right) '("L" . meow-right-expand)
     '("m" . meow-change) '("M" . meow-mark-symbol)
     '("n" . meow-search)
     '("o" . meow-append) '("O" . meow-open-below)
     '("p" . meow-pop-selection)
     '("q" . meow-quit)
     '("s" . meow-back-word) '("S" . meow-back-symbol)
     '("t" . meow-till)
     '("u" . meow-insert) '("U" . meow-open-above)
     '("v" . meow-replace) '("V" . meow-yank-pop)
     '("x" . meow-kill)
     '("y" . meow-join)
     '("z" . meow-undo) '("Z" . meow-undo-in-selection)
     '("'" . repeat) '("<escape>" . meow-cancel-selection)
     '("(" . fingertip-wrap-round) '(")" . fingertip-unwrap)
     '("{" . fingertip-wrap-curly)
     '("\"" . fingertip-double-quote)
    )
  )
  (meow-setup)
  (meow-global-mode)
)
```


### General {#general}

[General](https://github.com/noctuid/general.el) provides a more convenient method for binding keys in emacs
(for both evil and non-evil users).

**Note**: byte compile init.el will lead to function created by general-create-definer failed to work.

```elisp
;; Make ESC quit prompts
(global-set-key (kbd "<escape>") 'keyboard-escape-quit)

(use-package general
  :config
  ;; (general-evil-setup)
  ;; set up 'SPC' as the global leader key

  (general-evil-setup t)
  (general-create-definer config/leader
    ;:states '(normal insert visual emacs)
    :keymaps 'meow-normal-state-map
    ;:keymaps 'override
    :prefix "SPC" ;; set leader
    :global-prefix "M-SPC" ;; access leader in insert mode
  )

  (config/leader
    "DEL"     '(which-key-undo                 :wk "󰕍 Undo key"))

  ;; buffers
  (config/leader :infix "b"
    ""        '(nil                            :wk "  Buffer ")
    "DEL"     '(which-key-undo                 :wk "󰕍 Undo key")
    "b"       '(switch-to-buffer               :wk " Switch ")
    "d"       '(kill-this-buffer               :wk "󰅖 Delete ")
    "r"       '(revert-buffer                  :wk "󰑓 Reload ")
    "["       '(previous-buffer                :wk " Prev ")
    "]"       '(next-buffer                    :wk " Next ")
    )
  ;; centaur tabs
  (config/leader
    "{"       '(centaur-tabs-backward-group    :wk " Prev Group")
    "}"       '(centaur-tabs-forward-group     :wk " Next Group")
    "["       '(centaur-tabs-backward          :wk " Prev Buffer ")
    "]"       '(centaur-tabs-forward           :wk " Next Buffer ")
    )
  ;; builtin-tabs
  (config/leader :infix "TAB"
    ""        '(nil                            :wk " 󰓩 Tab ")
    "DEL"     '(which-key-undo                 :wk "󰕍 Undo key")
    "TAB"     '(tab-new                        :wk "󰝜 Tab New ")
    "d"       '(tab-close                      :wk "󰭌 Tab Del ")
    "["       '(tab-previous                   :wk " Prev ")
    "]"       '(tab-next                       :wk " Next ")
    )
  ;; windows
  (config/leader :infix "w"
    ""        '(nil                            :wk " 󰓩 Tab ")
    "DEL"     '(which-key-undo                 :wk "󰕍 Undo key")
    "d"       '(delete-window                  :wk "󰅖 Delete  ")
    "v"       '(split-window-vertically        :wk "󰤻 Split   ")
    "s"       '(split-window-horizontally      :wk "󰤼 Split   ")
    "\\"      '(split-window-vertically        :wk "󰤻 Split   ")
    "|"       '(split-window-horizontally      :wk "󰤼 Split   ")
    "h"       '(evil-window-left               :wk " Focus H ")
    "j"       '(evil-window-down               :wk " Focus J ")
    "k"       '(evil-window-up                 :wk " Focus K ")
    "l"       '(evil-window-right              :wk " Focus L ")
    )
  ;; Borg
  (config/leader :infix "B"
    ""        '(nil                            :wk " 󰏗 Borg      ")
    "DEL"     '(which-key-undo                 :wk "󰕍 Undo key   ")
    "a"       '(borg-assimilate                :wk "󱧕 Assimilate ")
    "A"       '(borg-activate                  :wk " Activate   ")
    "b"       '(borg-build                     :wk "󱇝 Build      ")
    "c"       '(borg-clone                     :wk " Clone      ")
    "r"       '(borg-remove                    :wk "󱧖 Remove     ")
    )
  ;; toggle
  (config/leader :infix "t"
    ""        '(nil                            :wk " 󰭩 Toggle    ")
    "DEL"     '(which-key-undo                 :wk "󰕍 Undo key   ")
    )
  ;; quit
  (config/leader :infix "q"
    ""        '(nil                            :wk " 󰗼 Quit      ")
    "DEL"     '(which-key-undo                 :wk "󰕍 Undo key   ")
    "q"       '(save-buffers-kill-terminal     :wk "󰗼 Quit Emacs ")
    )
  ;; Git
  (config/leader :infix "g"
    ""        '(nil                            :wk " 󰊢 Git       ")
    "DEL"     '(which-key-undo                 :wk "󰕍 Undo key   ")
    "g"       '(magit                          :wk " Magit      ")
    )
  ;; dired
  (config/leader
    "e"       '(dirvish-side                   :wk "󰙅 Dirvish-side ")
    ;;"E"       '(dirvish                        :wk " Dirvish      ")
    ;;"qe"      '(save-buffers-kill-emacs         :wk "Quit Emacs ")
    ;;"e"       '(treemacs                        :wk "󰙅 Treemacs ")
    )
  (config/leader
    "/"       '(evilnc-comment-or-uncomment-lines :wk "󱀢 Comment ")
    )
  )
```


### Which-key {#which-key}

[Which-key](https://github.com/justbur/emacs-which-key) is a minor mode for Emacs that displays the key bindings following your currently entered incomplete command (a prefix) in a popup.

Magit and meow all use transient maps, therefore we let which-key show transient maps.

```elisp
(use-package which-key
:after general
:init
  (setq
    which-key-sort-order #'which-key-key-order-alpha
    which-key-sort-uppercase-first nil
    which-key-add-column-padding 1
    which-key-max-display-columns nil
    which-key-min-display-lines 6
    which-key-side-window-location 'bottom
    which-key-side-window-slot -10
    which-key-side-window-max-height 0.25
    which-key-idle-delay 0.8
    which-key-idle-secondary-delay 0.01
    which-key-max-description-length 25
    which-key-allow-imprecise-window-fit t
    ;which-key-separator " → "
    which-key-separator " "
    Which-key-show-early-on-C-h t
    which-key-sort-order 'which-key-prefix-then-key-order
    which-key-show-transient-maps t
 )
  ;(general-define-key
  ;:keymaps 'which-key-mode-map
  ;  "DEL" '(which-key-undo :wk "undo")
  ;)
  (which-key-mode 1)
)
```

[Which-key-posframe](https://github.com/yanghaoxie/which-key-posframe) use posframe to show which-key popup.
options for `which-key-posframe-poshandler`:

```elisp
(use-package which-key-posframe
:config
  (setq which-key-posframe-poshandler
      'posframe-poshandler-window-bottom-center
      ;'posframe-poshandler-frame-bottom-center
  )
  (which-key-posframe-mode)
)
```


### Transient {#transient}

[Transient-posframe](https://github.com/yanghaoxie/transient-posframe) display transient popups using a posframe.

```elisp
(use-package transient-posframe
:after transient
:disabled
:config
  (setq transient-posframe-min-height 1)
  (setq transient-posframe-mode t)
)
```


### Key-echo {#key-echo}

[Key-Echo](https://github.com/manateelazycat/key-echo) is an Emacs plugin that uses XRecord technology to listen to system key events.

```elisp
(use-package key-echo
:disabled
:config
  (key-echo-enable)
)
```


## Basic UI {#basic-ui}

We setup UI for basic emacs widgets at this place. Again, not all UI's are set here.
Some package specific UI configs are set under where the package is configured.


### Fonts {#fonts}

Defining the various fonts that Emacs will use.
Note that monospace fonts are not always fixed-pitch [Monospace vs fixed-width](https://stackoverflow.com/questions/70797173/monospace-font-characters-are-not-fixed-width).

```elisp
(set-face-attribute 'default nil
  ;:font "JetBrainsMono Nerd Font"
  :font "RobotoMono Nerd Font"
  ;:font "Sarasa Term SC Nerd"
  ;:font "Sarasa Gothic SC"
  :height 180
)
(set-face-attribute 'variable-pitch nil
  :font "Sarasa Gothic SC"
  :height 180
)
(set-face-attribute 'fixed-pitch nil
  ;:font "Sarasa Fixed SC"
  :font "RobotoMono Nerd Font"
  :height 180
)
(set-face-attribute 'fixed-pitch-serif nil
  ;:family "Monospace Serif"
  :font "RobotoMono Nerd Font"
  :height 180
)
```

Makes commented text and keywords italics. Working in emacsclient but not emacs.

```elisp
(set-face-attribute 'font-lock-comment-face nil
  :foreground "LightSteelBlue4" :slant 'italic)
(set-face-attribute 'font-lock-keyword-face nil :slant 'italic)
```

links

```elisp
(set-face-attribute 'link nil
  :foreground "#ffcc66" :underline t :bold nil)
```


#### Zooming In/Out {#zooming-in-out}

You can use the bindings CTRL plus =/- for zooming in/out.  You can also use CTRL plus the mouse wheel for zooming in/out.

```elisp
(use-package emacs
  :init
    (global-set-key (kbd "C-=")            'text-scale-increase)
    (global-set-key (kbd "C--")            'text-scale-decrease)
    (global-set-key (kbd "<C-wheel-up>")   'text-scale-increase)
    (global-set-key (kbd "<C-wheel-down>") 'text-scale-decrease)
)
```


#### Pitch {#pitch}

There're 2 modes that controls pitch, mixed-pitch-mode and fixed-pitch-mode.

```elisp
(use-package fixed-pitch
:defer t
)
```

```elisp
(use-package mixed-pitch-mode
:defer t
:hook (Custom-mode . mixed-pitch-mode)
:config
  (setq  mixed-pitch-set-height t)
)
```


### Icons {#icons}


#### All-the-icons {#all-the-icons}

[All-the-icons](https://github.com/domtronn/all-the-icons.el) is an icon set that can be used with dashboard, dired, ibuffer and other Emacs programs.

```elisp
(use-package all-the-icons
  :if (display-graphic-p))

;(use-package all-the-icons-dired
;  :hook (dired-mode . (lambda () (all-the-icons-dired-mode t))))
```

**NOTE**: In order for the icons to work it is very important that you install the Resource Fonts included in this package. Run [M-x all-the-icons-install-fonts](all-the-icons-install-fonts) to install necessary icons.


#### Nerd-icons {#nerd-icons}

[Nerd-icons](https://github.com/rainstormstudio/nerd-icons.el) is a library for easily using Nerd Font icons inside Emacs, an alternative to all-the-icons.
Run [M-x nerd-icons-install-fonts](nerd-icons-install-fonts) to install `Symbols Nerd Fonts Mono` for you.

```elisp
(use-package nerd-icons
  ;; :custom
  ;; The Nerd Font you want to use in GUI
  ;; "Symbols Nerd Font Mono" is the default and is recommended
  ;; but you can use any other Nerd Font if you want
  ;; (nerd-icons-font-family "Symbols Nerd Font Mono")
)
```


### Theme {#theme}

[Doom-themes](https://github.com/hlissner/emacs-doom-themes) is a great set of themes with a lot of variety and support for many different Emacs modes. Taking a look at the [screenshots](https://github.com/hlissner/emacs-doom-themes/tree/screenshots) might help you decide which one you like best.  You can also run `M-x counsel-load-theme` to choose between them easily.


### Spacing {#spacing}


#### Margin {#margin}

We have 3 modes that can help centering text in a window.
But currently we only use olivetti mode.


##### Olivetti {#olivetti}

[Olibetti](https://github.com/rnkn/olivetti) is a simple Emacs minor mode for a nice writing environment.
Set olivetti-style to both margins and fringes for a fancy "page" look.

Note that for pages with variable-pitch fonts,
`olivetti-body-width` should be set smaller for it to look good.

```elisp
(use-package olivetti
:hook (org-mode . olivetti-mode)
  (Custom-mode . olivetti-mode)
  (help-mode . olivetti-mode)
  (dashboard-mode . olivetti-mode)
  (dashboard-mode . variable-pitch-mode)
  (olivetti-mode . visual-line-mode)
:init
  (setq-default fill-column 78)
:config
  ;If nil (the default), use the value of fill-column + 2.
  (setq olivetti-body-width nil
         olivetti-style 'fancy)
  (set-face-attribute 'olivetti-fringe nil :background "#171B24")
  (defun config/window-center (width)
    (interactive)
    (setq fill-column width)
    (olivetti-mode)
  )
  (config/leader
    "tc"  '(olivetti-mode     :wk "󰉠 Center")
  )
)
```


##### Visual-fill-column {#visual-fill-column}

[visual-fill-column](https://github.com/joostkremers/visual-fill-column)


##### Writeroom-mode {#writeroom-mode}


#### Vertical Spacing {#vertical-spacing}

[Topspace](https://github.com/trevorpogue/topspace) recenter line 1 with scrollable upper margin/padding

```elisp
(use-package topspace
:init (global-topspace-mode)
)
```


### Solaire mode {#solaire-mode}

[Solaire-mode](https://github.com/hlissner/emacs-solaire-mode) makes certain buffers grossly incandescent. Useful to distinguish the main  buffers from others.

```elisp
(use-package solaire-mode
  :hook (minibuffer-setup . solaire-mode)
        (help-mode . solaire-mode)
        (helpful-mode . solaire-mode)
        (org-export-stack-mode . solaire-mode)
  )
```


### Whitespace mode {#whitespace-mode}

[Whitespace mode](https://www.emacswiki.org/emacs/WhiteSpace) is a built in mode of emacs that visualizes whitespaces, tab symbols, indentations and related stuffs.

```elisp
(config/leader :infix "t"
  "SPC"  '(whitespace-mode  :wk "󰡭 Show Space")
)
```


### Transparency {#transparency}

Set background Transparency, according to [this page](https://www.emacswiki.org/emacs/TransparentEmacs).

```elisp
(set-frame-parameter nil 'alpha-background 96)
(add-to-list 'default-frame-alist '(alpha-background . 96))

(defun config/transparency (value)
  "Sets the transparency of the frame window. 0=transparent/100=opaque"
  (interactive "nTransparency Value 0 - 100 opaque:")
  (set-frame-parameter nil 'alpha-background value))
```


### Scroll {#scroll}

```elisp
(use-package emacs
:config
  (setq scroll-conservatively 97)
  (setq scroll-preserve-screen-position 1)
  (setq mouse-wheel-progressive-speed nil)
  ;; The following piece of code is stolen from
  ;; https://emacs-china.org/t/topic/25114/5
  (pixel-scroll-precision-mode 1)
  (setq pixel-scroll-precision-interpolate-page t)
  (defun +pixel-scroll-interpolate-down (&optional lines)
      (interactive)
      (if lines
          (pixel-scroll-precision-interpolate (* -1 lines (pixel-line-height)))
      (pixel-scroll-interpolate-down)))

  (defun +pixel-scroll-interpolate-up (&optional lines)
      (interactive)
      (if lines
          (pixel-scroll-precision-interpolate (* lines
          (pixel-line-height))))
      (pixel-scroll-interpolate-up))

  (defalias 'scroll-up-command '+pixel-scroll-interpolate-down)
  (defalias 'scroll-down-command '+pixel-scroll-interpolate-up)
)
```


### Modeline {#modeline}

[Doom-modeline](https://github.com/seagle0128/doom-modeline) is a very attractive and rich (yet still minimal) mode line configuration for Emacs.  The default configuration is quite good but you can check out the [configuration options](https://github.com/seagle0128/doom-modeline#customize) for more things you can enable or disable.

```elisp
(use-package doom-modeline
:init
  (setq
    doom-modeline-height 37
    doom-modeline-enable-word-count t)
  (doom-modeline-mode 1)
:config
  (set-face-attribute 'doom-modeline t
    :inherit 'variable-pitch
  )
)
```

**NOTE1**: [Nerd-icons](#nerd-icons) are necessary. Run [M-x nerd-icons-install-fonts](nerd-icons-install-fonts) to install the resource fonts.

**NOTE2:** [All-the-icons](#all-the-icons) hasn't been supported since 4.0.0. If you prefer all-the-icons, please use release 3.4.0, then run [M-x all-the-icons-install-fonts](all-the-icons-install-fonts) to install necessary icons.


#### Diminish {#diminish}

This package implements hiding or abbreviation of the modeline displays (lighters) of minor-modes.  With this package installed, you can add ':diminish' to any use-package block to hide that particular mode in the modeline.

```elisp
(use-package diminish)
```


### Dashboard {#dashboard}

Dashboard is an extensible startup screen showing you recent files, bookmarks, agenda items and an Emacs banner.

```elisp
(use-package dashboard
:init
  (setq initial-buffer-choice 'dashboard-open
  dashboard-image-banner-max-width 1000
  dashboard-set-heading-icons t
  dashboard-center-content t ;; set to 't' for centered content
  dashboard-set-file-icons t
  initial-buffer-choice
      (lambda () (get-buffer-create "*dashboard*"))
  dashboard-startup-banner ;; use custom image as banner
      (concat user-emacs-directory "assets/EmacsBound.xpm")
  dashboard-items '(
      (recents . 5)
      (agenda . 5 )
      (bookmarks . 3)
      (projects . 3)
      (registers . 3)
  )
  )
:config
  (dashboard-setup-startup-hook)
:bind (:map dashboard-mode-map
  ;("i" . 'dashboard-previous-line)
  ;("k" . 'dashboard-next-line)
  ("l" . 'dashboard-return)
  ("j" . 'dashboard-remove-item-under)
  )
)
```


### Tabs {#tabs}

```elisp
(use-package centaur-tabs
  :hook
    (emacs-startup . centaur-tabs-mode)
    (dired-mode . centaur-tabs-local-mode)
    (dirvish-directory-view-mode . centaur-tabs-local-mode)
    (dashboard-mode . centaur-tabs-local-mode)
    (calendar-mode . centaur-tabs-local-mode)
  :init
    (setq centaur-tabs-set-icons t
    centaur-tabs-set-modified-marker t
    centaur-tabs-modified-marker "M"
    centaur-tabs-cycle-scope 'tabs
    centaur-tabs-set-bar 'over
    centaur-tabs-enable-ido-completion nil
    )
    (centaur-tabs-mode t)
  :config
    (centaur-tabs-change-fonts "Sarasa Gothic SC" 160)
    ;; (centaur-tabs-headline-match)
    ;; (centaur-tabs-group-by-projectile-project)

)
```


### Diff {#diff}

```elisp
(use-package diff-hl
:custom-face
  (diff-hl-change ((t (:background "#2c5f72" :foreground "#77a8d9"))))
  (diff-hl-delete ((t (:background "#844953" :foreground "#f27983"))))
  (diff-hl-insert ((t (:background "#5E734A" :foreground "#a6cc70"))))
:config
  (setq diff-hl-draw-borders nil)
  (global-diff-hl-mode)
  ;(diff-hl-margin-mode)
  (add-hook 'magit-post-refresh-hook 'diff-hl-magit-post-refresh t)
)
```


### Beacon {#beacon}

```elisp
(use-package beacon
:config
  (beacon-mode)
)
```


### Minibuffer {#minibuffer}


#### Mini-frame {#mini-frame}

[Mini-Frame](https://github.com/muffinmad/emacs-mini-frame), similar to posframe, shows minibuffer in child frame on read-from-minibuffer.

```elisp
(use-package mini-frame
:config
  (setq mini-frame-detach-on-hide nil)
  ;(setq mini-frame-standalone 't)
  ;(setq mini-frame-resize-min-height 10)
  (setq mini-frame-ignore-commands
    (append mini-frame-ignore-commands
     '(evil-window-split evil-window-vsplit evil-ex)))
)
```


### Posframe {#posframe}

[Posframe](https://github.com/tumashu/posframe) can pop up a frame at point, this **posframe** is a child-frame connected to its root window's buffer.

The builtin poshandler functions are listed below:

```text
1.  `posframe-poshandler-frame-center'
2.  `posframe-poshandler-frame-top-center'
3.  `posframe-poshandler-frame-top-left-corner'
4.  `posframe-poshandler-frame-top-right-corner'
5.  `posframe-poshandler-frame-top-left-or-right-other-corner'
6.  `posframe-poshandler-frame-bottom-center'
7.  `posframe-poshandler-frame-bottom-left-corner'
8.  `posframe-poshandler-frame-bottom-right-corner'
9.  `posframe-poshandler-window-center'
10. `posframe-poshandler-window-top-center'
11. `posframe-poshandler-window-top-left-corner'
12. `posframe-poshandler-window-top-right-corner'
13. `posframe-poshandler-window-bottom-center'
14. `posframe-poshandler-window-bottom-left-corner'
15. `posframe-poshandler-window-bottom-right-corner'
16. `posframe-poshandler-point-top-left-corner'
17. `posframe-poshandler-point-bottom-left-corner'
18. `posframe-poshandler-point-bottom-left-corner-upward'
19. `posframe-poshandler-point-window-center'
```


### Holo-layer {#holo-layer}

[Holo-layer](https://github.com/manateelazycat/holo-layer) is developed based on PyQt, aiming to significantly enhance the visual experience of Emacs.
Disabled because it cannot run perfectly on hyprland.

```elisp
(use-package holo-layer
  :disabled
  :config
  (holo-layer-enable)
)
```


## Facilities {#facilities}


### Long tail {#long-tail}

```elisp
(use-package diff-mode
  :defer t
  :config
  (when (>= emacs-major-version 27)
    (set-face-attribute 'diff-refine-changed nil :extend t)
    (set-face-attribute 'diff-refine-removed nil :extend t)
    (set-face-attribute 'diff-refine-added   nil :extend t)))
```

```elisp
(use-package dired
  :defer t
  :config (setq dired-listing-switches "-alh"))
```

```elisp
(use-package eldoc
  :when (version< "25" emacs-version)
  :config (global-eldoc-mode))
```

```elisp
(use-package help
  :defer t
  :config (temp-buffer-resize-mode))
```

```elisp
(progn ;    `isearch'
  (setq isearch-allow-scroll t))
```

```elisp
(use-package lisp-mode
  :config
  (add-hook 'emacs-lisp-mode-hook 'outline-minor-mode)
  (add-hook 'emacs-lisp-mode-hook 'reveal-mode)
  (defun indent-spaces-mode ()
    (setq indent-tabs-mode nil))
  (add-hook 'lisp-interaction-mode-hook 'indent-spaces-mode))
```

```elisp
(use-package man
  :defer t
  :config (setq Man-width 80))
```

```elisp
(use-package paren
  :config (show-paren-mode))
```

```elisp
(use-package prog-mode
  :config (global-prettify-symbols-mode)
  (defun indicate-buffer-boundaries-left ()
    (setq indicate-buffer-boundaries 'left))
  (add-hook 'prog-mode-hook 'indicate-buffer-boundaries-left))
```

```elisp
(use-package recentf
  :demand t
  :config (add-to-list 'recentf-exclude "^/\\(?:ssh\\|su\\|sudo\\)?x?:"))
```

```elisp
(use-package savehist
  :config (savehist-mode))
```

```elisp
(use-package saveplace
  :when (version< "25" emacs-version)
  :config (save-place-mode))
```

```elisp
(use-package simple
  :config (column-number-mode))
```

```elisp
(use-package smerge-mode
  :defer t
  :config
  (when (>= emacs-major-version 27)
    (set-face-attribute 'smerge-refined-removed nil :extend t)
    (set-face-attribute 'smerge-refined-added   nil :extend t)))
```

```elisp
(progn ;    `text-mode'
  (add-hook 'text-mode-hook 'indicate-buffer-boundaries-left))
```

```elisp
(use-package tramp
  :defer t
  :config
  (add-to-list 'tramp-default-proxies-alist '(nil "\\`root\\'" "/ssh:%h:"))
  (add-to-list 'tramp-default-proxies-alist '("localhost" nil nil))
  (add-to-list 'tramp-default-proxies-alist
         (list (regexp-quote (system-name)) nil nil))
  (setq vc-ignore-dir-regexp
  (format "\\(%s\\)\\|\\(%s\\)"
    vc-ignore-dir-regexp
    tramp-file-name-regexp)))
```

```elisp
(use-package tramp-sh
  :defer t
  :config (cl-pushnew 'tramp-own-remote-path tramp-remote-path))
```


### Magit {#magit}

[Magit](https://github.com/magit/magit) is a VERY powerful git client.

```elisp
(use-package magit
  :defer t
  :commands (magit-add-section-hook)
  :hook (magit-mode . solaire-mode) (magit-mode . olivetti-mode)
  :config
  (magit-add-section-hook 'magit-status-sections-hook
        'magit-insert-modules
        'magit-insert-stashes
        'append))
```

-   tweaks to build magit

<!--listend-->

```cfg
[submodule "magit"]
  no-byte-compile = lisp/magit-libgit.el
```


### Treemacs {#treemacs}

```elisp
  (use-package treemacs
  :disabled
  :init
  (with-eval-after-load 'winum
    (define-key winum-keymap (kbd "M-0") #'treemacs-select-window))
  :config
  (progn
    (setq treemacs-collapse-dirs
            (if treemacs-python-executable 3 0)
          treemacs-deferred-git-apply-delay        0.5
          treemacs-directory-name-transformer      #'identity
          treemacs-display-in-side-window          t
          treemacs-eldoc-display                   'simple
          treemacs-file-event-delay                2000
          treemacs-file-extension-regex
            treemacs-last-period-regex-value
          treemacs-file-follow-delay               0.2
          treemacs-file-name-transformer           #'identity
          treemacs-follow-after-init               t
          treemacs-expand-after-init               t
          treemacs-find-workspace-method
            'find-for-file-or-pick-first
          treemacs-git-command-pipe                ""
          treemacs-goto-tag-strategy               'refetch-index
          treemacs-header-scroll-indicators        '(nil . "^^^^^^")
          treemacs-hide-dot-git-directory          t
          treemacs-indentation                     2
          treemacs-indentation-string              " "
          treemacs-is-never-other-window           nil
          treemacs-max-git-entries                 5000
          treemacs-missing-project-action          'ask
          treemacs-move-forward-on-expand          nil
          treemacs-no-png-images                   nil
          treemacs-no-delete-other-windows         t
          treemacs-project-follow-cleanup          nil
          treemacs-persist-file
            (expand-file-name ".cache/treemacs-persist"
             user-emacs-directory)
          treemacs-position                        'left
          treemacs-read-string-input               'from-child-frame
          treemacs-recenter-distance               0.1
          treemacs-recenter-after-file-follow      nil
          treemacs-recenter-after-tag-follow       nil
          treemacs-recenter-after-project-jump     'always
          treemacs-recenter-after-project-expand   'on-distance
          treemacs-litter-directories
             '("/node_modules" "/.venv" "/.cask")
          treemacs-project-follow-into-home        nil
          treemacs-show-cursor                     nil
          treemacs-show-hidden-files               t
          treemacs-silent-filewatch                nil
          treemacs-silent-refresh                  nil
          treemacs-sorting                         'alphabetic-asc
          treemacs-select-when-already-in-treemacs 'move-back
          treemacs-space-between-root-nodes        t
          treemacs-tag-follow-cleanup              t
          treemacs-tag-follow-delay                1.5
          treemacs-text-scale                      nil
          treemacs-user-mode-line-format           nil
          treemacs-user-header-line-format         nil
          treemacs-wide-toggle-width               70
          treemacs-width                           35
          treemacs-width-increment                 1
          treemacs-width-is-initially-locked       t
          treemacs-workspace-switch-cleanup        nil)

    ;; The default width and height of the icons is 22 pixels. If you are
    ;; using a Hi-DPI display, uncomment this to double the icon size.
    ;;(treemacs-resize-icons 44)

    (treemacs-follow-mode t)
    (treemacs-filewatch-mode t)
    (treemacs-fringe-indicator-mode 'always)
    (when treemacs-python-executable
      (treemacs-git-commit-diff-mode t))

    (pcase (cons (not (null (executable-find "git")))
                 (not (null treemacs-python-executable)))
      (`(t . t)
       (treemacs-git-mode 'deferred))
      (`(t . _)
       (treemacs-git-mode 'simple)))

    (treemacs-hide-gitignored-files-mode nil))
  :bind
  (:map global-map
        ("M-0"       . treemacs-select-window)
        ("C-x t 1"   . treemacs-delete-other-windows)
        ("C-x t t"   . treemacs)
        ("C-x t d"   . treemacs-select-directory)
        ("C-x t B"   . treemacs-bookmark)
        ("C-x t C-t" . treemacs-find-file)
        ("C-x t M-t" . treemacs-find-tag)))

(use-package treemacs-evil
  :after (treemacs evil)
  )

(use-package treemacs-projectile
  :after (treemacs projectile)
  )

(use-package treemacs-icons-dired
  :hook (dired-mode . treemacs-icons-dired-enable-once)
  )

(use-package treemacs-magit
  :after (treemacs magit)
  )

(use-package treemacs-persp ;;treemacs-perspective if you use perspective.el vs. persp-mode
  :after (treemacs persp-mode) ;;or perspective vs. persp-mode
  :config (treemacs-set-scope-type 'Perspectives))

(use-package treemacs-tab-bar ;;treemacs-tab-bar if you use tab-bar-mode
  :after (treemacs)
  :config (treemacs-set-scope-type 'Tabs))
```

All the el files in treemacs are in `src/elisp` and `src/extra`

```cfg
[submodule "treemacs"]
  load-path = src/elisp
  load-path = src/extra
```


### Dirvish {#dirvish}

Dropin replacement for dired.

```elisp
(use-package dirvish
:init
  ;(dirvish-override-dired-mode)
:hook
  (dired-mode . solaire-mode)
:custom
  (dirvish-quick-access-entries ;`setq' won't work for custom
    '(("h" "~/"                          "Home")
  ("d" "~/Downloads/"                "Downloads")
  ("m" "/mnt/"                       "Drives")
  ("t" "~/.local/share/Trash/files/" "TrashCan"))
  )
:config
  (dirvish-define-preview exa (file)
  "Use `exa' to generate directory preview."
  :require ("exa") ; tell Dirvish to check if we have the executable
  (when (file-directory-p file) ; we only interest in directories here
  `(shell . ("exa" "-al" "--color=always" "--icons"
    "--group-directories-first" ,file))))

  (add-to-list 'dirvish-preview-dispatchers 'exa)
  ;; (dirvish-peek-mode) ; Preview files in minibuffer
  ;; (dirvish-side-follow-mode) ; similar to `treemacs-follow-mode'
  (setq dirvish-path-separators (list "  " "  " "  "))
  (setq dirvish-mode-line-format
      '(:left (sort symlink) :right (omit yank index)))
  (setq dirvish-attributes
      '(all-the-icons file-time file-size collapse subtree-state vc-state git-msg))
  (setq delete-by-moving-to-trash t)
  (setq dired-listing-switches
      "-l --almost-all --human-readable --group-directories-first --no-group")
  (nmap dirvish-mode-map
  "?"      '(dirvish-dispatch          :wk "Dispatch")
  "TAB"    '(dirvish-subtree-toggle    :wk "Subtre-toggle")
  "q"      '(dirvish-quit              :wk "Quit")
  "h"      '(dired-up-directory        :wk "Up-dir")
  "l"      '(dired-find-file           :wk "Open/Toggle")
  "a"      '(dirvish-quick-access      :wk "Access")
  "f"      '(dirvish-file-info-menu    :wk "File Info Menu")
  "y"      '(dirvish-yank-menu         :wk "Yank Menu")
  "N"      '(dirvish-narrow            :wk "Narrow")
  ;         `dired-view-file'
  "v"      '(dirvish-vc-menu           :wk "View-file")
  ;         `dired-sort-toggle-or-edit'
  "s"      '(dirvish-quicksort         :wk "Quick-sort")

  "M-f"    '(dirvish-history-go-forward  :wk "History-forward")
  "M-b"    '(dirvish-history-go-backward :wk "History-back")
  "M-l"    '(dirvish-ls-switches-menu    :wk "ls Switch Menu")
  "M-m"    '(dirvish-mark-menu           :wk "Mark Menu")
  "M-t"    '(dirvish-layout-toggle       :wk "Layout-toggle")
  "M-s"    '(dirvish-setup-menu          :wk "Setup-Menu")
  "M-e"    '(dirvish-emerge-menu         :wk "Emerge-Menu")
  "M-j"    '(dirvish-fd-jump             :wk "fd-jump")
  )
)
```

```elisp
(use-package diredfl
  :hook
  ((dired-mode . diredfl-mode)
   ;; highlight parent and directory preview as well
   (dirvish-directory-view-mode . diredfl-mode))
  :config
  (set-face-attribute 'diredfl-dir-name nil :bold t)
)
```

Tweaks to build dirvish. Load dirvish and its extensions.

```cfg
[submodule "dirvish"]
  load-path = .
  load-path = extensions
```


### Helpful {#helpful}

[Helpful](https://github.com/Wilfred/helpful) adds a lot of very helpful information to Emacs' `describe-` command buffers.
For example, if you use `describe-function`, you will not only get the documentation about the function,
you will also see the source code of the function and where it gets used in other places in the Emacs configuration.
It is very useful for figuring out how things work in Emacs.

```elisp
(use-package helpful
:bind
   ([remap describe-key]      . helpful-key)
   ([remap describe-command]  . helpful-command)
   ([remap describe-variable] . helpful-variable)
   ([remap describe-function] . helpful-callable)
   ("C-h F" . describe-face)
   ("C-h K" . describe-keymap)
)
```


### Info+ {#info-plus}

```elisp
(use-package info+
:defer t
:config
)
```

```elisp
(use-package info-colors
:config
  (add-hook 'Info-selection-hook 'info-colors-fontify-node)
  (add-hook 'Info-mode-hook 'olivetti-mode)
  (add-hook 'Info-mode-hook 'mixed-pitch-mode)
)
```


### Marginalia {#marginalia}

[Marginalia](https://github.com/minad/marginalia) enriches existing commands with completion annotations

```elisp
(use-package marginalia
:general
  (:keymaps 'minibuffer-local-map
   "M-A" 'marginalia-cycle)
:custom
  (marginalia-max-relative-age 0)
  (marginalia-align 'right)
:init
  (marginalia-mode)
)
```


### Vertico {#vertico}


#### Vertico {#vertico}

[Vertico](https://github.com/minad/vertico#extensions) provides a performant and minimalistic vertical completion UI based on the default completion system.

```elisp
(use-package vertico
  :init
  ;; Different scroll margin
  (setq vertico-scroll-margin 1)
  ;; Show more candidates
  (setq vertico-count 20)
  ;; Grow and shrink the Vertico minibuffer
  (setq vertico-resize nil)
  ;; Optionally enable cycling for `vertico-next' and `vertico-previous'.
  (setq vertico-cycle t)
  ;; use Vertico as an in-buffer completion UI
  (setq completion-in-region-function 'consult-completion-in-region)
  (vertico-mode 1)
)
```

tweaks to build vertico

```cfg
[submodule "vertico"]
  load-path = .
  load-path = extensions
```


#### Orderless {#orderless}

[Orderless](https://github.com/oantolin/orderless) provides a completion style that divides the pattern into space-separated components, and matches candidates that match all of the components in any order.
Each component can match in any one of several ways: literally, as a regexp, as an initialism, in the flex style, or as multiple word prefixes. By default, regexp and literal matches are enabled.

```elisp
(use-package orderless
  :init
  (setq completion-styles '(orderless))
  (setq orderless-component-separator
    #'orderless-escapable-split-on-space)
  (setq orderless-matching-styles
    '(orderless-initialism orderless-prefixes orderless-regexp))
  )
```


#### Vertico-directory {#vertico-directory}

```elisp
(use-package vertico-directory
  :after vertico
  ;; More convenient directory navigation commands
  :bind (:map vertico-map
        ("RET" . vertico-directory-enter)
        ("DEL" . vertico-directory-delete-char)
        ("M-DEL" . vertico-directory-delete-word))
  ;; Tidy shadowed file names
  :hook (rfn-eshadow-update-overlay . vertico-directory-tidy))
```


#### Vertico-multiform {#vertico-multiform}

Vertico-multiform configures Vertico modes per command or completion category.

```elisp
(use-package vertico-multiform
  :after vertico
  :config (vertico-multiform-mode)
)
```


#### Vertico-posframe {#vertico-posframe}

[Vertico-posframe](https://github.com/tumashu/vertico-posframe) is an vertico extension, which lets vertico use posframe to show its candidate menu.

```elisp
(use-package vertico-posframe
;:disabled
:after vertico-multiform
:init
  (setq vertico-posframe-poshandler
        'posframe-poshandler-frame-top-center)
  (setq vertico-count 15
        vertico-posframe-border-width 3
        vertico-posframe-width 140
        vertico-resize nil)
  (setq vertico-posframe-parameters
       '((left-fringe . 20)
         (right-fringe . 20)))
  (setq vertico-multiform-commands '(
        (execute-extended-command ; M-x
          (vertico-posframe-poshandler .
             posframe-poshandler-frame-top-center)
          (vertico-posframe-width . 120)
        )
        (meow-visit
          (vertico-posframe-poshandler .
             posframe-poshandler-window-top-right-corner)
          (vertico-posframe-width . 50)
        )
        (meow-yank-pop; M-x
          (vertico-posframe-poshandler .
             posframe-poshandler-point-window-center)
          (vertico-posframe-width . 50)
        )
        (find-file
          (vertico-count . 25)
          (vertico-posframe-width . 70)
          (vertico-posframe-poshandler .
             posframe-poshandler-window-center)
        )
        (org-insert-link; M-x
          (vertico-posframe-poshandler .
             posframe-poshandler-point-top-left-corner)
          (vertico-posframe-width . 70)
        )
        (consult-line
          (vertico-posframe-poshandler .
             posframe-poshandler-frame-top-center)
          (vertico-posframe-fallback-mode . vertico-buffer-mode)
          (vertico-posframe-width . 70))
        (t
          (vertico-posframe-poshandler .
             posframe-poshandler-frame-top-center)
          (vertico-posframe-width . 120)
        )
  ))

:config
  (vertico-multiform-mode 1)
  (vertico-posframe-mode 1)
)
```


#### Savehist {#savehist}

```elisp
;; Persist history over Emacs restarts. Vertico sorts by history position.
  (use-package savehist
:init
(savehist-mode))
```

```elisp
;; A few more useful configurations...
  (use-package emacs
:init
;; Add prompt indicator to `completing-read-multiple'.
;; We display [CRM<separator>], e.g., [CRM,] if the separator is a comma.
(defun crm-indicator (args)
(cons (format "[CRM%s] %s"
    (replace-regexp-in-string
    "\\`\\[.*?]\\*\\|\\[.*?]\\*\\'" ""
    crm-separator)
    (car args))
  (cdr args)))
(advice-add #'completing-read-multiple :filter-args #'crm-indicator)

;; Do not allow the cursor in the minibuffer prompt
(setq minibuffer-prompt-properties
    '(read-only t cursor-intangible t face minibuffer-prompt))
(add-hook 'minibuffer-setup-hook #'cursor-intangible-mode)

;; Emacs 28: Hide commands in M-x which do not work in the current mode.
;; Vertico commands are hidden in normal buffers.
;; (setq read-extended-command-predicate
;;       #'command-completion-default-include-p)
;; Enable recursive minibuffers
(setq enable-recursive-minibuffers t))
```


### Consult {#consult}

[Consult](https://github.com/minad/consult) provides search and navigation commands based on the Emacs completion function completing-read.

```elisp
(use-package consult
  ;; Replace bindings. Lazily loaded due by `use-package'.
  :bind (;; C-c bindings in `mode-specific-map'
   ("C-c M-x" . consult-mode-command)
   ("C-c h" . consult-history)
   ;("C-c k" . consult-kmacro)
   ("C-c m" . consult-man)
   ;("C-c i" . consult-info)
   ([remap Info-search] . consult-info)
   ;; C-x bindings in `ctl-x-map'
   ("C-x M-:" . consult-complex-command)     ;; orig. repeat-complex-command
   ("C-x b" . consult-buffer)                ;; orig. switch-to-buffer
   ("C-x 4 b" . consult-buffer-other-window) ;; orig. switch-to-buffer-other-window
   ("C-x 5 b" . consult-buffer-other-frame)  ;; orig. switch-to-buffer-other-frame
   ("C-x r b" . consult-bookmark)            ;; orig. bookmark-jump
   ("C-x p b" . consult-project-buffer)      ;; orig. project-switch-to-buffer
   ;; Custom M-# bindings for fast register access
   ("M-#" . consult-register-load)
   ("M-'" . consult-register-store)          ;; orig. abbrev-prefix-mark (unrelated)
   ("C-M-#" . consult-register)
   ;; Other custom bindings
   ("M-y" . consult-yank-pop)                ;; orig. yank-pop
   ;; M-g bindings in `goto-map'
   ("M-g e" . consult-compile-error)
   ("M-g f" . consult-flymake)               ;; Alternative: consult-flycheck
   ("M-g g" . consult-goto-line)             ;; orig. goto-line
   ("M-g M-g" . consult-goto-line)           ;; orig. goto-line
   ("M-g o" . consult-outline)               ;; Alternative: consult-org-heading
   ("M-g m" . consult-mark)
   ("M-g k" . consult-global-mark)
   ("M-g i" . consult-imenu)
   ("M-g I" . consult-imenu-multi)
   ;; M-s bindings in `search-map'
   ("M-s d" . consult-find)
   ("M-s D" . consult-locate)
   ("M-s g" . consult-grep)
   ("M-s G" . consult-git-grep)
   ("M-s r" . consult-ripgrep)
   ("M-s l" . consult-line)
   ("M-s L" . consult-line-multi)
   ("M-s k" . consult-keep-lines)
   ("M-s u" . consult-focus-lines)
   ;; Isearch integration
   ("M-s e" . consult-isearch-history)
   :map isearch-mode-map
   ("M-e" . consult-isearch-history)         ;; orig. isearch-edit-string
   ("M-s e" . consult-isearch-history)       ;; orig. isearch-edit-string
   ("M-s l" . consult-line)                  ;; needed by consult-line to detect isearch
   ("M-s L" . consult-line-multi)            ;; needed by consult-line to detect isearch
   ;; Minibuffer history
   :map minibuffer-local-map
   ("M-s" . consult-history)                 ;; orig. next-matching-history-element
   ("M-r" . consult-history))                ;; orig. previous-matching-history-element

  ;; Enable automatic preview at point in the *Completions* buffer. This is
  ;; relevant when you use the default completion UI.
  :hook (completion-list-mode . consult-preview-at-point-mode)

  ;; The :init configuration is always executed (Not lazy)
  :init

  ;; Optionally configure the register formatting. This improves the register
  ;; preview for `consult-register', `consult-register-load',
  ;; `consult-register-store' and the Emacs built-ins.
  (setq register-preview-delay 0.5
  register-preview-function #'consult-register-format)

  ;; Optionally tweak the register preview window.
  ;; This adds thin lines, sorting and hides the mode line of the window.
  (advice-add #'register-preview :override #'consult-register-window)

  ;; Use Consult to select xref locations with preview
  (setq xref-show-xrefs-function #'consult-xref
  xref-show-definitions-function #'consult-xref)

  ;; Configure other variables and modes in the :config section,
  ;; after lazily loading the package.
  :config

  ;; Optionally configure preview. The default value
  ;; is 'any, such that any key triggers the preview.
  ;; (setq consult-preview-key 'any)
  ;; (setq consult-preview-key "M-.")
  ;; (setq consult-preview-key '("S-<down>" "S-<up>"))
  ;; For some commands and buffer sources it is useful to configure the
  ;; :preview-key on a per-command basis using the `consult-customize' macro.
  (consult-customize
   consult-theme :preview-key '(:debounce 0.2 any)
   consult-ripgrep consult-git-grep consult-grep
   consult-bookmark consult-recent-file consult-xref
   consult--source-bookmark consult--source-file-register
   consult--source-recent-file consult--source-project-recent-file
   ;; :preview-key "M-."
   :preview-key '(:debounce 0.4 any))

  ;; Optionally configure the narrowing key.
  ;; Both < and C-+ work reasonably well.
  (setq consult-narrow-key "<") ;; "C-+"

  ;; Optionally make narrowing help available in the minibuffer.
  ;; You may want to use `embark-prefix-help-command' or which-key instead.
  ;; (define-key consult-narrow-map (vconcat consult-narrow-key "?") #'consult-narrow-help)

  ;; By default `consult-project-function' uses `project-root' from project.el.
  ;; Optionally configure a different project root function.
  ;;;; 1. project.el (the default)
  ;; (setq consult-project-function #'consult--default-project--function)
  ;;;; 2. vc.el (vc-root-dir)
  ;; (setq consult-project-function (lambda (_) (vc-root-dir)))
  ;;;; 3. locate-dominating-file
  ;; (setq consult-project-function (lambda (_) (locate-dominating-file "." ".git")))
  ;;;; 4. projectile.el (projectile-project-root)
  ;; (autoload 'projectile-project-root "projectile")
  ;; (setq consult-project-function (lambda (_) (projectile-project-root)))
  ;;;; 5. No project support
  ;; (setq consult-project-function nil)
)
```


### Blink search {#blink-search}

[Blink-search](https://github.com/manateelazycat/blink-search) is the fastest multi-source search framework for Emacs.

```elisp
(use-package blink-search
:defer t
:config
  (setq blink-search-enable-posframe t)
)
```


### Color-rg {#color-rg}

[Color-rg](https://github.com/manateelazycat/color-rg) is a search and refactoring tool based on ripgrep.

```elisp
(use-package color-rg
:defer t
:config
  (general-def isearch-mode-map
    "M-s M-s" 'isearch-toggle-color-rg
  )
)
```


### Popweb {#popweb}


## Coding {#coding}


### LSP-bridge {#lsp-bridge}

[lsp-bridge](https://github.com/manateelazycat/lsp-bridge) builds a high-speed cache between Emacs and the LSP server.

```elisp
(use-package lsp-bridge
:init
  (global-lsp-bridge-mode)
:config
  ;(set-face-attributes 'lsp-bridge-alive-mode-line nil
  ;  :inherit 'variable-pitch
  ;)
)
```

tweaks to build lsp-bridge

```cfg
[submodule "lsp-bridge"]
  load-path = .
  load-path = acm
  load-path = core
```


### Treesit {#treesit}

You can find the addresses of language parsers at [treesitter's official doc](https://tree-sitter.github.io/tree-sitter/).

```elisp
(use-package treesit
:commands (treesit-install-language-grammar
           config/treesit-install-all-languages)
:init
  (setq treesit-language-source-alist
    '((bash . ("https://github.com/tree-sitter/tree-sitter-bash"))
      (c . ("https://github.com/tree-sitter/tree-sitter-c"))
      (cpp . ("https://github.com/tree-sitter/tree-sitter-cpp"))
      (css . ("https://github.com/tree-sitter/tree-sitter-css"))
      (cmake . ("https://github.com/uyha/tree-sitter-cmake"))
      (common-lisp .
        ("https://github.com/theHamsta/tree-sitter-commonlisp"))
      (csharp     .
        ("https://github.com/tree-sitter/tree-sitter-c-sharp.git"))
      (dockerfile .
        ("https://github.com/camdencheek/tree-sitter-dockerfile"))
      (elisp . ("https://github.com/Wilfred/tree-sitter-elisp"))
      (go . ("https://github.com/tree-sitter/tree-sitter-go"))
      (gomod      .
        ("https://github.com/camdencheek/tree-sitter-go-mod.git"))
      (html . ("https://github.com/tree-sitter/tree-sitter-html"))
      (java       .
        ("https://github.com/tree-sitter/tree-sitter-java.git"))
      (javascript .
        ("https://github.com/tree-sitter/tree-sitter-javascript"))
      (json . ("https://github.com/tree-sitter/tree-sitter-json"))
      (lua . ("https://github.com/Azganoth/tree-sitter-lua"))
      (make . ("https://github.com/alemuller/tree-sitter-make"))
      (markdown .
        ("https://github.com/MDeiml/tree-sitter-markdown" nil
        "tree-sitter-markdown/src"))
      (ocaml .
          ("https://github.com/tree-sitter/tree-sitter-ocaml" nil
          "ocaml/src"))
      (org . ("https://github.com/milisims/tree-sitter-org"))
      (python . ("https://github.com/tree-sitter/tree-sitter-python"))
      (php . ("https://github.com/tree-sitter/tree-sitter-php"))
      (typescript .
          ("https://github.com/tree-sitter/tree-sitter-typescript" nil
          "typescript/src"))
      (tsx .
          ("https://github.com/tree-sitter/tree-sitter-typescript" nil
          "tsx/src"))
      (ruby . ("https://github.com/tree-sitter/tree-sitter-ruby"))
      (rust . ("https://github.com/tree-sitter/tree-sitter-rust"))
      (sql . ("https://github.com/m-novikov/tree-sitter-sql"))
      (vue . ("https://github.com/merico-dev/tree-sitter-vue"))
      (yaml . ("https://github.com/ikatyang/tree-sitter-yaml"))
      (toml . ("https://github.com/tree-sitter/tree-sitter-toml"))
      (zig . ("https://github.com/GrayJack/tree-sitter-zig"))))
:config
(defun config/treesit-install-all-languages ()
  "Install all languages specified by `treesit-language-source-alist'."
  (interactive)
  (let ((languages (mapcar 'car treesit-language-source-alist)))
    (dolist (lang languages)
      (treesit-install-language-grammar lang)
      (message "`%s' parser was installed." lang)
      (sit-for 0.75)))))
;; stolen from lazycat
(setq major-mode-remap-alist
      '((c-mode          . c-ts-mode)
        (c++-mode        . c++-ts-mode)
        (cmake-mode      . cmake-ts-mode)
        (conf-toml-mode  . toml-ts-mode)
        (css-mode        . css-ts-mode)
        (js-mode         . js-ts-mode)
        (js-json-mode    . json-ts-mode)
        (python-mode     . python-ts-mode)
        (sh-mode         . bash-ts-mode)
        (typescript-mode . typescript-ts-mode)
        (rust-mode       . rust-ts-mode)
        ))

(add-hook 'markdown-mode-hook #'(lambda ()
          (treesit-parser-create 'markdown)))

(add-hook 'web-mode-hook #'(lambda ()
           (let ((file-name (buffer-file-name)))
             (when file-name
         (treesit-parser-create
          (pcase (file-name-extension file-name)
            ("vue" 'vue)
            ("html" 'html)
            ("php" 'php))))
             )))

(add-hook 'emacs-lisp-mode-hook #'(lambda () (treesit-parser-create 'elisp)))
(add-hook 'ielm-mode-hook #'(lambda () (treesit-parser-create 'elisp)))
(add-hook 'json-mode-hook #'(lambda () (treesit-parser-create 'json)))
(add-hook 'go-mode-hook #'(lambda () (treesit-parser-create 'go)))
(add-hook 'java-mode-hook #'(lambda () (treesit-parser-create 'java)))
(add-hook 'java-ts-mode-hook #'(lambda () (treesit-parser-create 'java)))
(add-hook 'php-mode-hook #'(lambda () (treesit-parser-create 'php)))
(add-hook 'php-ts-mode-hook #'(lambda () (treesit-parser-create 'php)))

```

[Treesit-auto](https://github.com/renzmann/treesit-auto) automatically installs and uses tree-sitter major modes in Emacs 29+. If the tree-sitter version can't be used, fall back to the original major mode.
Disabled because I use lazycat's method instead.

```elisp
(use-package treesit-auto
:disabled
:config
  (global-treesit-auto-mode))
```


### UI {#ui}


#### Line Number {#line-number}

```elisp
(use-package emacs
:custom-face
  (line-number ((t (
    :weight normal :slant normal :foreground "LightSteelBlue4"
    :inherit default))))
  (line-number-current-line ((t (
    :inherit (hl-line default) :slant normal :foreground "#ffcc66"))))
:hook (prog-mode . config/toggle-line-number-absolute)
:config
  (defun config/toggle-line-number-nil ()
    (interactive)
    (setq display-line-numbers nil)
  )
  (defun config/toggle-line-number-absolute ()
    (interactive)
    (setq display-line-numbers t)
  )
  (defun config/toggle-line-number-relative ()
    (interactive)
    (setq display-line-numbers 'relative)
  )
  (defun config/toggle-line-number-visual ()
    (interactive)
    (setq display-line-numbers 'visual)
  )
  (config/leader :infix "tl"
    ""    '(nil                                :wk "  Line Number ")
    "DEL" '(which-key-undo                     :wk "󰕍 Undo key   ")
    "n"   '(config/toggle-line-number-nil      :wk "󰅖 Nil        ")
    "a"   '(config/toggle-line-number-absolute :wk "󰱇 Absolute   ")
    "r"   '(config/toggle-line-number-relative :wk "󰰠 Relative   ")
    "v"   '(config/toggle-line-number-visual   :wk " Visual     ")
    "h"   '(hl-line-mode                       :wk "󰸱 Hl-line")
  )
)
```


#### Rainbow-mode {#rainbow-mode}

This minor mode sets background color to strings that match color names.

```elisp

```


#### Rainbow-Delimiters {#rainbow-delimiters}

[Rainbow-delimiters](https://github.com/Fanael/rainbow-delimiters) is a "rainbow parentheses"-like mode which highlights
parentheses, brackets, and braces according to their depth.

```elisp
(use-package rainbow-delimiters
:hook (prog-mode . rainbow-delimiters-mode)
)
```


#### Highlight-Indent-Guides {#highlight-indent-guides}

[Highlight-Indent-Guides](https://github.com/DarthFennec/highlight-indent-guides) is a minor mode that highlights indentation levels via font-lock.

```elisp
(use-package highlight-indent-guides
:hook (prog-mode . highlight-indent-guides-mode)
:config
  (setq highlight-indent-guides-method 'bitmap
        highlight-indent-guides-character 9474
        highlight-indent-guides-auto-enabled nil
  )
  (set-face-attribute 'highlight-indent-guides-character-face nil
    :foreground "#3b445f")
  (set-face-attribute 'highlight-indent-guides-top-character-face nil
    :foreground "#ffcc66")

)
```


### Smartparens {#smartparens}

[Smartparens](https://github.com/Fuco1/smartparens) is minor mode for Emacs that deals with parens pairs
and tries to be smart about it.

```elisp
(use-package smartparens
:config
  (smartparens-global-mode)
)
```


### Fingertip {#fingertip}

fingertip.el is a plugin that provides grammatical edit base on treesit

```elisp
(use-package fingertip
:config
  (dolist (hook (list
  'c-mode-common-hook 'c-mode-hook 'c++-mode-hook
  'c-ts-mode-hook 'c++-ts-mode-hook
  'cmake-ts-mode-hook
  'java-mode-hook
  'haskell-mode-hook
  'emacs-lisp-mode-hook
           'lisp-interaction-mode-hook 'lisp-mode-hook
  'maxima-mode-hook
  'ielm-mode-hook
  'bash-ts-mode-hook 'sh-mode-hook
  'makefile-gmake-mode-hook
  'php-mode-hook
  'python-mode-hook 'python-ts-mode-hook
  'js-mode-hook
  'go-mode-hook
  'qml-mode-hook
  'jade-mode-hook
  'css-mode-hook 'css-ts-mode-hook
  'ruby-mode-hook
  'coffee-mode-hook
  'rust-mode-hook 'rust-ts-mode-hook
  'qmake-mode-hook
  'lua-mode-hook
  'swift-mode-hook
  'web-mode-hook
  'markdown-mode-hook
  'llvm-mode-hook
  'conf-conf-mode-hook 'conf-ts-mode-hook
  'nim-mode-hook
  'typescript-mode-hook 'typescript-ts-mode-hook
  'js-ts-mode-hook 'json-ts-mode-hook
  ))
  (add-hook hook #'(lambda () (fingertip-mode 1))))
  (general-def
    :keymaps 'fingertip-mode-map
"(" 'fingertip-open-round
"[" 'fingertip-open-bracket
"{" 'fingertip-open-curly
")" 'fingertip-close-round
"]" 'fingertip-close-bracket
"}" 'fingertip-close-curly
"=" 'fingertip-equal

"%" 'fingertip-match-paren
"\"" 'fingertip-double-quote
"'" 'fingertip-single-quote

"SPC" 'fingertip-space
"RET" 'fingertip-newline

"M-o" 'fingertip-backward-delete
"C-d" 'fingertip-forward-delete
"C-k" 'fingertip-kill

"M-\"" 'fingertip-wrap-double-quote
"M-'" 'fingertip-wrap-single-quote
"M-[" 'fingertip-wrap-bracket
"M-{" 'fingertip-wrap-curly
"M-(" 'fingertip-wrap-round
"M-)" 'fingertip-unwrap

"M-p" 'fingertip-jump-right
"M-n" 'fingertip-jump-left
"M-:" 'fingertip-jump-out-pair-and-newline

"C-j" 'fingertip-jump-up
  )
)

```


### Aggressive-Indent {#aggressive-indent}

[Aggressive-indent-mode](https://github.com/Malabarba/aggressive-indent-mode) is a minor mode that keeps your code always
indented.  It reindents after every change, making it more reliable
than \`electric-indent-mode'.

```elisp
(use-package aggressive-indent
:config
  (global-aggressive-indent-mode 1)
)
```


### YASnippet {#yasnippet}

[YASnippet](https://github.com/joaotavora/yasnippet) is a template system for Emacs. It allows you to type an abbreviation and automatically expand it into function templates.

```elisp
(use-package yasnippet
  :init
  (yas-global-mode 1)
)
```


### AAS {#aas}

[AAS (Auto Activating Snippets)](https://github.com/ymarco/auto-activating-snippets) implements an engine for auto-expanding snippets. It is done by tracking your inputted chars along a tree until you complete a registered key sequence.

```elisp
(use-package aas
  :hook (LaTeX-mode . aas-activate-for-major-mode)
  :hook (org-mode . aas-activate-for-major-mode)
  :config
  (aas-set-snippets 'text-mode
    ;; expand unconditionally
    ";o-" "ō"
    ";i-" "ī"
    ";a-" "ā"
    ";u-" "ū"
    ";e-" "ē")
  (aas-set-snippets 'latex-mode
    ;; set condition!
    :cond #'texmathp ; expand only while in math
    "supp" "\\supp"
    "On" "O(n)"
    "O1" "O(1)"
    "Olog" "O(\\log n)"
    "Olon" "O(n \\log n)"
    ;; Use YAS/Tempel snippets with ease!
    "amin" '(yas "\\argmin_{$1}") ; YASnippet snippet shorthand form
    "amax" '(tempel "\\argmax_{" p "}") ; Tempel snippet shorthand form
    ;; bind to functions!
    ";ig" #'insert-register
    ";call-sin"
    (lambda (angle) ; Get as fancy as you like
      (interactive "sAngle: ")
      (insert (format "%s" (sin (string-to-number angle))))))
  ;; disable snippets by redefining them with a nil expansion
  (aas-set-snippets 'latex-mode
    "supp" nil))
```


### Copilot {#copilot}

<https://github.com/zerolfx/copilot.el>


## Languages {#languages}


### Elisp {#elisp}

-   buttercup
-   elisp-def
-   elisp-demos
-   highlight-quoted
-   macrostep
-   oversee


### LaTeX {#latex}


#### AUCTeX {#auctex}

```elisp
(use-package lsp-bridge
:config
  (setq lsp-bridge-tex-lsp-server "digestif")
)
```

```elisp
(use-package auctex)
```

AucTex needs some tweaks to be built.

```cfg
[submodule "auctex"]
  load-path = .
  build-step = ./autogen.sh
  build-step = ./configure
  build-step = make
  build-step = make doc
  build-step = borg-maketexi
  build-step = borg-makeinfo
  build-step = borg-update-autoloads
```


#### CDTeX {#cdtex}


#### LAAS {#laas}

[LASS (LaTeX Auto Activating Snippets)](https://github.com/tecosaur/LaTeX-auto-activating-snippets) is a chunky set of LaTeX snippets for the auto-activating-snippets engine.

```elisp
(use-package laas
  :hook (LaTeX-mode . laas-mode))
```


## Org {#org}


### UI {#ui}


#### Fonts {#fonts}

Change the font size of different org-levels.

```elisp
(use-package org
:custom-face
  (org-latex-and-related ((t (:foreground "LightSteelBlue4" :weight bold))))
  (org-meta-line ((t (:foreground "LightSteelBlue4"))))
  (org-special-keyword ((t (:foreground "LightSteelBlue4"))))
  (org-tag ((t (:foreground "LightSteelBlue4" :weight normal))))
:hook (org-mode . mixed-pitch-mode)
:config
  (set-face-attribute 'org-level-1 nil
:family "Sarasa Gothic SC" :height 1.4 )
  (set-face-attribute 'org-level-2 nil
:family "Sarasa Gothic SC" :height 1.4 )
  (set-face-attribute 'org-level-3 nil
:family "Sarasa Gothic SC" :height 1.4 )
  (set-face-attribute 'org-level-4 nil
:family "Sarasa Gothic SC" :height 1.3 )
  (set-face-attribute 'org-level-5 nil
:family "Sarasa Gothic SC" :height 1.2 )
  (set-face-attribute 'org-level-6 nil
:family "Sarasa Gothic SC" :height 1.1 )
  (set-face-attribute 'org-document-title nil
:family "Sarasa Gothic SC" :height 2.5 :bold t)
  (set-face-attribute 'org-document-info nil
:family "Sarasa Gothic SC" :height 1.8 :bold t)
  (set-face-attribute 'org-document-info-keyword nil
    :foreground "LightSteelBlue4" :inherit 'org-document-info)
  (set-face-attribute 'org-block t
    :extend t :inherit 'fixed-pitch)
)
```


#### Bullets {#bullets}

[Org-modern](https://github.com/minad/org-modern) implements a modern style for Org buffers using font locking and text properties. The package styles `headlines`, `keywords`, `tables` and `source blocks`.

```elisp
(use-package org-modern
:hook (org-mode . org-modern-mode)
:config
   (setq org-modern-keyword
     (quote (("author" . "⛾")
       ("title" . "❖")
       ("subtitle" . "◈")
       ("html" . "󰅱 ")
       (t . t))))
   (setq org-modern-star
  ;'("◉" "○" "◈" "◇" "✳")
  '("⚀" "⚁" "⚂" "⚃" "⚄" "⚅")
  ;'("☰" "☱" "☲" "☳" "☴" "☵" "☶" "☷")
   )
   (setq org-modern-list ;; for '+' '-' '*' respectively
 '((43 . "⯌") (45 . "⮚") (42 . "⊛"))
   )
   (setq org-modern-block-fringe nil)
   (setq org-modern-todo nil)
   (setq org-modern-block-name '("⇲ " . "⇱ "))
   (set-face-attribute 'org-modern-block-name nil
:inherit 'variable-pitch)
   (setq org-modern-table nil)
)
```

[Org-superstar](https://github.com/integral-dw/org-superstar-mode) replaces the asterisk before every org-level with ascii symbols.
Disabled because org-morden is a drop-in replacement.

```elisp
(use-package org-superstar
:defer t
;:hook (org-mode . org-superstar-mode)
:init
  (setq
    ;;org-superstar-headline-bullets-list '("󰇊" "󰇋" "󰇌" "󰇍" "󰇎" "󰇏")
    org-superstar-special-todo-items t
    ;;org-ellipsis "  "
  )
)
```


#### Indent lines {#indent-lines}

[Org-bars-mode](https://github.com/tonyaldon/org-bars) is a minor mode for org-mode.
It adds bars to the virtual indentation provided by the built-in package org-indent.
Have drawbacks when using mixed-pitch-mode.

```elisp
(use-package org-bars
:defer t
:commands 'org-bars-mode
;:hook (org-mode . org-bars-mode)
:custom-face
  (org-visual-indent-blank-pipe-face ((t (:background "#1f2430" :foreground "#1f2430" :height 0.1 :width extra-expanded))))
  (org-visual-indent-pipe-face ((t (:background "slate gray" :foreground "slate gray" :height 0.1))))
:config
  (setq org-bars-color-options '(
  :desaturate-level-faces 100
  :darken-level-faces 10
  ))
  (setq org-bars-extra-pixels-height 25)
  (setq org-bars-stars '(:empty "" :invisible "" :visible ""))
)
```

[Org-visual-outline](https://github.com/legalnonsense/org-visual-outline) does the same as Org-bars-mode. It is split into two independent packages:

Org-dynamic-bullets
: handles the dynamic bullets. `Brokes highlight`.

Org-visual-indent
: adds vertical lines to org-indent. `Does better than Org-bars-mode`

<!--listend-->

```elisp
(use-package org-visual-outline
:hook (org-mode . org-visual-indent-mode)
)
```


#### valign {#valign}

This package provides visual alignment for Org Mode, Markdown and table.el tables on GUI Emacs. It can properly align tables containing variable-pitch font, CJK characters and images. Meanwhile, the text-based alignment generated by Org mode (or Markdown mode) is left untouched.

```elisp
(use-package valign
:hook (org-mode . valign-mode)
:config
  (setq valign-fancy-bar t)
)
```


#### Highlight TODO {#highlight-todo}

```elisp
(use-package hl-todo
  :init
  (hl-todo-mode)
)
```


#### Org-appear {#org-appear}

[Org-appear](https://github.com/awth13/org-appearAutomatically) disaply emphasis markers and links when the cursor is on them.

```elisp
(use-package org-appear
  :hook (org-mode . org-appear-mode)
  :init
  (setq org-appear-autoemphasis  t
        ;org-appear-autolinks t
        org-appear-autosubmarkers t
        org-appear-autoentities t
        org-appear-autokeywords t
        org-appear-inside-latex t
        org-hide-emphasis-markers t
  )
)
```


### Blocks {#blocks}


#### Code block {#code-block}

```elisp
(use-package org
:init
  (setq electric-indent-mode nil)
:config
  (setq org-src-tab-acts-natively t)
  (setq org-src-preserve-indentation nil)
)
```

[Org-auto-tangle](https://github.com/yilkalargaw/org-auto-tangle) Automatically and Asynchronously tangles org files on save.
Adding the option `#+auto_tangle: t` in an org file to auto-tangle.
Or setting `org-auto-tangle-default` to t to configure auto-tangle as the default behavior for all org buffers. In this case, it can be disabled for some buffers by adding `#+auto_tangle: nil`.

```elisp
(use-package org-auto-tangle
:hook (org-mode . org-auto-tangle-mode)
)
```


#### Table-block {#table-block}

[Valign](https://github.com/casouri/valign) provides visual alignment for Org Mode, Markdown and table.el tables on GUI Emacs. It can properly align tables containing variable-pitch font, CJK characters and images. Meanwhile, the text-based alignment generated by Org mode (or Markdown mode) is left untouched.
Disabled cause org-modern already does the job.

[Orgtbl-aggregate](https://github.com/tbanel/orgaggregate) can creat a new table by computing sums, averages, and so on, out of material from the first table.


### Images {#images}


#### Org-download {#org-download}

```elisp
(use-package org-download
  :defer t
  :after org
)
```


#### PlantUML {#plantuml}

```elisp
(use-package plantuml-mode
  :defer t
  :mode ("\\.plantuml\\'" . plantuml-mode)
  :init
  ;; enable plantuml babel support
  (add-to-list 'org-src-lang-modes '("plantuml" . plantuml))
  (org-babel-do-load-languages 'org-babel-load-languages
                               (append org-babel-load-languages
                                       '((plantuml . t))))
  :config
  (setq org-plantuml-exec-mode 'plantuml)
  (setq org-plantuml-executable-path "plantuml")
  (setq plantuml-executable-path "plantuml")
  (setq plantuml-default-exec-mode 'executable)
  ;; set default babel header arguments
  (setq org-babel-default-header-args:plantuml
        '((:exports . "results")
          (:results . "file")
          ))
)
```


### Notes {#notes}


#### Org-capture {#org-capture}

```elisp
(use-package org-capture
:after org
:bind (("C-c c" . org-capture))
:init
  (setq org-directory "~/Org")
:config
  ;; if file path is not absolute
  ;; it is treated as relative path to org-directory
  (defun org-hugo-new-subtree-post-capture-template ()
    "Returns `org-capture' template string for new Hugo post.
     See `org-capture-templates' for more information."
    (let* ((title (read-from-minibuffer "Post Title: "))
       (fname (org-hugo-slug title)))
    (mapconcat #'identity
        `(,(concat "* TODO " title) ":PROPERTIES:"
          ,(concat ":EXPORT_FILE_NAME: " fname) ":END:" "%?\n")
          ;Place the cursor here finally
        "\n")))
  (setq org-capture-templates (append org-capture-templates '(
    ("j" "Journal" entry
      (file+datetree "journal.org")
      "* %U - %?\n")
    ("i" "Inbox" entry
      (file "inbox.org")
      "* %U - %? %^g\n")
   '("h" "Hugo post" entry
      (file+olp "capture.org" "Notes")
      (function org-hugo-new-subtree-post-capture-template))
  )))
)
```


#### Org-super-links {#org-super-links}

```elisp
(use-package org-super-links
:after org
:bind (("C-c s s" . org-super-links-link)
       ("C-c s l" . org-super-links-store-link)
       ("C-c s C-l" . org-super-links-insert-link))
)
```


#### Org-roam {#org-roam}

```elisp
(use-package org-roam
:after org
:defer t
:init
  (setq org-roam-directory (file-truename "~/roam"))
  (setq org-roam-v2-ack t)
  (setq org-roam-capture-templates '(
     ("d" "default" plain "%?"
       :target (file+head
         "%<%Y%m%d%H>-${slug}.org"
         "#+title: ${title}\n#+filetags: \n")
       :unnarrowed t)
     ("b" "book notes" plain "%?"
       :target (file+head
         "book/book%<%Y%m%d%H>-${slug}.org"
         "#+title: ${title}\n#+filetags: :bookreading: \n\n")
       :unnarrowed t)
     ("c" "company" plain "%?"
       :target (file+head
         "company/company%<%Y%m%d%H>-${slug}.org"
         "#+title: ${title}\n#filetags: :compnay: \n\n")
       :unnarrowed t)
     ("i" "industry" plain "%?"
       :target (file+head
         "industry/industry%<%Y%m%d%H>-${slug}.org"
         "#+title:${slug}\n#+filetags: :industry: \n\n")
       :unnarrowed t)
     ("m" "marketing" plain "%?"
       :target (file+head
         "marketing/marketing%<%Y%m%d%H%M%S>-${slug}.org"
         "#+title: ${title}\n#+filetags: :marketing: \n\n")
       :unnarrowed t)
     ("p" "project" plain "%?"
       :target (file+head
         "project/project%<%Y%m%d%H>-${slug}.org"
         "#+title: ${title}\n#+filetags: :project: \n\n - tag ::")
       :unnarrowed t)
     ("r" "reference" plain "%?"
       :target (file+head
         "<%Y%m%d%H>-${slug}.org"
         "#+title: {$title}\n%filetags: reference \n\n -tag ::")
       :unarrowed t)))
)
```


### Agenda {#agenda}


#### TODOs {#todos}

```elisp
(use-package org
:config
(setq org-todo-keywords '(
     (sequence "TODO(t)" "NEXT(n)" "|" "DONE(d)")
     (sequence "WAIT(w@/!)" "HOLD(h@/!)" "|" "CNCL(c@/!)")
     (sequence "SOMEDAY")
))
(setq org-todo-state-tags-triggers
   '(("CNCL" ("CNCL" . t))
     ("WAIT" ("WAIT" . t))
     ("HOLD" ("WAIT") ("HOLD" . t))
     (done ("WAIT") ("HOLD"))
     ("TODO" ("WAIT") ("CNCL") ("HOLD"))
     ("NEXT" ("WAIT") ("CNCL") ("HOLD"))
     ("DONE" ("WAIT") ("CNCL") ("HOLD"))))
)

;;selecting keys from the fast todo selection key menu
(setq org-use-fast-todo-selection t
      org-enforce-todo-dependencies t
      org-log-done 'time
)
```


#### Org-agenda {#org-agenda}

```elisp
(use-package org-agenda
:after org
:hook
  (org-agenda-mode . solaire-mode)
  (org-agenda-mode . olivetti-mode)
:bind
  ("<f12>" . org-agenda)
:config
  (setq org-agenda-span 'day)
  (setq org-agenda-files '("~/Org"))
  (let ((org-agenda-custom-commands '(
    ("u" "Super view" (
      (agenda "" (
        (org-agenda-span 1)
        (org-super-agenda-groups '(
          (:name "Today"
           :tag ("bday" "ann" "hols" "cal" "today")
           :time-grid t
           :todo ("WIP")
           :deadline today)
          (:name "Perso"
           :tag "perso")
          (:name "Overdue"
           :deadline past)
          (:name "Reschedule"
           :scheduled past)
          (:name "Due Soon"
           :deadline future)
          )))
      )
      (tags (concat "w" (format-time-string "%V"))
        ( (org-agenda-overriding-header
            (concat "ToDos Week " (format-time-string "%V")))
          (org-super-agenda-groups '(
             (:discard (:deadline t))
             (:name "Perso" :tag "perso")
             (:name "Loka"  :tag "loka")
             (:name "Ping"  :tag "ping")
          ))
        )
      )
   )))))
  (org-agenda nil "u"))

)
```


#### Org-super-agenda {#org-super-agenda}

```elisp
(use-package org-super-agenda
:after org-agenda
:config
  (org-super-agenda-mode)
  (setq org-super-agenda-groups '(
    (:name "Today"
     :time-grid t
     :scheduled today)
    (:name "Due today"
     :deadline today)
    (:name "Important"
     :priority "A")
    (:name "Overdue"
     :deadline past)
    (:name "Due soon"
     :deadline future)
    (:name "Waiting"
     :todo "WAIT")
    (:name "Someday"
     :todo "SOMEDAY")
  ))
)
```


#### Calendar {#calendar}

```elisp
(use-package calendar
:hook
  (calendar-mode . olivetti-mode)
  (calendar-mode . solaire-mode)
:config
  (setq calendar-date-style 'iso)
)
```


#### GTD {#gtd}

```elisp
(use-package org-gtd
:after org
:init
  (setq org-gtd-update-ack "3.0.0")
:config
;; where org-gtd will put its files.
(setq org-gtd-directory "~/gtd/")
;; package: https://github.com/Malabarba/org-agenda-property
;; this is so you can see who an item was delegated to in the agenda
(setq org-agenda-property-list '("DELEGATED_TO"))
;; I think this makes the agenda easier to read
(setq org-agenda-property-position 'next-line)
;; package: https://www.nongnu.org/org-edna-el/
;; org-edna is used to make sure that when a project task gets DONE,
;; the next TODO is automatically changed to NEXT.
(setq org-edna-use-inheritance t)
(org-edna-load)
:bind
(("C-c d c" . org-gtd-capture) ;; add item to inbox
 ("C-c d a" . org-agenda-list) ;; see what's on your plate today
 ("C-c d p" . org-gtd-process-inbox) ;; process entire inbox
 ("C-c d n" . org-gtd-show-all-next) ;; see all NEXT items
 ;; see projects that don't have a NEXT item
 ("C-c d s" . org-gtd-show-stuck-projects)
 ;; the keybinding to hit when you're done editing an item in the
 ;; processing phase
 ("C-c d f" . org-gtd-clarify-finalize)))
```


### Preview {#preview}


#### Org 9.7 {#org-9-dot-7}

```elisp
(use-package org
:after emacs
:init
  (setq org-element-cache-persistent nil)
  (setq org-element-use-cache nil)
  (setq org-latex-preview-numbered t)
  (plist-put org-latex-preview-options :zoom 1.25)
  (let ((pos (assoc 'dvisvgm org-latex-preview-process-alist)))
    (plist-put (cdr pos) :image-converter '("dvisvgm --page=1- --optimize --clipjoin --relative --no-fonts --bbox=preview -o %B-%%9p.svg %f")))
:config
  (add-hook 'org-mode-hook #'turn-on-org-cdlatex)
)
```


#### Org-fragtog {#org-fragtog}

[Org-fragtog](https://github.com/io12/org-fragtog) automatically toggle Org mode LaTeX fragment previews as the cursor enters and exits them

Code under :config is taken from <https://emacs-china.org/t/org-mode-latex-mode/22490>

```elisp
(use-package org-fragtog
:defer t
:config
    ;; Vertically align LaTeX preview in org mode
    (defun my-org-latex-preview-advice (beg end &rest _args)
    (let* ((ov (car (overlays-at (/ (+ beg end) 2) t)))
      (img (cdr (overlay-get ov 'display)))
      (new-img (plist-put img :ascent 95)))
  (overlay-put ov 'display (cons 'image new-img))))
    (advice-add 'org--make-preview-overlay
    :after #'my-org-latex-preview-advice)

    ;; from: https://kitchingroup.cheme.cmu.edu/blog/2016/11/06/
    ;; Justifying-LaTeX-preview-fragments-in-org-mode/
    ;; specify the justification you want
    (plist-put org-format-latex-options :justify 'right)

    (defun eli/org-justify-fragment-overlay (beg end image imagetype)
    (let* ((position (plist-get org-format-latex-options :justify))
      (img (create-image image 'svg t))
      (ov (car (overlays-at (/ (+ beg end) 2) t)))
      (width (car (image-display-size (overlay-get ov 'display))))
      offset)
  (cond
  ((and (eq 'center position)
      (= beg (line-beginning-position)))
  (setq offset (floor (- (/ fill-column 2)
        (/ width 2))))
  (if (< offset 0)
      (setq offset 0))
  (overlay-put ov 'before-string (make-string offset ? )))
  ((and (eq 'right position)
      (= beg (line-beginning-position)))
  (setq offset (floor (- fill-column
        width)))
  (if (< offset 0)
      (setq offset 0))
  (overlay-put ov 'before-string (make-string offset ? ))))))
    (advice-add 'org--make-preview-overlay
    :after 'eli/org-justify-fragment-overlay)

    ;; from: https://kitchingroup.cheme.cmu.edu/blog/2016/11/07/
    ;; Better-equation-numbering-in-LaTeX-fragments-in-org-mode/
    (defun org-renumber-environment (orig-func &rest args)
    (let ((results '())
      (counter -1)
      (numberp))
  (setq results (cl-loop for (begin .  env) in
      (org-element-map (org-element-parse-buffer)
    'latex-environment
    (lambda (env)
    (cons
        (org-element-property :begin env)
        (org-element-property :value env))))
      collect
      (cond
    ((and (string-match "\\\\begin{equation}" env)
        (not (string-match "\\\\tag{" env)))
    (cl-incf counter)
    (cons begin counter))
    ((and (string-match "\\\\begin{align}" env)
        (string-match "\\\\notag" env))
    (cl-incf counter)
    (cons begin counter))
    ((string-match "\\\\begin{align}" env)
    (prog2
        (cl-incf counter)
        (cons begin counter)
    (with-temp-buffer
        (insert env)
        (goto-char (point-min))
        ;; \\ is used for a new line. Each one leads
        ;; to a number
        (cl-incf counter (count-matches "\\\\$"))
        ;; unless there are nonumbers.
        (goto-char (point-min))
        (cl-decf counter
          (count-matches "\\nonumber")))))
    (t
    (cons begin nil)))))
  (when (setq numberp (cdr (assoc (point) results)))
  (setf (car args)
    (concat
    (format "\\setcounter{equation}{%s}\n" numberp)
    (car args)))))
    (apply orig-func args))
    (advice-add 'org-create-formula-image :around #'org-renumber-environment)
)
```


#### grip-mode {#grip-mode}

```elisp
(use-package grip-mode
:after org
)
```


### Export {#export}


#### Ox {#ox}

```elisp
(use-package ox
:after org
:defer t
:config
  (setq
    org-export-with-toc t
    org-export-with-tags 'not-in-toc
    org-export-with-drawers nil
    org-export-with-priority t
    org-export-with-footnotes t
    org-export-with-smart-quotes t
    org-export-with-section-numbers t
    org-export-with-sub-superscripts '{}
    org-export-use-babel t
    org-export-headline-levels 9
    org-export-coding-system 'utf-8
    org-export-with-broken-links 'mark
  )
)
```


#### HTML {#html}

```elisp
(use-package ox-html
  :defer t
  :after ox
  :config
  (setq org-html-doctype "html5"
    org-html-html5-fancy t
    org-html-checkbox-type 'unicode
    org-html-validation-link nil))
```

```elisp
(use-package htmlize
  :defer t
  :config
  (setq htmlize-pre-style t
    htmlize-output-type 'inline-css))
```


#### PDF {#pdf}

There're 3 common ways to export org into PDF.

-   ox-html, then export to pdf via browser.
-   ox-latex
-   ox-pandoc

<!--listend-->

```elisp

```


#### LaTeX {#latex}

```elisp
(use-package ox-latex
:defer t
:after ox
)
```


#### Slides {#slides}

Use [BROKEN LINK: revealjs.com] to generate beautiful presentations.
This requires reveal.js installed on your machine.

```bash
git clone https://github.com/hakimel/reveal.js.git
cd reveal.js && npm install
```

```elisp
(use-package ox-reveal
:defer t
:after ox
:config
  (setq org-reveal-hlevel 1
    org-reveal-theme "moon"
    org-reveal-mathjax t
    org-reveal-ignore-speaker-notes t
    org-reveal-title-slide "<h1><font size=\"8\">%t</font></h1><h2><font size=\"6\">%s</font></h2><p><font size=\"5\">%a</font><br/><font size=\"5\">%d</font></p>"
    org-reveal-plugins '(markdown zoom notes search)
    org-reveal-klipsify-src 'on))
```


#### Markdown {#markdown}

Use [ox-gfm](https://github.com/larstvei/ox-gfm) to generate github-style markdown file.

```elisp
(use-package ox-gfm
:defer t
:after ox
)
```


#### MS-Office {#ms-office}

Use ox-pandoc to generate MS-Office files like word and ppt.

```elisp
(use-package ox-pandoc
:defer t
:after ox
:config
  (setq org-pandoc-format-extensions
        '(markdown_github+pipe_tables+raw_html)
  )
)
```


#### Static HTML {#static-html}

```elisp
(use-package ox-publish
:defer t
)
```


#### Hugo {#hugo}

General steps to publish a blog using hugo in emacs includes:

-   create a org mode heading
-   write your blog
-   use ox-hugo to export to a markdown file
    (Because hugo doesn't support org very well)
-   use easy-hugo to preview
-   publish

[ox-hugo](https://github.com/kaushalmodi/ox-hugo) is an Org exporter backend that exports Org to Hugo-compatible Markdown and also generates the front-matter (in TOML or YAML format).

```elisp
(use-package ox-hugo
:after ox
:commands (org-hugo-export-as-md org-hugo-export-to-md)
:init
  (setq org-hugo-base-dir "~/Blog"
        org-hugo-front-matter-format "yaml"
  )
)

```

Easy-hugo

```elisp
(use-package easy-hugo
:defer t
)
```


### Org-transclusion {#org-transclusion}


## Emacs OS {#emacs-os}


### PDF-Tools {#pdf-tools}

PDF-tools is a replacement for DocView for PDF files.
The rendering is performed by poppler.
Run `M-x pdf-tools-install` to install libraries needed.

```elisp
(use-package pdf-tools
;; manually update
;; after each update we have to call:
;; Install pdf-tools but don't ask or raise error (otherwise daemon mode will wait for input)
;; (pdf-tools-install t t t)
:magic ("%PDF" . pdf-view-mode)
:mode (("\\.pdf\\'" . pdf-view-mode))
:hook ((pdf-view-mode . pdf-view-init))
:bind (:map pdf-view-mode-map
       ("C-s" . isearch-forward)
       ("M-w" . pdf-view-kill-ring-save)
       ("M-p" . print-pdf))
:config
(defun pdf-view-init ()
  "Initialize pdf-tools view like enabline TOC functions or use dark theme at night."

  ;; Use dark theme when opening PDFs at night time
  (let ((hour (string-to-number (format-time-string "%H"))))
    (when (or (< hour 5) (< 20 hour))
      (pdf-view-midnight-minor-mode)))

  ;; Enable pdf-tools minor mode to have features like TOC extraction by pressing `o'.
  (pdf-tools-enable-minor-modes)

  ;; Disable while-line-or-region to free keybindings.
  (whole-line-or-region-local-mode -1))

;; Use `gtklp' or `hp-print' to print as it has better cups support
(setq print-pdf-command "hp-print")
(defun print-pdf (&optional pdf)
  "Print PDF using external program defined in `print-pdf-command'."
  (interactive "P")
  (start-process-shell-command
   print-pdf-command nil (concat print-pdf-command " " (shell-quote-argument (or pdf (buffer-file-name))))))

;; more fine-grained zooming; +/- 10% instead of default 25%
(setq pdf-view-resize-factor 1.1)
;; Always use midnight-mode and almost same color as default font.
;; Just slightly brighter background to see the page boarders
(setq pdf-view-midnight-colors '("#c6c6c6" . "#363636")))
```


### Elfeed {#elfeed}

```elisp
(use-package elfeed
:defer t
:config
  (setq elfeed-feeds '(
("http://nullprogram.com/feed/" blog emacs)
"http://www.50ply.com/atom.xml"  ; no autotagging
("http://nedroid.com/feed/" webcomic)
    )
  )
)
```


### Mu4e {#mu4e}

```cfg
[submodule "mu4e"]
  build-step = ./autogen.sh
  build-step = make -C mu4e > /dev/null
  build-step = borg-update-autoloads
  load-path = build/mu4e
```


### Eaf {#eaf}

[EAF](https://github.com/emacs-eaf/emacs-application-framework) is an extensible framework that revolutionizes the graphical capabilities of Emacs.

```elisp
(use-package eaf
:disabled
:custom
  ; See https://github.com/emacs-eaf/emacs-application-framework/wiki/Customization
  (eaf-browser-continue-where-left-off t)
  (eaf-browser-enable-adblocker t)
  (browse-url-browser-function 'eaf-open-browser)
:config
  (defalias 'browse-web #'eaf-open-browser)
  ;; (eaf-bind-key scroll_up "C-n" eaf-pdf-viewer-keybinding)
  ;; (eaf-bind-key scroll_down "C-p" eaf-pdf-viewer-keybinding)
  ;; (eaf-bind-key take_photo "p" eaf-camera-keybinding)
  ;; (eaf-bind-key nil "M-q" eaf-browser-keybinding)
  (setq confirm-kill-processes nil)
) ;; unbind, see more in the Wiki

;(use-package eaf-browser)
;(use-package eaf-pdf-viewer)
```

tweaks to build eaf

```cfg
[submodule "eaf"]
  load-path = .
  load-path = core
  load-path = extension
```


### Games {#games}


### Tetris {#tetris}

```elisp
(add-hook 'tetris-mode-hook
  (defun config/tetris-center ()
    (config/window-center 76)
  )
)
```


### 2048 {#2048}

```elisp
(add-hook '2048-mode-hook
  (defun config/2048-center ()
    (config/window-center 35)
  )
)
```


## End {#end}

```elisp
(progn ;     startup
  (message "Loading %s...done (%fs)" user-init-file
     (float-time (time-subtract (current-time)
              before-user-init-time)))
  (add-hook 'after-init-hook
      (lambda ()
        (message
         "Loading %s...done (%fs) [after-init]" user-init-file
         (float-time (time-subtract (current-time)
            before-user-init-time))))
      t))

(progn ;     personalize
  (let ((file (expand-file-name (concat (user-real-login-name) ".el")
        user-emacs-directory)))
    (when (file-exists-p file)
      (load file))))

;; Local Variables:
;; indent-tabs-mode: nil
;; End:
;;; init.el ends here
```
