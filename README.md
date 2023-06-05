/* Kết nối tới database SQLite */
libname sqlite oledb init_string="Provider=SQLiteOLEDB.1;Data Source=C:\path\to\sqlite.db";

/* Tạo một table mới trong SQLite */
proc sql;
    create table sqlite.mytable (
        column1 char(10),
        column2 num
    );
quit;

/* Insert dữ liệu từ một table trong SAS vào trong table mới trong SQLite */
data sqlite.mytable;
    set sas.mytable;
run;

/* Đóng kết nối tới database SQLite */
libname sqlite clear;
