---
title: "Handling Lockfile ransomware and detecting NTLM Relay Attacks"
author: "fr0z830"
description: "The Ransomware TTP'S have evolved throughout the years. In this article, we're going to discuss the identification of several Attacks that have been detected to the known 'LockFile' Ransomware."
date: 2022-02-04
---

### Handling Lockfile ransomware and detecting NTLM Relay Attacks
---

Lately, on the internet, malicious activities involving Ransomware are increasing and upgrading in relation to the actions they take to achieve their purpose.

On August 22, 2021, the Symantec team reported in detail the detection of Ransomware under the name **`LockFile`** and as NTLM Relay Attack did use a well-known PoC name with **`PetitPotam`**.


Threat actors can completely take over a Windows domain with ADCS running without any authentication — they simply need to connect the target server to the LSARPC named pipe with interface c681d488-d850-11d0-8c52-00c04fd90f7e. This allows the attacker to leverage LSARPC to communicate with the Encrypting File System Remote Protocol (MS-EFSRPC) which appears to allow unauthenticated access to provoke an NTLM authentication, which can be then captured. 
       
Τo achieve this, it relies on a known Microsoft vulnerability with **CVE ID: `CVE-2021-36942`** (Windows LSA Spoofing Vulnerability) and Impacket tools (ntlmrelayx, kekeo, mimikatz).


#### CVE ID: `CVE-2021-36942`
* #### Description:

    An unauthenticated attacker could call a method on the *`LSARPC`* interface and coerce the domain controller to authenticate against another server using NTLM. This security update blocks the affected API calls `OpenEncryptedFileRawA` and `OpenEncryptedFileRawW` through LSARPC interface.

In case the necessary updates have not been made by the client and if there is an avoidance of recognition of the incident (Defense Evasion) by security mechanisms (eg EDR), the attack will be detected based on the following:

#### Search for any suspicious login:

- **Event id:** `4624`
- **Authentication Package:** `NTLM`
- **ElevatedToken:** `*1842`
- **User:** `ANONYMOUS LOGON`
- **Event Log:** `Security`

#### Search for malicious share access:

- **Event Log =** `Security`
- **Event ID =** `5145`
- **Object Name =** `*IPC*`
- **Target Name =** `lsarpc` or `efsrpc` or `lsass` or `samr` or `netlogon`

Another solution according to the detection of the aforementioned activity, is to implement a SIEM solution on your infrastructure.

Below, you will find a NTLM relay attack detection which has been implemented in Azure KQL according to the Cybermonk's repository:

```javascript
# Indication Of NTLM Relay Attack
let DCs = dynamic (["yourdc1.yourdomain.local", "yourdc2.yourdomain.local"]);
DeviceLogonEvents
| where Protocol == "NTLM"
| where AccountName endswith "$"
| where DCs has replace_string (AccountName, "$", "")
```


Last but not least, the other vulnerability that has been associated with the above-mentioned PoC, is that the IIS Server that is used for the Active Directory Certificate Services (AD CS), uses NTLM over HTTP for authentication. Therefore, an attacker could use Impacket to relay NTLM authentication towards the ADCS IIS Server.

In this process, we first send a `POST` HTTP request to the `/certsrv/certfnsh.as` endpoint with an automatically generated certificate. While doing this it also passes the NTLM credentials.

If the `POST` request was successful, the Active Directory Certificate Services server will sign the certificate and Responder will fetch it by sending a `GET` HTTP request to `/certsrv/certnew.cer?ReqID=` where the parameter will be provided as the response to the POST request.

With the Certificate now, the attacker can now use the `Rubeus` tool to fetch a Kerberos TGT (Ticket Granting Ticket), by using the machine account that was initially abused to make the NTLM connection. If that machine was a domain controller, we can get the `TGT` as that domain controller machine account, which will then ultimately allow the attacker to fully compromise the domain.

#### References:
- [Symantec Blogpost](https://symantec-enterprise-blogs.security.com/blogs/threat-intelligence/lockfile-ransomware-new-petitpotam-windows)
- [PetitPotam](https://github.com/topotam/PetitPotam)
- [Credential Access Azure KQL Rules](https://github.com/Cyb3r-Monk/Threat-Hunting-and-Detection/blob/main/Credential%20Access)
- [Detecting PetitPotam AD CS and other Domain Controller Account Takeovers](https://posts.bluraven.io/detecting-petitpotam-and-other-domain-controller-account-takeovers-d3364bd9ee0a)
- [Active Directory Certificate Services (ADCS - PKI) domain admin vulnerability](https://isc.sans.edu/diary/27668)
- [Microsoft Update Guide](https://msrc.microsoft.com/update-guide/vulnerability/ADV210003)

###### fr0z830's social links
- [[HackTheBox]](https://app.hackthebox.com/profile/39199)
