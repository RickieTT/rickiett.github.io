---
title: PyQt5与Qt冲突
date: 2022-04-26 15:24:23
---

　　在使用QT时 如果在用户变量上声明了 QT_QPA_PLATFORM_PLUGIN_PATH 变量名 并且赋值时，可能会出现QT无法正确使用的情况。

　　解决方法：删除变量即可。

　　BTW，用户变量的变量名 为 QT_QPA_PLATFORM_PLUGIN_PATH， 变量值C:\Users\Username\AppData\Roaming\Python\Python36\site-packages\PyQt5\Qt\plugins（即路径）

　　使用PyQt时，重新添加即可。
