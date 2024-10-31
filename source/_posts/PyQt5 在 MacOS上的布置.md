---
title: PyQt5 在 MacOS上的布置
date: 2023-03-26 22:21:23
---

系统环境 MacOS

Python版本 Python3.7

1 Pycharm->Preferences->Project:项目名->Python Interpreter->"+"

下载PyQt5和PyQt5-tools

2 配置Qt Designer, PyUIC, Pyrcc.

    1) Qt Designer的配置

　　  Program: /usr/local/Cellar/qt@5/5.15.4_2/libexec/Designer.app

　　  Working directory: $ProjectFileDir$

    2) PyUIC的配置

　　  Program: /usr/local/Cellar/python@3.7/3.7.12_1/bin/python3.7

　　  Arguments: -m PyQt5.uic.pyuic $FileName$ -o $FileNameWithoutExtension$.py

　　  Working directory: $ProjectFileDir$ 

    3) Qt Designer的配置

　　  Program: /usr/local/Cellar/qt@5/5.15.4_2/bin/pyrcc5

 　　 Arguments: $FileName$ -o $FileNameWithoutExtension$_rc.py

　　  Working directory: $ProjectFileDir$

至此，三个工具全部配置完成！