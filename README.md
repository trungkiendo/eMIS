def update_datamart_tab(self):
    # Clear existing rows in datamart_tree
    self.datamart_tree.delete(*self.datamart_tree.get_children())

    # Define a dictionary to map values in the "Desc" column to background colors
    desc_to_color = {'A': 'blue', 'B': 'red', 'C': 'green'}

    # Select data from the "dm_datamart" table in the database
    cursor = self.conn.execute(
        "SELECT Tb_No, Datamart, 1, Old_Runtime, Start_Runtime, End_Runtime, Rownum, Status, Desc FROM dm_datamart")

    # Add rows to datamart_tree
    for row in cursor.fetchall():
        # Determine the background color based on the value in the "Desc" column
        background = desc_to_color.get(row[8], 'white')
        
        # Add the row to the treeview with the determined background color
        self.add_datamart(*row, background=background)
