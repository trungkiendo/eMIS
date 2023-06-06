   def update_datamart_tab(self):
        # Clear existing rows in datamart_tree
        for row in self.datamart_tree.get_children():
            self.datamart_tree.delete(row)

        # Select data from the "dm_datamart" table in the database
        cursor = self.conn.execute(
            "SELECT Tb_No, Datamart, 'Data table', Old_Runtime, Start_Runtime, End_Runtime, Rownum, Status, Desc FROM dm_datamart")
        r_set = self.conn.execute('''SELECT Desc from dm_datamart''')
        self.datamart_tree.tag_configure('A', background='lightgreen')
        self.datamart_tree.tag_configure('B', background='lightblue')
        self.datamart_tree.tag_configure('C', background='lightyellow')
        self.datamart_tree.tag_configure('D', background='white')
        for dt in r_set:
            if (dt[0] >= 'Finish'):
                my_tag = 'A'
            elif (dt[0] >= 70):
                my_tag = 'Unfinish'
            elif (dt[0] >= 60):
                my_tag = 'C'
            else:
                my_tag = 'D'
            self.datamart_tree.insert("", 'end', iid=dt[0], text=dt[0],
                       values=(dt[0], dt[1], dt[2], dt[3], dt[4]), tags=(my_tag))
        # Add rows to datamart_tree
        for row in cursor.fetchall():
            self.add_datamart(*row)
