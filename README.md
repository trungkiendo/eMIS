/* Đọc bảng dữ liệu từ file csv */
proc import datafile='/path/to/loan_data.csv'
            out=loa_tb
            dbms=csv replace;
run;

/* Tính tổng giá trị loan_amt của bảng dữ liệu */
proc sql;
   select sum(loan_amt) into :total_loan_amt from loa_tb;
quit;

/* Tính số lượng case của bảng dữ liệu */
proc sql;
   select count(*) into :num_cases from loa_tb;
quit;

/* Tạo ra một biến mới cho bảng dữ liệu, biểu thị nhóm mà mỗi case thuộc về */
data loa_tb;
   set loa_tb;
   group = ifn(_n_ <= ceil(&num_cases / 2), 1, 2);
run;

/* Định nghĩa hàm tính tổng sai số của phân chia */
%macro sum_squared_error(data);
   proc means data=&data noprint;
      var loan_amt;
      output out=_tmp sum=total_loan_amt n=n_cases;
   run;

   proc sql noprint;
      select sum((loan_amt - &total_loan_amt / &n_cases) ** 2)
      into :sum_squared_error
      from _tmp;
   quit;

   %put Sum of squared error: &sum_squared_error;

   %let sum_squared_error = &sum_squared_error;
%mend;

/* Định nghĩa hàm tính tổng sai số của phân chia cho một giá trị ngưỡng */
%macro split_data(data, threshold);
   data _tmp;
      set &data;
      group = ifn(loan_amt <= &threshold, 1, 2);
   run;

   %sum_squared_error(_tmp)
%mend;

/* Tìm giá trị ngưỡng tối ưu bằng thuật toán tối ưu hóa */
%let min_threshold = %sysevalf(%sysfunc(min(loa_tb.loan_amt)));
%let max_threshold = %sysevalf(%sysfunc(max(loa_tb.loan_amt)));
%let step = %sysevalf((&max_threshold - &min_threshold) / 100);
%let best_threshold = &min_threshold;
%let best_error = %split_data(loa_tb, &best_threshold);

%do i = %sysevalf(&min_threshold + &step) %to %sysevalf(&max_threshold) %by %sysevalf(&step);
   %let error = %split_data(loa_tb, &i);

   %if &error < &best_error %then %do;
      %let best_error = &error;
      %let best_threshold = &i;
   %end;
%end;

%put Best threshold: &best_threshold;
%put Best sum of squared error: &best_error;

/* Phân chia bảng dữ liệu thành hai tập dữ liệu sử dụng giá trị ngưỡng tối ưu */
data loa_tb_1 loa_tb_2;
   set loa_tb;
   if loan_amt <= &best_threshold then output loa_tb_1;
   else output loa_tb_2;
run;

/* Tính tổng giá trị loan_amt và số lượng case của 2 tập dữ liệu */
proc sql;
   select sum(loan_amt) into :loan_amt_1 from loa_tb_1;
   select sum(loan_amt) into :loan_amt_2 from loa_tb_2;
   select count(*) into :num_cases_1 from loa_tb_1;
   select count(*) into :num_cases_2 from loa_tb_2;
quit;

/* In kết quả */
%put Tổng giá trị loan_amt của toàn bộ bảng dữ liệu: &total_loan_amt. 
%put Số lượng case của toàn bộ bảng dữ liệu: &num_cases;
%put Giá trị ngưỡng tối ưu: &best_threshold;
%put Số lượng case của tập dữ liệu con thứ nhất: &num_cases_1;
%put Tổng giá trị loan_amt của tập dữ liệu con thứ nhất: &loan_amt_1;
%put Số lượng case của tập dữ liệu con thứ hai: &num_cases_2;
%put Tổng giá trị loan_amt của tập dữ liệu con thứ hai: &loan_amt_2;
