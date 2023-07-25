proc sort data = loan;
by loan_amt;
run;

data set1;
set loan;
where mod(_n_, 5000) = 1;
run;

data set2;
set loan;
where mod(_n_, 5000) = 0;
run;

proc print data = set1;
var loan_amt;
run;

proc print data = set2;
var loan_amt;
run;
