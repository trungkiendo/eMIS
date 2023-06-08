%macro loop_over_libraries;
%let liblist = mylib1 mylib2 mylib3;
%let outdir = /path/to/output/folder;
%let dsid = %sysfunc(open(&syslast));
%let memcount = %sysfunc(attrn(&dsid, nobs));
%do i = 1 %to &memcount;
    %let libname = %sysfunc(getvarc(&dsid, 1));
    %let memname = %sysfunc(getvarc(&dsid, 2));
    PROC CONTENTS DATA=&libname..&memname OUT=&outdir./&libname._&memname._info ALL LIBRARY=&libname LIST;
    RUN;
%end;
%let rc = %sysfunc(close(&dsid));
%mend loop_over_libraries;

%loop_over_libraries;
