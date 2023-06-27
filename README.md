/* Tạo bảng macro_table */
data macro_table;
   length STT 8 macro_name $32 macro_code $1000;
   input STT macro_name $ macro_code $;
   datalines;
1 my_macro "%macro my_macro; /* Đoạn mã xử lý của hàm macro */ /* Trả về bảng có tên là mytable */ proc sql; create table mytable as select * from sashelp.class; quit; %mend my_macro;";
2 my_macro2 "%macro my_macro2; /* Đoạn mã xử lý của hàm macro */ /* Trả về bảng có tên là mytable2 */ proc sql; create table mytable2 as select name, sex, age from sashelp.class; quit; %mend my_macro2;";
;
run;

/* Đọc code macro từ bảng */
%let macro_code = %sysfunc(quote(%qsysfunc(getvarc(macro_table, %sysfunc(varnum(macro_table, macro_code))))));
%put &macro_code.;

/* Sử dụng code macro */
%let macro_code_resolved = %sysfunc(resolve(&macro_code.));
%put &macro_code_resolved.;
%macro_code_resolved.;
