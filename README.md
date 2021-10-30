import tkinter.messagebox

def run_macro():
    print('macro')

    #this if is here because if an executable is created, __file__ doesn't work
    if getattr(sys, 'frozen', False):
        name = (os.path.dirname(sys.executable) + '\Forecast template.xlsm')

    else:
        name = str(os.path.dirname(os.path.realpath(__file__)) + '\Forecast template.xlsm')

    print(name)

    #this part runs the macro from excel
    if os.path.exists(name):
        xl=win32com.client.Dispatch("Excel.Application")
        xl.Workbooks.Open(Filename=name, ReadOnly=1)
        xl.Application.Run("ThisWorkbook.LoopFilesInFolder")
        xl.Application.Quit() # Comment this out if your excel script closes
        del xl

    print('File refreshed!')
