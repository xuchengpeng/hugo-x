---
title: "Color Lighten and Darken"
date: 2025-08-28T13:07:17+08:00
categories: ["Emacs"]
tags: ["Emacs", "Themes"]
---

RGB color lighten and darken in catppuccin-themes.
<!--more-->

```elisp
(defun catppuccin-themes-blend (a b &optional alpha)
  "Blend the two colors A and B in linear space with ALPHA (a float between 0 and 1)."
  (pcase-let ((`(,r ,g ,b) (color-blend (color-name-to-rgb a) (color-name-to-rgb b) alpha)))
    (color-rgb-to-hex r g b 2)))

(defun catppuccin-themes-lighten (color value)
  "Lighten COLOR by VALUE% (0–100)."
  (let* ((alpha (/ value 100.0)))
    (catppuccin-themes-blend color "#ffffff" (- 1 alpha))))

(defun catppuccin-themes-darken (color value)
  "Darken COLOR by VALUE% (0–100)."
  (let* ((alpha (/ value 100.0)))
    (catppuccin-themes-blend color "#000000" (- 1 alpha))))

(catppuccin-themes-darken "#eff1f5" 5)
(catppuccin-themes-lighten "#303446" 5)
```
