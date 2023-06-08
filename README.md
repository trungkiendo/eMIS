PROC SQL;
SELECT libname
FROM saslib;
QUIT;

%macro loop_over_libraries;
%let outdir = /path/to/output/folder;
%let dsid = %sysfunc(open(&syslast));
%let memcount = %sysfunc(attrn(&dsid, nobs));
%do i = 1 %to &memcount;
    %let libname = %sysfunc(getvarc(&dsid, 1));
    PROC CONTENTS DATA=&libname.._all_ OUT=&outdir./&libname._all_info ALL LIBRARY=&libname LIST;
    RUN;
%end;
%let rc = %sysfunc(close(&dsid));
%mend loop_over_libraries;

%loop_over_libraries;
