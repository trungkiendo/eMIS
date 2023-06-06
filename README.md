def update_datamart_tab(self):
    # Clear existing rows in datamart_tree
    for row in self.datamart_tree.get_children():
        self.datamart_tree.delete(row)

    # Select data from the "dm_datamart" table in the database
    cursor = self.conn.execute(
        "SELECT Tb_No, Datamart, 1, Old_Runtime, Start_Runtime, End_Runtime, Rownum, Status, Desc FROM dm_datamart")

    # Add rows to datamart_tree and assign tags with background colors based on Desc value
    for row in cursor.fetchall():
        self.add_datamart(*row)
        desc_value = row[8]
        if desc_value == 'A':
            tag = 'A'
            bg_color = 'lightgreen'
        elif desc_value == 'B':
            tag = 'B'
            bg_color = 'lightblue'
        elif desc_value == 'C':
            tag = 'C'
            bg_color = 'lightyellow'
        else:
            tag = 'D'
            bg_color = 'white'
        self.datamart_tree.item(self.datamart_tree.get_children()[-1], tags=(tag,))
        self.datamart_tree.tag_configure(tag, background=bg_color)
