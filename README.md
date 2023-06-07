def update_datamart_tab(self):
    # Clear existing rows in datamart_tree
    for row in self.datamart_tree.get_children():
        self.datamart_tree.delete(row)

    # Select data from the "dm_datamart" table in the database
    cursor = self.conn.execute(
        "SELECT Tb_No, Datamart, Table_Name, Old_Runtime, Start_Runtime, End_Runtime, Rownum, Status, Desc FROM dm_datamart")

    # Define tag names and corresponding background colors
    tag_colors = {
        'Finish': ('tag_blue', 'blue'),
        'Unfinish': ('tag_red', 'red'),
        'Error': ('tag_green', 'green'),
        '': ('', 'white')
    }

    # Add rows to datamart_tree and assign tags with background colors based on Desc value
    for row in cursor.fetchall():
        item = self.add_datamart(*row)
        desc_value = row[8]
        if desc_value in tag_colors:
            tag_name, bg_color = tag_colors[desc_value]
        else:
            tag_name, bg_color = tag_colors['']
        self.datamart_tree.tag_configure(tag_name, background=bg_color)
        if tag_name:
            self.datamart_tree.item(item, tags=(tag_name,))
