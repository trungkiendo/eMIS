%macro check_tables(egp_file);
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
   
   /* Kiểm tra các bảng và chạy file .egp nếu các bảng đã được cập nhật */
   data _null_;
      set table_list;
      call execute('%check_table_update(libname=mylib, tablename='||trim(Name)||')');
      %if &syserr. = 0 %then %do;
         rc = system('"%sasexe%" -sysin "&egp_file"');
      %end;
   run;
%mend;

/* Chạy macro */
%check_tables('C:\path\to\your\egp\file.egp');
