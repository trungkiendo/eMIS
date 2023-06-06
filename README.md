frame = ttk.Frame(self.datamart_tab)
frame.pack(fill="both", expand=True)

self.datamart_tree = ttk.Treeview(frame, columns=(
    "Tb_No", "Datamart", "Table", "Old_Runtime", "Start_Runtime", "End_Runtime", "Rownum", "Status", "Desc"),
    show="headings")
self.datamart_tree.pack(side="left", fill="both", expand=True)

# Thêm thanh cuộn
scrollbar = ttk.Scrollbar(frame, orient="vertical", command=self.datamart_tree.yview)
scrollbar.pack(side="right", fill="y")
self.datamart_tree.configure(yscrollcommand=scrollbar.set)
