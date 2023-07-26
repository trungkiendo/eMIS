data loan1;
set loan;
keep loan_id loan_amt;
rank loan_amt;
where rank <= 0.7 * _N_;
run;

data loan2;
set loan;
keep loan_id loan_amt;
rank loan_amt;
where rank > 0.7 * _N_;
run;
