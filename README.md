Exception in Tkinter callback
Traceback (most recent call last):
  File "C:\Python\lib\tkinter\__init__.py", line 1885, in __call__
    return self.func(*args)
  File "D:\Kien\Python\MIS_MONITOR\MIS_MONITOR\main.py", line 11, in show_datamart_screen
    datamart = DatamartScreen(root, login.show_home_screen)
  File "D:\Kien\Python\MIS_MONITOR\MIS_MONITOR\datamart_screen.py", line 37, in __init__
    self.datamart_tree.configure(borderwidth=2, relief="solid")
  File "C:\Python\lib\tkinter\__init__.py", line 1639, in configure
    return self._configure('configure', cnf, kw)
  File "C:\Python\lib\tkinter\__init__.py", line 1629, in _configure
    self.tk.call(_flatten((self._w, cmd)) + self._options(cnf))
_tkinter.TclError: unknown option "-borderwidth"
