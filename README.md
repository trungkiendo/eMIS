/* Bước 1: Xác định tên các bảng được sử dụng trong file SAS .egp */
/* Thêm tên bảng vào danh sách TABLE_NAMES */
data _null_;
   length code $ 200;
   infile "<đường_dẫn_đến_file_SAS.egp>" truncover;
   input;
   if index(_infile_,'QUERY')>0 then do;
      code = _infile_;
      code = tranwrd(code,'QUERY','PROC SQL noprint select distinct ');
      code = tranwrd(code,'FROM','from ');
      code = tranwrd(code,';',',;');
      call execute(code);
   end;
run;

%put TABLE_NAMES= &TABLE_NAMES; /* in danh sách các bảng được sử dụng */

/* Bước 2: Liệt kê tất cả các bảng trong thư mục làm việc hiện tại */
proc contents data=_all_ out=contents noprint;
run;

/* Bước 3: Lọc và in ra các bảng được sử dụng trong file SAS .egp */
/* Tạo một bảng mới chứa danh sách các bảng được sử dụng */
data myTables;
   set contents;
   where upcase(memname) in (&TABLE_NAMES);
run;

/* In danh sách các bảng được sử dụng */
proc print data=myTables noobs;
   var memname;
run;
