/* Đọc bảng dữ liệu từ file csv */
proc import datafile='/path/to/loan_data.csv'
            out=loa_tb
            dbms=csv replace;
run;

/* Sắp xếp bảng dữ liệu theo giá trị của loan_amt */
proc sort data=loa_tb;
   by loan_amt;
run;

/* Tính tổng giá trị loan_amt của bảng dữ liệu */
proc sql;
   select sum(loan_amt) into :total_loan_amt from loa_tb;
quit;

/* Tính số lượng case của bảng dữ liệu */
proc sql;
   select count(*) into :num_cases from loa_tb;
quit;

/* Tạo ra 2 tập dữ liệu con sử dụng kỹ thuật sampling */
data loa_tb_1 loa_tb_2;
   /* Chọn ngẫu nhiên một số case cho tập 1 */
   call streaminit(1234); /* Khởi tạo seed cho random number generator */
   do i = 1 to ceil(&num_cases / 2);
      set loa_tb nobs=num_cases;
      if rand("UNIFORM") < 0.5 then output loa_tb_1;
   end;

   /* Chọn các case còn lại cho tập 2 */
   set loa_tb;
   if _n_ <= &num_cases / 2 then delete;
   output loa_tb_2;
run;

/* Tính tổng giá trị loan_amt và số lượng case của 2 tập dữ liệu */
proc sql;
   select sum(loan_amt) into :loan_amt_1 from loa_tb_1;
   select sum(loan_amt) into :loan_amt_2 from loa_tb_2;
   select count(*) into :num_cases_1 from loa_tb_1;
   select count(*) into :num_cases_2 from loa_tb_2;
quit;

/* In kết quả */
%put Tổng giá trị loan_amt của toàn bộ bảng dữ liệu: &total_loan_amt;
%put Tổng giá trị loan_amt của tập 1: &loan_amt_1;
%put Tổng giá trị loan_amt của tập 2: &loan_amt_2;
%put Số lượng case của tập 1: &num_cases_1;
%put Số lượng case của tập 2: &num_cases_2;
