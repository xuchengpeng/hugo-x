---
title: "Org Babel + Mermaid"
date: 2026-10-09T11:21:47+08:00
categories: ["Emacs"]
tags: ["Emacs", "Org", "Mermaid"]
---

Org Babel 内置的语言已支持 PlantUML，在导出文件时可以生成各种图表；同样的，可以为 Org Babel 添加 Mermaid 语言支持。
<!--more-->

1. 安装 [mermaid-cli](https://github.com/mermaid-js/mermaid-cli) :
   ```bash
   npm install -g @mermaid-js/mermaid-cli
   ```
2. 安装 [ob-mermaid](https://github.com/arnm/ob-mermaid) 包，或者直接下载 ob-mermaid.el 文件保存到 User Lisp Directory 。
3. 确保 `mmdc` 在 PATH 路径下，或者指定路径：
   ```emacs-lisp
   (setq ob-mermaid-cli-path "mmdc")
   ```
4. 添加配置文件 `mermaid-config.json` ，并通过 `ob-mermaid-default-config-file` 指定配置文件路径:
   ```json
   {
     "htmlLabels": false,
     "flowchart": { "htmlLabels": false }
   }
   ```
   ```emacs-lisp
   (setq ob-mermaid-default-config-file "/path/to/mermaid-config.json")
   ```
5. 添加 `mermaid` 到 `org-babel-load-languages` ：
   ```emacs-lisp
   (with-eval-after-load 'org
     (org-babel-do-load-languages
      'org-babel-load-languages
      '((mermaid . t))))
   ```
6. 在 Org 文件中添加代码块：
   ```org
   #+begin_src mermaid :file mermaid-test.png
   sequenceDiagram
    A-->B: Works!
   #+end_src
   ```
   {{< figure
     src="./mermaid-test.png"
   >}}

{{< alert "error" >}}
🔴 如果在导出过程中碰到类似 `Error: Could not find chrome-headless-shell` 的错误，可以通过环境变量指定浏览器的路径：

```bash
export PUPPETEER_EXECUTABLE_PATH="/path/to/chrome"
```

`PUPPETEER_EXECUTABLE_PATH` 是 Puppeteer 的一个环境变量，用于指定浏览器（如 Chrome 或 Chromium）的自定义二进制文件路径。
{{< /alert >}}
