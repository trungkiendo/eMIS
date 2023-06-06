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
self.datasource_tree.heading("Desc", text="Desc", anchor="w")          "SELECT Code, Datamart, libname, tabname, DailyYN, crdate, crtime, FileSize, RunYN, Desc FROM dm_datasource")
