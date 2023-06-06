import logging
import threading
import time
from datetime import datetime
import subprocess
import os, os.path
import win32com.client
import pandas as pd
import xlsxwriter
from pathlib import Path
from colorama import init; init()
from colorama import Fore, Back, Style
logError = '\n[LOG]' + Fore.RED + Style.BRIGHT + ' [ERROR] ' + Style.RESET_ALL
now = datetime.now() # current date and time
time_rp=now.strftime("%H")+'h-'+str(int((int(now.strftime("%M"))/30)+1))
dir_path = "D:\Kien\Datamart"
xlsx_name = 'V:/LOG DATAMART/'+'LOG_SCHEDULER_DATAMART ('+ time_rp +').xlsx'
output_name = os.path.join(xlsx_name)

def runMacro(filename):
    projectPath = os.path.join(dir_path, 'File_macro', filename)
    print(projectPath)
    if os.path.exists(projectPath):
        xl = win32com.client.Dispatch("Excel.Application")
        xl.Workbooks.Open(os.path.abspath(projectPath))
        xl.Application.Run("excelsheet.xlsm!modulename.macroname")
        ##    xl.Application.Save() # if you want to save then uncomment this line and change delete the ",
        ##    ReadOnly=1" part from the open function.
        xl.Application.Quit()  # Comment this out if your excel script closes
        print('\n[LOG] SAS Macro Complete ')
        del xl
def set_color(dataframe,writer,sheet_name,column,param):
    worksheet = writer.sheets[sheet_name]
    (max_row, max_col) = dataframe.shape
    # print(str(max_row)+"-"+str(max_col))
    format1 = workbook.add_format({"bg_color": "#669731"})
    format2 = workbook.add_format({"bg_color": "#FFFA22"})
    format3 = workbook.add_format({"bg_color": "#ff4d4d"})
    format4 = workbook.add_format({"bg_color": "#BDB76B"})
    format5 = workbook.add_format({"bg_color": "#BD8E6B"})
    # df.ix[] with df.iloc[] or df.loc[]
    for i in range(0, len(dataframe)):
        # print("Length-"+str(len(dataframe)))
        if dataframe[column].iloc[i] == param[0]:
            worksheet.write(i + 1, max_col-1, dataframe[column].iloc[i], format1)
        elif dataframe[column].iloc[i] == param[1]:
            worksheet.write(i + 1, max_col-1, dataframe[column].iloc[i], format2)
        elif dataframe[column].iloc[i] == param[2]:
            worksheet.write(i + 1, max_col-1, dataframe[column].iloc[i], format3)
        elif dataframe[column].iloc[i] == param[3]:
            worksheet.write(i + 1, max_col-1, dataframe[column].iloc[i], format4)
        elif dataframe[column].iloc[i] == param[4]:
            worksheet.write(i + 1, max_col-1, dataframe[column].iloc[i], format5)
    print('Sheet_color' + sheet_name + ' successfully!')
def export_to_xlsx(dataframe,writer,sheet_name):
    worksheet = writer.sheets[sheet_name]
    # Get the dimensions of the dataframe.
    (max_row, max_col) = dataframe.shape
    # Create a list of column headers, to use in add_table().
    column_settings = [{'header': column} for column in dataframe.columns]
    # Add the Excel table structure. Pandas will add the data.
    worksheet.add_table(0, 0, max_row, max_col - 1, {'columns': column_settings})
    # Make the columns wider for clarity.
    for idx, col in enumerate(dataframe):  # loop through all columns
        series = dataframe[col]
        max_len = max((
            series.astype(str).map(len).max(),  # len of largest item
            len(str(series.name))  # len of column name/header
            )) + 1  # adding a little extra space
        worksheet.set_column(idx, idx, max_len)  # set column width
    print(sheet_name + ' successfully!')

def runSAS(dir,filename):
    projectPath = os.path.join(dir_path,dir, filename)
    print(projectPath)
    filevbs = os.path.join(dir_path,"Code_SAS\Config", "run.vbs")
    fileRun = ' '.join(['cscript', filevbs, projectPath])
    if Path(projectPath).is_file():
        print('\n--------------------------------------------------------------------------')
        print('Launching SAS Project From:' + filename + '\n')
        print('--------------------------------------------------------------------------')
        # Call SAS and Run Progra
        subprocess.check_call(fileRun)
        print('--------------------------------------------------------------------------')
        print('[LOG] SAS Project Complete   '+ filename + '\n')
        print("Complete " + "%s: %s" % (filename, time.ctime(time.time())))
    else:
        print(logError + 'PROJECT FILE NOT FOUND')

def runBatch(filename):
    path = os.path.join(dir_path,'File_batch', filename)
    print(path)
    if os.path.exists(path):
        global done1, res
        res = os.system(path)
    else:
        print(logError + 'PROJECT FILE NOT FOUND')

def thread_func(name):
    logging.info("Thread %s: starting...", name)
    runSAS("Code_SAS\Datamart",name)
    # time.sleep(100)
    logging.info("Thread %s: finishing!", name)


