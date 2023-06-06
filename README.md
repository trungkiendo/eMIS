self.datamart_tree = ttk.Treeview(self.datamart_tab, columns=(
    "Tb_No", "Datamart", "Table", "Old_Runtime", "Start_Runtime", "End_Runtime", "Rownum", "Status", "Desc"),
    show="headings")

# Thêm khung bao quanh bảng
self.datamart_tree.configure(borderwidth=2, relief="solid")

# Thêm đường kẻ ngang và đứng
self.datamart_tree.grid(column=0, row=0, sticky="nsew")
self.datamart_tab.columnconfigure(0, weight=1)
self.datamart_tab.rowconfigure(0, weight=1)
