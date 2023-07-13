import os
import subprocess

dir_path = 'path/to/macro/folder'
xlsm_files = [os.path.join(dir_path, f) for f in os.listdir(dir_path) if f.endswith('.xlsm')]

for file_path in xlsm_files:
    bat_file_path = file_path + '.bat'
    with open(bat_file_path, 'w') as f:
        f.write(f'@echo off\nstart "" /wait "{file_path}"\n')
    subprocess.run(bat_file_path, shell=True)
