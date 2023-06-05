import sqlite3
import tkinter as tk
from tkinter import ttk

class MyApp:
    def __init__(self, parent):
        self.parent = parent

        # Create a notebook with tabs for datamart, scheduler, and datasource pages
        self.notebook = ttk.Notebook(self.parent)

        self.datamart_tab = tk.Frame(self.notebook)
        self.scheduler_tab = tk.Frame(self.notebook)
        self.datasource_tab = tk.Frame(self.notebook)

        self.notebook.add(self.datamart_tab, text="Datamart")
        self.notebook.add(self.scheduler_tab, text="Scheduler")
        self.notebook.add(self.datasource_tab, text="Datasource")

        # Create Treeview widgets to display data on each tab
        self.user_tree = ttk.Treeview(self.datamart_tab)
        self.task_tree = ttk.Treeview(self.scheduler_tab)
        self.source_tree = ttk.Treeview(self.datasource_tab)

        # Pack the Treeview widgets into their respective tabs
        self.user_tree.pack(fill="both", expand=True)
        self.task_tree.pack(fill="both", expand=True)
        self.source_tree.pack(fill="both", expand=True)

        # Connect to database and update tabs with data
        self.conn = sqlite3.connect('path/to/your/database.db')
        self.update_datamart_tab()
        self.update_scheduler_tab()
        self.update_datasource_tab()

        # Pack the notebook
        self.notebook.pack(fill="both", expand=True)

    def update_datamart_tab(self):
        # Clear existing rows in user_tree
        for row in self.user_tree.get_children():
            self.user_tree.delete(row)

        # Select data from the "users" table in the database
        cursor = self.conn.execute("SELECT name, age, email FROM users")

        # Add rows to user_tree
        for row in cursor.fetchall():
            self.add_user(*row)

    def update_scheduler_tab(self):
        # Clear existing rows in task_tree
        for row in self.task_tree.get_children():
            self.task_tree.delete(row)

        # Select data from the "tasks" table in the database
        cursor = self.conn.execute("SELECT task, due_date, status FROM tasks")

        # Add rows to task_tree
        for row in cursor.fetchall():
            self.add_task(*row)

    def update_datasource_tab(self):
        # Clear existing rows in source_tree
        for row in self.source_tree.get_children():
            self.source_tree.delete(row)

        # Select data from the "sources" table in the database
        cursor = self.conn.execute("SELECT name, type, location FROM sources")

        # Add rows to source_tree
        for row in cursor.fetchall():
            self.add_source(*row)

    def add_user(self, name, age, email):
        """Add a user to the Datamart table."""
        self.user_tree.insert("", "end", values=(name, age, email))

    def add_task(self, task, due_date, status):
        """Add a task to the Scheduler table."""
        self.task_tree.insert("", "end", values=(task, due_date, status))

    def add_source(self, name, type, location):
        """Add a source to the Datasource table."""
        self.source_tree.insert("", "end", values=(name, type, location))


if __name__ == "__main__":
    root = tk.Tk()
    app = MyApp(root)
    root.mainloop()
