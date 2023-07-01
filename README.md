/* Thiết lập đường dẫn đến file .egp */
%let egp_file = C:\path\to\your\egp\file.egp;

/* Macro kiểm tra các bảng */
%macro check_table_update(libname=, tablename=);
   /* Kiểm tra ngày tạo của bảng */
   proc sql noprint;
      select crdate into :table_crdate
      from dictionary.tables
      where libname = "&libname." and memname = "&tablename.";
   quit;
   
   /* In ra thông báo nếu bảng đã được cập nhật */
   %if "&table_crdate." = %sysfunc(today(), date9.) %then %do;
      %put NOTE: Table &libname..&tablename. has been updated.;
   %end;
   
   /* In ra thông báo nếu bảng chưa được cập nhật */
   %else %do;
      %put NOTE: Table &libname..&tablename. has not been updated.;
   %end;
%mend;

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

/* Chạy file .egp nếu các bảng đã được cập nhật */
proc sql noprint;
   select count(*) into :table_count
   from dictionary.tables
   where libname = "MYLIB" and crdate = %sysfunc(today(), date9.);
quit;

%if &table_count > 0 %then %do;
   proc stp stprepository="&egp_file";
   run;
%end;
%else %do;
   %put NOTE: No tables have been updated. The .egp file will not be run.;
%end;
