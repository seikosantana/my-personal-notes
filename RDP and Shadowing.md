# RDP and Shadowing

In today's search of RDP and shadowing with multiple sessions, here's what I found.

## Multple Session with RDP

Use [RDPWrap](https://github.com/stascorp/rdpwrap) to allow multiple RDP connections

## No Disconnecting with Shadowing

To connect RDP without logging out the active user, use shadowing. Shadowing is available in `mstsc`.  
Use `mstsc /h` to show the help. Help explains enough and is intuitive enough. Enabling the shadowing is more of an issue.

### Enable Shadowing on Windows

- Open Local Group Policy Editor (GPO) or `gpedit.msc`
- Navigate to `Computer Configuration -> Administrative Templates -> Windows components -> Remote Desktop Services -> Remote Session Host -> Connections`
- Double click `Set rules for remote control of Remote Desktop Services user sessions`
- `Enable` the policy
- Choose `Full Control without user's permission`
- Allow `File and Printer Sharing (SMB-In)` and `Remote Desktop - Shadow (TCP-In)` firewall rule.


After all those configurations, you should be able to connect with `/shadow:sessionid /noConsentPrompt /v:computer_name_or_ip /prompt` using `mstsc`.

Session ID should be 1 when the specified user entered with `/prompt` is logged in.

## External Sources

https://woshub.com/rdp-session-shadow-to-windows-10-user/
