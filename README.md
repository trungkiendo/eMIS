/* Gộp tên các cột lại thành một mảng và sắp xếp mảng đó */
PROC SQL;
  SELECT name
  INTO :col_array separated by ' '
  FROM dictionary.columns
  WHERE libname = 'yourlib' AND memname = 'yourtable'
  ORDER BY name;
QUIT;

/* Tạo bảng mới theo mảng đã sắp xếp */
PROC SORT DATA=yourtable OUT=newtable;
  BY &col_array; /* Sắp xếp bảng dữ liệu theo mảng đã sắp xếp */
RUN;

PROC SQL;
  CREATE TABLE newtable AS
  SELECT &col_array /* Chọn toàn bộ các cột trong mảng */
  FROM newtable;
QUIT;
