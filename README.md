/* Tính tổng số loan_amt trong bảng dữ liệu */
proc sql;
   select sum(loan_amt) into :total_loan_amt from loa_tb;
quit;

/* Tính số case của bảng dữ liệu */
proc sql;
   select count(*) into :num_cases from loa_tb;
quit;

/* Tính số case và tổng giá trị loan_amt của mỗi tập */
%let num_sets = 2;
%let num_cases_per_set = %sysevalf(&num_cases/&num_sets);
%let total_loan_amt_per_set = %sysevalf(&total_loan_amt/&num_sets);

/* Lựa chọn ngẫu nhiên các case cho tập dữ liệu thứ nhất */
proc surveyselect data=loa_tb out=loa_tb_set1
   method=srs n=&num_cases_per_set total=&total_loan_amt_per_set;
run;

/* Tạo tập dữ liệu thứ hai từ các case không được chọn cho tập dữ liệu thứ nhất */
data loa_tb_set2;
   set loa_tb;
   if _n_ notin loa_tb_set1;
run;

/* Tính tổng số case và tổng số loan_amt trong 2 tập */
proc sql;
   select count(*) as num_cases_1, sum(loan_amt) as sum_loan_amt_1 from loa_tb_set1;
   select count(*) as num_cases_2, sum(loan_amt) as sum_loan_amt_2 from loa_tb_set2;
quit;
