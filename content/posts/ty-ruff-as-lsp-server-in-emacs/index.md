---
title: "在 Emacs 中使用 Ty+Ruff 作为 LSP 服务端"
date: 2026-08-29T17:00:20+08:00
categories: ["Emacs"]
tags: ["Emacs", "Python", "Eglot", "LSP", "Ty", "Ruff"]
---

Ty 和 Ruff 都是使用 Rust 语言开发的高性能 Python 开发工具，它们分工不同，可以完美配合，提升开发体验。
<!--more-->

之前的文章介绍了 [Python Development in Emacs](../python-development-in-emacs/) ，其中是使用 Pyright 作为 LSP 服务端，如果使用 Ty+Ruff 作为服务端，则项目配置需要做一些调整。

在 `pyproject.toml` 中增加配置：

```toml
[tool.ty.environment]
python = ".venv"

[tool.ruff]
line-length = 88
```

在 `.dir-locals.el` 中增加配置：

```emacs-lisp
((python-base-mode . ((python-indent-offset . 4)
                      (python-indent-guess-indent-offset-verbose . nil)
                      (python-shell-interpreter . "python")
                      (python-shell-virtualenv-root . "/project/.venv/")
                      (eglot-server-programs . (((python-mode python-ts-mode) . ("rass" "python"))))
                      (apheleia-formatter . (ruff)))))
```
