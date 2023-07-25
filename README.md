[proc sort data = loan;
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
](https://performancemanager10.successfactors.com/sf/pmreview?fid=20925&company=svfc&username=ECF001791&reqOrig=todoList&&_s.crb=usAWIqedAOQDxxOnoPfjQNC9HE8eAIBg8z4gHETLK2I%253d


ECF001791


Dtk@200289)https://performancemanager10.successfactors.com/sf/pmreview?fid=20925&company=svfc&username=ECF001791&reqOrig=todoList&&_s.crb=usAWIqedAOQDxxOnoPfjQNC9HE8eAIBg8z4gHETLK2I%253d


ECF001791


Dtk@200289
