def update_datamart_tab(self):
    # Clear existing rows in datamart_tree
    for row in self.datamart_tree.get_children():
        self.datamart_tree.delete(row)

    # Select data from the "dm_datamart" table in the database
    cursor = self.conn.execute(
        "SELECT Tb_No, Datamart, 1, Old_Runtime, Start_Runtime, End_Runtime, Rownum, Status, Desc FROM dm_datamart")

    # Add rows to datamart_tree
    for row in cursor.fetchall():
        # Add the row to the treeview
        item = self.add_datamart(*row)

        # Determine the background color based on the value in the "Desc" column
        if row[8] == 'A':
            tag_name = 'tag_blue'
            bg_color = 'blue'
        elif row[8] == 'B':
            tag_name = 'tag_red'
            bg_color = 'red'
        elif row[8] == 'C':
            tag_name = 'tag_green'
            bg_color = 'green'
        else:
            tag_name = ''
            bg_color = 'white'

        # Tag the item with the corresponding tag and configure the tag to set the background color
        self.datamart_tree.tag_configure(tag_name, background=bg_color)
        self.datamart_tree.item(item, tags=(tag_name,))
