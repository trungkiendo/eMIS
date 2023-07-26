/* Tạo cột số ngẫu nhiên và tính tổng giá trị loan_amt trong bảng dữ liệu */
data loa_tb;
   set loa_tb;
   rand = ranuni(0);
   sum_loan_amt + loan_amt;
run;

/* Sắp xếp bảng dữ liệu theo giá trị của cột số ngẫu nhiên */
proc sort data=loa_tb;
   by rand;
run;

/* Chọn ngưỡng phân chia các case giữa hai tập dữ liệu */
data _null_;
   set loa_tb;
   if _n_ eq ceil(&num_cases/2) then do;
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
