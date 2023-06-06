import sqlite3
import tkinter as tk
from tkinter import ttk
import openpyxl

class DatamartScreen:
    def __init__(self, parent, show_home_screen):
        self.parent = parent
        self.show_home_screen = show_home_screen
        self.filename_var = tk.StringVar()  # Create a variable to store the filename


        # Create datamart screen with tabs for datamart, scheduler, and datasource pages
        datamart_screen = tk.Toplevel(self.parent)
        datamart_screen.title("Datamart Screen")
        datamart_screen.geometry("600x400")

        self.notebook = ttk.Notebook(datamart_screen)

        self.datamart_tab = tk.Frame(self.notebook)
        self.scheduler_tab = tk.Frame(self.notebook)
        self.datasource_tab = tk.Frame(self.notebook)

        self.notebook.add(self.datamart_tab, text="Datamart")
        self.notebook.add(self.scheduler_tab, text="Scheduler")
        self.notebook.add(self.datasource_tab, text="Datasource")

        # Create Treeview widgets to display data on each tab
        self.datamart_tree = ttk.Treeview(self.datamart_tab, columns=(
            "Tb_No", "Datamart", "Table", "Old_Runtime", "Start_Runtime", "End_Runtime", "Rownum", "Status", "Desc"),show="headings")
        # Thêm thanh cuộn
        scrollbar = ttk.Scrollbar(self.datamart_tab, orient="vertical", command=self.datamart_tree.yview)
        scrollbar.pack(side="right", fill="y")
        self.datamart_tree.configure(yscrollcommand=scrollbar.set)
        self.datamart_tree.pack(side="left", fill="both", expand=True)

        self.scheduler_tree = ttk.Treeview(self.scheduler_tab, columns=(
        "Code", "Datamart", "Code_SAS", "Groups", "Status", "Time_check", "RunYN", "Run_time", "Time_Finish", "Desc"),show="headings")
        # Thêm thanh cuộn
        scrollbar = ttk.Scrollbar(self.scheduler_tab, orient="vertical", command=self.scheduler_tree.yview)
        scrollbar.pack(side="right", fill="y")
        self.scheduler_tree.configure(yscrollcommand=scrollbar.set)
        self.scheduler_tree.pack(side="left", fill="both", expand=True)

        self.datasource_tree = ttk.Treeview(self.datasource_tab, columns=(
        "Code", "Datamart", "libname", "tabname", "DailyYN", "crdate", "crtime", "FileSize", "RunYN", "Desc"),show="headings")
        # Thêm thanh cuộn
        scrollbar = ttk.Scrollbar(self.scheduler_tab, orient="vertical", command=self.datasource_tree.yview)
        scrollbar.pack(side="right", fill="y")
        self.datasource_tree.configure(yscrollcommand=scrollbar.set)
        self.datasource_tree.pack(side="left", fill="both", expand=True)
        # Thêm khung bao quanh bảng
        # self.datamart_tree.pack(side="left", fill="both", expand=True)
        # self.scheduler_tree.configure(borderwidth=2, relief="solid")
        # self.datasource_tree.configure(borderwidth=2, relief="solid")

        # Define column headings for Treeview widgets
        self.datamart_tree.column("Tb_No", width=50, anchor="center")
        self.datamart_tree.column("Datamart", width=100, anchor="center")
        self.datamart_tree.column("Table", width=150, anchor="w")
        self.datamart_tree.column("Old_Runtime", width=150, anchor="center")
        self.datamart_tree.column("Start_Runtime", width=150, anchor="center")
        self.datamart_tree.column("End_Runtime", width=150, anchor="center")
        self.datamart_tree.column("Rownum", width=80, anchor="center")
        self.datamart_tree.column("Status", width=80, anchor="center")
        self.datamart_tree.column("Desc", width=200, anchor="w")

        self.datamart_tree.heading("Tb_No", text="Tb_No", anchor="center")
        self.datamart_tree.heading("Datamart", text="Datamart", anchor="center")
        self.datamart_tree.heading("Table", text="Table", anchor="w")
        self.datamart_tree.heading("Old_Runtime", text="Old_Runtime", anchor="center")
        self.datamart_tree.heading("Start_Runtime", text="Start_Runtime", anchor="center")
        self.datamart_tree.heading("End_Runtime", text="End_Runtime", anchor="center")
        self.datamart_tree.heading("Rownum", text="Rownum", anchor="center")
        self.datamart_tree.heading("Status", text="Status", anchor="center")
        self.datamart_tree.heading("Desc", text="Desc", anchor="w")

        self.datasource_tree.column("Code", width=80, anchor="center")
        self.datasource_tree.column("Datamart", width=100, anchor="center")
        self.datasource_tree.column("libname", width=100, anchor="w")
        self.datasource_tree.column("tabname", width=150, anchor="w")
        self.datasource_tree.column("DailyYN", width=80, anchor="center")
        self.datasource_tree.column("crdate", width=100, anchor="center")
        self.datasource_tree.column("crtime", width=100, anchor="center")
        self.datasource_tree.column("FileSize", width=100, anchor="center")
        self.datasource_tree.column("RunYN", width=80, anchor="center")
        self.datasource_tree.column("Desc", width=200, anchor="w")

        self.datasource_tree.heading("Code", text="Code", anchor="center")
        self.datasource_tree.heading("Datamart", text="Datamart", anchor="center")
        self.datasource_tree.heading("libname", text="libname", anchor="w")
        self.datasource_tree.heading("tabname", text="tabname", anchor="w")
        self.datasource_tree.heading("DailyYN", text="DailyYN", anchor="center")
        self.datasource_tree.heading("crdate", text="crdate", anchor="center")
        self.datasource_tree.heading("crtime", text="crtime", anchor="center")
        self.datasource_tree.heading("FileSize", text="FileSize", anchor="center")
        self.datasource_tree.heading("RunYN", text="RunYN", anchor="center")
        self.datasource_tree.heading("Desc", text="Desc", anchor="w")

        self.scheduler_tree.column("Code", width=80, anchor="center")
        self.scheduler_tree.column("Datamart", width=100, anchor="center")
        self.scheduler_tree.column("Code_SAS", width=80, anchor="center")
        self.scheduler_tree.column("Groups", width=100, anchor="center")
        self.scheduler_tree.column("Status", width=80, anchor="center")
        self.scheduler_tree.column("Time_check", width=150, anchor="center")
        self.scheduler_tree.column("RunYN", width=80, anchor="center")
        self.scheduler_tree.column("Run_time", width=150, anchor="center")
        self.scheduler_tree.column("Time_Finish", width=150, anchor="center")
        self.scheduler_tree.column("Desc", width=200, anchor="w")

        self.scheduler_tree.heading("Code", text="Code", anchor="center")
        self.scheduler_tree.heading("Datamart", text="Datamart", anchor="center")
        self.scheduler_tree.heading("Code_SAS", text="Code_SAS", anchor="center")
        self.scheduler_tree.heading("Groups", text="Groups", anchor="center")
        self.scheduler_tree.heading("Status", text="Status", anchor="center")
        self.scheduler_tree.heading("Time_check", text="Time_check", anchor="center")
        self.scheduler_tree.heading("RunYN", text="RunYN", anchor="center")
        self.scheduler_tree.heading("Run_time", text="Run_time", anchor="center")
        self.scheduler_tree.heading("Time_Finish", text="Time_Finish", anchor="center")
        self.scheduler_tree.heading("Desc", text="Desc", anchor="w")

        # Pack the Treeview widgets into their respective tabs
        self.datamart_tree.pack(fill="both", expand=True)
        self.scheduler_tree.pack(fill="both", expand=True)
        self.datasource_tree.pack(fill="both", expand=True)

        # Connect to database and update tabs with data
        self.conn = sqlite3.connect('MIS.db')
        self.update_datamart_tab()
        self.update_scheduler_tab()
        self.update_datasource_tab()

        # Pack the notebook
        self.notebook.pack(fill="both", expand=True)

    def update_datamart_tab(self):
        # Clear existing rows in datamart_tree
        for row in self.datamart_tree.get_children():
            self.datamart_tree.delete(row)

        # Select data from the "dm_datamart" table in the database
        cursor = self.conn.execute(
            "SELECT Tb_No, Datamart, 'Data table', Old_Runtime, Start_Runtime, End_Runtime, Rownum, Status, Desc FROM dm_datamart")

        # Add rows to datamart_tree
        for row in cursor.fetchall():
            self.add_datamart(*row)

    def update_scheduler_tab(self):
        # Clear existing rows in scheduler_tree
        for row in self.scheduler_tree.get_children():
            self.scheduler_tree.delete(row)

        # Select data from the "dm_datamart" table in the database
        cursor = self.conn.execute(
            "SELECT Code, Datamart, Code_SAS, Groups, Status, Time_check, RunYN, Run_time, Time_Finish, Desc FROM dm_scheduler")

        # Add rows to scheduler_tree
        for row in cursor.fetchall():
            self.add_scheduler(*row)

    def update_datasource_tab(self):
        # Clear existing rows in datasource_tree
        for row in self.datasource_tree.get_children():
            self.datasource_tree.delete(row)

        # Select data from the "dm_datamart" table in the database
        cursor = self.conn.execute(
            "SELECT Code, Datamart, libname, tabname, DailyYN, crdate, crtime, FileSize, RunYN, Desc FROM dm_datasource")

        # Add rows to datasource_tree
        for row in cursor.fetchall():
            self.add_datasource(*row)

    def add_datamart(self, Tb_No, Datamart, Table, Old_Runtime, Start_Runtime, End_Runtime, Rownum, Status, Desc):
        """Add a row to the Datamart table."""
        self.datamart_tree.insert("", "end", values=(
        Tb_No, Datamart, Table, Old_Runtime, Start_Runtime, End_Runtime, Rownum, Status, Desc))

    def add_scheduler(self, Code, Datamart, Code_SAS, Groups, Status, Time_check, RunYN, Run_time, Time_Finish, Desc):
        """Add a row to the Scheduler table."""
        self.scheduler_tree.insert("", "end", values=(
        Code, Datamart, Code_SAS, Groups, Status, Time_check, RunYN, Run_time, Time_Finish, Desc))

    def add_datasource(self, Code, Datamart, libname, tabname, DailyYN, crdate, crtime, FileSize, RunYN, Desc):
        """Add a row to the Datasource table."""
        self.datasource_tree.insert("", "end", values=(
        Code, Datamart, libname, tabname, DailyYN, crdate, crtime, FileSize, RunYN, Desc))

        # Add an "Export to Excel" button to the Datamart tab
        # export_button = tk.Button(self.datamart_tab, text="Export to Excel", command=lambda: self.export_to_excel(self.datamart_tree))
        # export_button.pack()
        #
        # # Add an "Export to Excel" button to the Datasource tab
        # export_button = tk.Button(self.datasource_tab, text="Export to Excel", command=lambda: self.export_to_excel(self.datasource_tree))
        # export_button.pack()

        self.notebook.pack(expand=1, fill="both")
        self.notebook.select(self.datamart_tab)

        # # Add back button to return to home screen
        # back_button = tk.Button(datamart_screen, text="Back", command=self.hide)
        # back_button.pack()
        #
        # # Update current screen
        # self.current_screen = datamart_screen

    # def add_user(self, name, age, email):
    #     """Add a user to the Datamart table."""
    #     self.user_tree.insert("", "end", values=(name, age, email))
    #
    # def add_job(self, name, description, frequency):
    #     """Add a job to the Scheduler table."""
    #     self.job_tree.insert("", "end", values=(name, description, frequency))
    #
    # def add_datasource(self, name, location, description):
    #     """Add a datasource to the Datasource table."""
    #     self.datasource_tree.insert("", "end", values=(name, location, description))

    def run_job(self):
        """Run the selected job."""
        selected_job = self.scheduler_tree.selection()
        if selected_job:
            job_name = self.scheduler_tree.item(selected_job)["values"][0]
            print(f"Running job: {job_name}")

    def double_click_job(self, event):
        """Handle double click event on the job_tree."""
        selected_job = self.scheduler_tree.selection()
        if selected_job:
            job_name = self.scheduler_tree.item(selected_job)["values"][0]
            print(f"Hello {job_name}")

    def export_to_excel(self, tree):
        """Export the data from the selected tab to an Excel file."""
        # Create a new Excel workbook and worksheet
        workbook = openpyxl.Workbook()
        worksheet = workbook.active

        # Write the headings to the worksheet
        headings = [tree.heading(column)["text"] for column in tree["columns"]]
        worksheet.append(headings)

        # Write the data to the worksheet
        for row in tree.get_children():
            values = [tree.item(row)["values"][column] for column in range(len(tree["columns"]))]
            worksheet.append(values)

        # Get the tab's title to name the file
        tab_title = self.notebook.tab(self.notebook.select(), "text")
        filename = f"{tab_title} Data.xlsx"

        # Save the workbook to a file
        workbook.save(filename)

        # Update the filename label
        # filename = f"{tab_title} Data.xlsx"
        # workbook.save(filename)
        # self.filename_var.set(f"Exported to {filename}")

    def hide(self):
        self.current_screen.withdraw()
        self.show_home_screen()
