from tkinter import *
from tkinter import ttk

TrvFrm = Tk()
TrvFrm.title('TreeView Row Color')
TrvFrm.geometry('500x350')

style = ttk.Style(TrvFrm)

# ttk Style theme
style.theme_use("default")

# Set treeview Row Colomn Color
style.map('Treeview', background=[('selected', '#BFBFBF')])

MyTrv = ttk.Treeview(TrvFrm, columns=('MyCl1', 'MyCl2', 'MyCl3'), show='headings', selectmode = 'browse')
MyTrv.column("#0", width=60, minwidth=30, stretch=NO)
MyTrv.column("MyCl1", width=80, minwidth=20, stretch=NO)
MyTrv.column("MyCl2", width=100, minwidth=30, stretch=NO)
MyTrv.column("MyCl3", width=100, minwidth=30, stretch=NO)

MyTrv.heading('MyCl1', text='CLID')
MyTrv.heading('MyCl2', text='CLName')
MyTrv.heading('MyCl3', text='CLFName')

# Set treeview row font color or foreground color
MyTrv.tag_configure('Cl1', background = '#f9fbdb', foreground = '#c26981')
MyTrv.tag_configure('Cl2', background = '#fbdbf1', foreground = '#6f9594')

for sn in range(1, 6):
    if (sn % 2) == 0:
        MyTrv.insert('', END, values=('MyIDs' + str(sn), 'MyName' + str(sn), 'FName' + str(sn)), tags = 'Cl1')
    else:
        MyTrv.insert('', END, values=('MyIDs' + str(sn), 'MyName' + str(sn), 'FName' + str(sn)), tags = 'Cl2')

MyTrv.pack(side=TOP, fill="both", expand=True)

TrvFrm.mainloop()
