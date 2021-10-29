# eMIS

import os, os.path
import win32com.client

if os.path.exists("excelsheet.xlsm"):
    xl=win32com.client.Dispatch("Excel.Application")
    xl.Workbooks.Open(os.path.abspath("excelsheet.xlsm"), ReadOnly=1)
    xl.Application.Run("excelsheet.xlsm!modulename.macroname")
##    xl.Application.Save() # if you want to save then uncomment this line and change delete the ", ReadOnly=1" part from the open function.
    xl.Application.Quit() # Comment this out if your excel script closes
    del xl