if __name__ == "__main__":
    format = "%(asctime)s: %(message)s"
    logging.basicConfig(format=format,level=logging.INFO,datefmt="%H:%M:%S")
    # Run SAS Config create Scheduler
    runSAS("Code_SAS\Config","Data_source_new.egp")
    # Run macro create excel
    # runBatch("Run_macro.bat")
    # Raad Excell
    schedule = pd.ExcelFile(os.path.join(dir_path, 'Input', "MIS_SCHEDULER_DATAMART.xlsx"))
    datasource = pd.ExcelFile(os.path.join(dir_path, 'Input', "MIS_LOG_DATASOURCE_DAILY.xlsx"))
    datamart = pd.ExcelFile(os.path.join(dir_path, 'Input', "MIS_LOG_DATAMART.xlsx"))
    # Remove a file MIS_SCHEDULER_DATAMART.xlsx
    os.remove(os.path.join(dir_path, 'Input', "MIS_SCHEDULER_DATAMART.xlsx"))
    # Data frame
    df = pd.read_excel(schedule, 'MIS_SCHEDULER_DATAMART',dtype={'Code':str,'Run_time':datetime,'Time_Finish':datetime})
    df1 = pd.read_excel(datasource, 'MIS_LOG_DATASOURCE_DAILY',dtype={'Code':str,'Code':str,'crdate':datetime})
    df2 = pd.read_excel(datamart, 'MIS_LOG_DATAMART', dtype={'Tb_No': str})
    writer = pd.ExcelWriter(output_name, engine='xlsxwriter', datetime_format='dd/mm/yyyy|hh:mm:ss')
    # writer = xlsxwriter.Workbook('write_data.xlsx')
    df.to_excel(writer, sheet_name='scheduler', startrow=1, header=False, index=False)
    export_to_xlsx(df, writer, 'scheduler')
    df1.to_excel(writer, sheet_name='datasource', startrow=1, header=False, index=False)
    export_to_xlsx(df1, writer, 'datasource')
    df2.to_excel(writer, sheet_name='datamart', startrow=1, header=False, index=False)
    export_to_xlsx(df2, writer, 'datamart')
    workbook = writer.book
    red = workbook.add_format({'bg_color': 'red'})
    green = workbook.add_format({'bg_color': 'green'})
    ws1 = writer.sheets['scheduler']
    ws2 = writer.sheets['datasource']
    ws3 = writer.sheets['datamart']
    ws1.conditional_format('H1:I30', {'type': '3_color_scale'})
    ws1.conditional_format('D1:D30', {'type': '3_color_scale'})
    ws2.conditional_format('F1:F100', {'type': '3_color_scale'})
    ws2.conditional_format('G1:G100', {'type': '3_color_scale'})
    ws2.conditional_format('H1:H100', {'type': '3_color_scale'})
    ws3.conditional_format('E1:G100', {'type': '3_color_scale'})
    ws3.conditional_format('J1:J100', {'type': '3_color_scale'})
    # Apply conditional formats to the cell range.
    set_color(df, writer, 'scheduler', 'Desc', param=["Finish", "Running", "Error", "Close","Other"])
    set_color(df1, writer, 'datasource', 'Desc', param=["Ready", "NoDaily", "Error", "Updated","K"])
    set_color(df2, writer, 'datamart', 'Desc', param=["Finish", "Running", "Error", "Unfinish","NoRun"])
    writer.save()
    # df1.to_excel(writer, sheet_name="scheduler2", index=False)
    # df.loc[(df['Status'] =='A' & df['RunYN'] =='Y')]bb
    # & (df['Desc'].isin(['Waiting']))
    list_first = df.loc[(df['Code'] .isin(['10']))&(df['Desc'] .isin(['Waiting'])),'Code_SAS'].values.tolist()
    # list_name = df['Code_SAS'].drop_duplicates().values
    first_threads = list()
    print(list_first)
    if len(list_first) == 0:
        print("list_first was finished")
    else:
        for index in list_first:
            # logging.info("Main first   : create and start thread %s...", index)
            t1 = threading.Thread(target=thread_func, args=(index,))
            first_threads.append(t1)
            t1.start()
            # logging.info("Main first   : thread %d done...", index)
    # notin = [10,12]
    print('\n--------------------------------------------------------------------------')
    print(df.loc[(~df['Code'].isin([10])) & (df['Desc'].isin(['Waiting'])), ['Code', 'Datamart', 'Groups']])
    print('--------------------------------------------------------------------------\n')
    # list_in = [10,5,9,20,21,21,4,14,16,17,7,8,2] # 1,15,21,14,17,6,7
    # list_name = df.loc[(~df['Code'] .isin(not_in))&(df['Desc'] .isin(['Waiting'])),'Code_SAS'].values.tolist()
    list_name = df.loc[(~df['Code'].isin([10])) &(df['Desc'].isin(['Waiting'])),'Code_SAS'].values.tolist()
    # list_name = df['Code_SAS'].drop_duplicates().values
    multiple_threads = list()
    if len(list_name) == 0:
        print("Datamart Finished!")
    else:
        for index in list_name:
            # logging.info("Main    : create and start thread %s...",index)
            t = threading.Thread(target=thread_func,args=(index,))
            multiple_threads.append(t)
            t.start()
        print("Datamart is not Finished yet!")
    print("The end!")
