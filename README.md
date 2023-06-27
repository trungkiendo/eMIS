data macro_table;
   length STT 8 macro_name $32 macro_code $1000; /* Độ dài các trường trong bảng */
   input STT macro_name $ macro_code $; /* Nhập dữ liệu cho bảng */
   datalines;
1 my_macro %macro my_macro; /* Đoạn mã xử lý của hàm macro */ /* Trả về bảng có tên là mytable */ proc sql; create table mytable as select * from sashelp.class; quit; %mend;
2 my_macro2 %macro my_macro2; /* Đoạn mã xử lý của hàm macro */ /* Trả về bảng có tên là mytable2 */ proc sql; create table mytable2 as select name, sex, age from sashelp.class; quit; %mend;
;
run;
