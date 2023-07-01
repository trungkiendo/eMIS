   /* Mở file .egp */
   proc catalog cat="&egp_file" memtype="project" out=myproject;
   run;

   /* Truy xuất danh sách các đối tượng trong file .egp */
   proc catalog cat=myproject out=objects;
   run;
   
   /* Tạo danh sách các bảng */
   data table_list;
      set objects;
      where type = 'Table' and libref = 'WORK';
      keep name;
   run;
   
