    def update_datamart_tab(self):
        # Clear existing rows in datamart_tree
        for row in self.datamart_tree.get_children():
            self.datamart_tree.delete(row)

        # Select data from the "dm_datamart" table in the database
        cursor = self.conn.execute(
            "SELECT Tb_No, Datamart, Table_Name , Old_Runtime, Start_Runtime, End_Runtime, Rownum, Status, Desc FROM dm_datamart")

        # Add rows to datamart_tree and assign tags with background colors based on Desc value
        for row in cursor.fetchall():
            item = self.add_datamart(*row)
            # desc_value = row[8]
            print(row[8])
            if row[8] == 'Finish':
                tag_name = 'tag_blue'
                bg_color = 'blue'
            elif row[8] == 'Unfinish':
                tag_name = 'tag_red'
                bg_color = 'red'
            elif row[8] == 'Error':
                tag_name = 'tag_green'
                bg_color = 'green'
            else:
                tag_name = ''
                bg_color = 'white'
            self.datamart_tree.tag_configure(tag_name, background=bg_color)
            if item is not None and tag_name:
                self.datamart_tree.item(item, tags=(tag_name,))
