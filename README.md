/* Tạo danh sách các thư viện SAS cần xuất */
%let liblist = lib1 lib2 lib3;

/* Tạo bảng tạm thời để lưu thông tin về các tập tin SAS */
data temp;
  length libname memname memtype $32;
  keep libname memname memtype;
run;

/* Vòng lặp chạy PROC CONTENTS cho mỗi thư viện SAS và lưu thông tin vào bảng tạm thời */
%macro save_contents;
  %do i=1 %to %sysfunc(countw(&liblist));
    %let lib=%scan(&liblist,&i);

    proc contents data=&lib._all_ noprint out=contents;
    run;

    proc sql noprint;
      insert into temp
      select distinct "&lib" as libname, memname, memtype
      from contents;
    quit;

  %end;
%mend;

/* Chạy macro để lưu PROC CONTENTS của tất cả các thư viện SAS vào bảng tạm thời */
%save_contents;

/* Hiển thị thông tin trong bảng tạm thời */
proc print data=temp noobs;
run;
