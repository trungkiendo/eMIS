import subprocess

# Gọi tệp VB.NET để chạy SAS EGP
subprocess.Popen\
(['C:\\Windows\\Microsoft.NET\\Framework\\v4.0.30319\\vbnc.exe', '/out:C:\\path\\to\\output\\RunSAS.exe', '-target:exe', '-platform:x64', '-quiet', '-utf8output', '''
Imports System
Imports System.IO
Imports System.Collections.Generic
Imports SAS.Shared.Names
Imports SAS.Tasks.Toolkit
Imports SAS.Tasks.Toolkit.Controls
Imports SAS.Tasks.Toolkit.Data
Imports SAS.Tasks.Toolkit.DataImportExport
Imports SAS.Tasks.Toolkit.DataUtilities
Imports SAS.Tasks.Toolkit.Text

Public Class Program
    Public Shared Sub Main(ByVal args As String())
        Dim sasWorkspaceManager As New SASWorkspaceManager()
        Dim workspace As ISASTaskWorkspace = sasWorkspaceManager.CreateWorkspaceByServerName("MyServer")
        Dim sasTask As New SAS.EG.ProjectElements.Tasks.ETL.SASProgramTask(workspace)
        sasTask.Program = "C:\path\to\your\SAS\EGP\file.egp"
        sasTask.Run()
    End Sub
End Class
'''])

# Chờ đợi quá trình chạy tệp VB.NET hoàn tất
subprocess.Popen.wait()
