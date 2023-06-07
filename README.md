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
