/* Tính tổng số loan_amt trong bảng dữ liệu */
proc sql;
   select sum(loan_amt) into :total_loan_amt from loa_tb;
quit;

/* Sắp xếp bảng dữ liệu theo giá trị của loan_amt */
proc sort data=loa_tb;
   by loan_amt;
run;

/* Chia bảng dữ liệu thành 2 tập tương đương về giá trị của loan_amt */
data loa_tb_set1 loa_tb_set2;
   set loa_tb;

   /* Lấy các case từ trên xuống cho tới khi tổng giá trị loan_amt vượt quá nửa tổng giá trị */
   if sum_loan_amt le &total_loan_amt/2 then do;
      output loa_tb_set1;
      sum_loan_amt + loan_amt;
   end;
   else output loa_tb_set2;
run;

/* Tính tổng số case và tổng số loan_amt trong 2 tập */
proc sql;
   select count(*) as num_cases_1, sum(loan_amt) as sum_loan_amt_1 from loa_tb_set1;
   select count(*) as num_cases_2, sum(loan_amt) as sum_loan_amt_2 from loa_tb_set2;
quit;
