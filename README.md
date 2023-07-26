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

/* Tạo một biến trung gian case_weight để tính toán trọng số cho từng case */
data loa_tb;
   set loa_tb;
   case_weight = loan_amt / &total_loan_amt * &num_cases;
run;

/* Sắp xếp bảng dữ liệu theo thứ tự giảm dần của case_weight */
proc sort data=loa_tb;
   by descending case_weight;
run;

/* Tạo một biến mới cho bảng dữ liệu, biểu thị nhóm mà mỗi case thuộc về */
data loa_tb;
   set loa_tb;
   group = mod(_n_, 2) + 1;
run;

/* Tính tổng số lượng case và tổng giá trị loan_amt của mỗi nhóm */
proc sql;
   create table loa_tb_summary as
   select group, count(*) as num_cases, sum(loan_amt) as total_loan_amt
   from loa_tb
   group by group;
quit;

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
