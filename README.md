Exception in Tkinter callback
Traceback (most recent call last):
  File "C:\Python\lib\tkinter\__init__.py", line 1885, in __call__
    return self.func(*args)
  File "D:\Kien\Python\MIS_MONITOR\MIS_MONITOR\main.py", line 11, in show_datamart_screen
    datamart = DatamartScreen(root, login.show_home_screen)
  File "D:\Kien\Python\MIS_MONITOR\MIS_MONITOR\datamart_screen.py", line 148, in __init__
    self.update_datamart_tab()
  File "D:\Kien\Python\MIS_MONITOR\MIS_MONITOR\datamart_screen.py", line 201, in update_datamart_tab
    self.add_datamart(*row, bg_color=background)
TypeError: add_datamart() got an unexpected keyword argument 'bg_color'
