/* Tính số case và tổng giá trị loan_amt của bảng dữ liệu */
proc sql;
   select count(*) into :num_cases from loa_tb;
   select sum(loan_amt) into :total_loan_amt from loa_tb;
quit;

/* Tính phân vị thứ nhất và thứ hai của giá trị loan_amt */
proc univariate data=loa_tb noprint;
   var loan_amt;
   output out=loan_amt_pctl pctlpre=loan_amt_ pctlpts=25 50 75;
run;

/* Chọn ngưỡng phân chia các case giữa hai tập dữ liệu */
data _null_;
   set loan_amt_pctl;
   if loan_amt_50 then do;
      call symputx('threshold', loan_amt);
      stop;
   end;
run;

/* Chia bảng dữ liệu thành 2 tập tương đương về giá trị của loan_amt */
data loa_tb_set1 loa_tb_set2;
   set loa_tb;
   if loan_amt le &threshold then output loa_tb_set1;
   else output loa_tb_set2;
run;

/* Tính tổng số case và tổng số loan_amt trong 2 tập */
proc sql;
   select count(*) as num_cases_1, sum(loan_amt) as sum_loan_amt_1 from loa_tb_set1;
   select count(*) as num_cases_2, sum(loan_amt) as sum_loan_amt_2 from loa_tb_set2;
quit;
