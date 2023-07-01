%macro check_table_update(libname=, tablename=);

/* Tạo macro variable chứa ngày hiện tại */
%let today = %sysfunc(today());

/* Tạo macro variable chứa ngày tạo của bảng */
proc sql noprint;
   select crdate into :table_crdate
   from dictionary.tables
   where libname = upcase("&libname.")
     and memname = upcase("&tablename.");
quit;

/* Kiểm tra ngày tạo của bảng và xác định xem bảng đã được cập nhật hay chưa */
%if "&table_crdate." = "&today." %then %do;
   %put Table &libname..&tablename. has been updated.;
%end;
%else %do;
   %put Table &libname..&tablename. has not been updated.;
%end;

%mend check_table_update;
