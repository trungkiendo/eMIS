/* Tính tổng số loan_amt trong bảng dữ liệu */
proc sql;
   select sum(loan_amt) into :total_loan_amt from loa_tb;
quit;

/* Tính phân phối phần trăm của loan_amt trong bảng dữ liệu */
proc sql;
   create table loa_tb_percent as
   select loan_id, loan_amt, loan_amt/&total_loan_amt as loan_amt_pct
   from loa_tb;
quit;

/* Sắp xếp bảng dữ liệu theo phân phối phần trăm của loan_amt */
proc sort data=loa_tb_percent;
   by loan_amt_pct;
run;

/* Chia bảng dữ liệu thành 2 tập tương đương về phân phối phần trăm của loan_amt */
data loa_tb_set1 loa_tb_set2;
   set loa_tb_percent;

   /* Chia tập dựa trên giá trị của phân phối phần trăm */
   if _n_ le nobs/2 then output loa_tb_set1;
   else output loa_tb_set2;
run;

/* Tính tổng số case và tổng số loan_amt trong 2 tập */
proc sql;
   select count(*) as num_cases_1, sum(loan_amt) as sum_loan_amt_1 from loa_tb_set1;
   select count(*) as num_cases_2, sum(loan_amt) as sum_loan_amt_2 from loa_tb_set2;
quit;
