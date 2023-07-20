# Xuất kết quả vào file Excel
with pd.ExcelWriter('output.xlsx') as writer:
    from_table_df.to_excel(writer, sheet_name='FROM or JOIN')
    create_output_table_df.drop_duplicates().to_excel(writer, sheet_name='CREATE or OUTPUT')
