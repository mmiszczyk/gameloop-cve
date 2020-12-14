# Multiple vulnerabilities in Tencent Gameloop - privilege escalation, remote code execution


|CVEID          |Name of the affected product(s) and version(s)       |Problem type                    |
|---------------|-----------------------------------------------------|--------------------------------|
|CVE-2020-29008 |Tencent Gameloop (all versions)                      |CWE-284: Improper Access Control|
|CVE-2020-29009 |Tencent Gameloop (all versions prior to 3.21.736.100)|CWE-284: Improper Access Control|

---

**Summary**

Gameloop is an Android emulator released by Tencent. During our tests, we have identified multiple
vulnerabilitieswhich can lead to code execution and privilege escalation inside the guest operating system.

All versions of Gameloop emulator up to 3.21.736.100 allow remote code execution in guest operating system
with root privileges by unsecured ADB connection to port 5555. Additionally, all versions of Gameloop allow
unprivileged local attackers to gain root privileges.
 
**Description**
 
Every version of Tencent GameLoop emulator allows local attackers (e.g., any installed apps, as well as
attackers who achieved code execution through a different vulnerability) to obtain root privileges by calling
the unprotected su binary. The user will not be informed about the attempt to obtain root privileges, as they
would be if the Android root access manager application were used. This is known as **CVE-2020-29008**.
 
Additionally, Tencent GameLoop emulator before 3.21.736.100 accepts ADB connections on TCP port 5555. Those
connections might come from remote machines and allow the attacker to access the emulated Android system with
the permissions of root user. Users of the affected emulator do not receive any prompt to accept or reject
the connection, as they would on most ADB-enabled Android devices. This bug is designated **CVE-2020-29009**.

**Reproduction**

+ **CVE-2020-29008**: ```su -c [command_to_execute]```
+ **CVE-2020-29009**: ```adb connect [emulator_ip_address]:5555; adb shell```
 
**Mitigation**

Update Gameloop to the newest version. Do not install any untrusted applications in Gameloop’s guest
operating system.

