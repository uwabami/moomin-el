# moomin.el

Edit MoinMoin with Emacs

Original Version: [tototshi/moomin-el](https://github.com/tototoshi/moomin-el).

Modified: 
* Satoru Kurashiki [lurdan/moomin-el](https://github.com/lurdan/moomin-el), add [counsel](https://github.com/abo-abo/swiper) support. 
* Youhei SASAKI(me), Add ido support

## Install

Evaluate this script.

```lisp
(defun moomin-install-dependencies-from-elpa ()
  (require 'package)
  (add-to-list 'package-archives '("melpa" . "http://melpa.milkbox.net/packages/") t)
  (package-initialize)
  (unless (package-installed-p 'request)
    (package-refresh-contents) (package-install 'request)))

(defun moomin-install-dependencies-with-el-get ()
  (unless (require 'el-get nil t)
    (with-current-buffer
        (url-retrieve-synchronously "https://raw.github.com/dimitri/el-get/master/el-get-install.el")
      (goto-char (point-max))
      (eval-print-last-sexp)))
  (require 'el-get)
  (let ((el-get-sources
           (:name moinmoin-mode :type github :pkgname "uwabami/moinmoin-mode")
           (:name moomin :type github :pkgname "uwabami/moomin-el"))))
    (dolist (p el-get-sources)
      (let ((package-name (plist-get p :name)))
        (unless (el-get-package-installed-p package-name)
          (el-get-install package-name))))))

(defun moomin-install-dependencies ()
  (moomin-install-dependencies-from-elpa)
  (moomin-install-dependencies-with-el-get))

(moomin-install-dependencies)
```

## Usage

```lisp
(require 'moomin)

(setq moomin-wiki-url-base "http://your.moinmoin/wiki")

(setq moomin-user "user")
(setq moomin-password "password")

;; When your moin wiki requires basic authentication
(setq moomin-basic-auth-user "user")
(setq moomin-basic-auth-password "password")

;; Assign keybind to 'helm-moomin and 'moomin-save-current-buffer as you like
;; (global-set-key (kbd "C-x w") 'helm-moomin)
;; (global-set-key (kbd "C-x w") 'counsel-moomin)
;; (global-set-key (kbd "C-x w") 'ido-moomin)
(add-hook 'moinmoin-mode-hook
          (lambda ()
            (define-key moinmoin-mode-map (kbd "C-c C-c") 'moomin-save-current-buffer)
            (define-key moinmoin-mode-map (kbd "C-c C-o") 'moomin-browse-current-page)
            (define-key moinmoin-mode-map (kbd "C-c C-r") 'moomin-reload-current-page)))
```
