---
layout: post
title: Dark Mode for Zim Desktop Wiki on Windows
date: "2025-01-01"
---

This recipe is a concise adaption of [Zim's documentation on its config files](https://zim-wiki.org/manual/Help/Config_Files.html). Works for me™ when I want access to my Journal soon after any fresh Windows installation. Please do not hesitate to leave a message in the [comments section](https://github.com/MaykGyver/maykgyver.github.io/issues) if you know an easier or faster approach.

1. On a user prompt:
    ```powershell
    winget install Python.Python.3.13
    winget install Zimwiki.Zim
    ```
2. Launch `python` on an elevated prompt and paste these lines
    ```python
    import os
    import pathlib
    settings = pathlib.Path(
        os.environ['PROGRAMFILES'],
        'Zim Desktop Wiki',
        'etc/gtk-3.0/settings.ini',
    )
    settings.parent.mkdir(parents=True,exist_ok=False)
    settings.touch(exist_ok=False)
    with settings.open('w') as settings:
        settings.write('[Settings]\ngtk-application-prefer-dark-theme=1\n')
    ```
