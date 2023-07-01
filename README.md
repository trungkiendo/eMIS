/* Thiết lập đường dẫn đến file .egp */
%let egp_file = C:\path\to\your\egp\file.egp;

/* Kiểm tra các bảng trong file .egp */
%macro check_tables();
   /* Truy xuất các thông tin về các bảng trong file .egp */
   proc metadata in="&egp_file"
                 out=tables
                 metadatalevel=1
                 select=Name Type;
   run;
   
   /* Tạo danh sách các bảng */
   data table_list;
      set tables;
      where Type = 'Table';
      keep Name;
   run;
   
   /* Kiểm tra các bảng */
   data _null_;
      set table_list;
      call execute('%check_table_update(libname=mylib, tablename='||trim(Name)||')');
   run;
%mend;

/* Chạy macro kiểm tra các bảng */
%check_tables;

/* Chạy file .egp */
proc stp stprepository="&egp_file";
run;
