/* Đọc bảng dữ liệu từ file csv */
proc import datafile='/path/to/loan_data.csv'
            out=loa_tb
            dbms=csv replace;
run;

/* Tính tổng giá trị của biến loan_amt để tính toán tỷ lệ phân chia */
proc sql;
   select sum(loan_amt) into :total_loan_amt from loa_tb;
quit;

/* Tính số lượng case trong bảng dữ liệu */
proc sql;
   select count(*) into :num_cases from loa_tb;
quit;

/* Tạo một biến mới cho bảng dữ liệu, biểu thị nhóm mà mỗi case thuộc về */
data loa_tb;
   set loa_tb;
   group = ifn(cumloanamt le &total_loan_amt/2, 1, 2);
   cumloanamt + loan_amt;
run;

/* Khởi tạo biến cumloanamt */
data loa_tb;
   set loa_tb;
   retain cumloanamt 0;
run;

/* Chọn các case cho tập dữ liệu con thứ nhất */
data loa_tb_1;
   set loa_tb;
   if group = 1 then output;
run;

/* Chọn các case cho tập dữ liệu con thứ hai */
data loa_tb_2;
   set loa_tb;
   if group = 2 then output;
run;
