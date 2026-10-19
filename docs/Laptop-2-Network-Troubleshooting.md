# Laptop 2: Fixing Wi-Fi and Ethernet Internet Access

Dell Inspiron 5502 · Windows 11 · Helix router · Wi-Fi network: Wolfky

## What fixed it last time

Both Wi-Fi and Ethernet lost internet access. Windows recognized the adapters, and Ethernet received an IP address from Helix. Connection tests failed, including a router ping that returned “General failure.” Other computers on Helix still worked.

After Windows networking resets and a restart, uninstalling Windscribe was followed by restored Ethernet and Wi-Fi access. Windscribe's firewall or network filter is the leading suspect, but the sequence did not isolate which step made the final difference.

## 1. Check other devices

Try the internet on another device connected to Helix.

- Other devices work: continue on Laptop 2.
- Other devices also fail: check Helix or your internet provider first.

For Ethernet, connect the USB-C Ethernet adapter to Helix with the network cable. For Wi-Fi, turn airplane mode off and connect to Wolfky.

If internet access works after reconnecting, go directly to step 8 and verify both connections. Otherwise continue.

## 2. Check Windscribe first

If Windscribe is installed:

1. Open Windscribe and disconnect the VPN.
2. Turn only the **Firewall switch inside Windscribe** OFF. Leave Microsoft Defender Firewall on.
3. If its firewall mode is Always On, change it to Manual, then turn it off.
4. Try opening a website.

Simply closing Windscribe may leave its firewall active.

If disabling Windscribe's Firewall restores access, go to step 8. If Windscribe will not open and its Firewall may still be active, uninstall it as follows:

1. Save your work.
2. Open Start → Settings → Apps → Installed apps.
3. Find Windscribe, click the three dots, and choose Uninstall.
4. Complete the uninstall and restart Windows.
5. Try a website again.

Uninstalling removes Windscribe's VPN protection. Decide whether to reinstall it after confirming normal connectivity. If turning its Firewall off made no difference, continue the diagnostics before uninstalling other software.

## 3. Optional: Cold Turkey for website-only blocking

Skip this section for now. Return only when the network tests in step 6 work but a browser or particular websites remain blocked. Cold Turkey would not normally explain a missing adapter, a `169.254` address, or failure to reach the Helix gateway.

1. Open Cold Turkey and look for active website or internet blocks.
2. Pause unlocked blocks through the application.
3. Try a website again.

For locked blocks, follow the configured unlock conditions or wait for them to expire.

## 4. Check whether Windows sees the adapters

Right-click Start and open Terminal or Windows PowerShell. Run:

```powershell
Get-NetAdapter -IncludeHidden | Format-Table Name, InterfaceDescription, Status, LinkSpeed
```

Look for:

- Realtek USB GbE Family Controller: Up when Ethernet is connected. Previously showed 1 Gbps.
- Intel Wi-Fi 6 AX201: Up when connected to Wolfky.

Disconnected means that adapter currently has no network connection. Ignore WAN Miniport and virtual-adapter entries for this basic check. An active link alone does not prove internet access.

If either physical adapter is missing, reconnect its hardware where applicable and check Device Manager → Network adapters and Other devices for warning symbols. Record the device name and exact error code. Do not install, roll back, or uninstall Intel or Realtek drivers while both adapters appear normally and Device Manager shows no error.

## 5. Check the network address

Run:

```powershell
ipconfig /all
```

Find the section for the connection being used. Previously, Helix supplied:

- IPv4 address: 10.0.0.xxx (the last number can change).
- Subnet mask: 255.255.255.0.
- Default gateway: 10.0.0.1.
- DHCP enabled: Yes.
- DNS servers: 24.201.245.77 and 24.200.243.189.

These are previous observations, not settings to enter manually. Router configuration can change.

An IPv4 address beginning with 169.254, or a missing gateway, suggests the connection has not obtained the expected settings. Check the cable or reconnect to Wolfky, then run `ipconfig /all` again. If the gateway remains missing, stop before the tests or resets below. Record the adapter and `ipconfig` details and get device-specific help.

## 6. Test where connectivity stops

Run each command separately. If PowerShell is in text-selection mode, press Esc first. Test one connection at a time: temporarily disconnect Wi-Fi while testing Ethernet, and unplug Ethernet while testing Wi-Fi.

Test Helix (use the current default gateway from ipconfig if it differs):

```powershell
ping -n 4 10.0.0.1
```

Test one direct public-IP TCP endpoint without using DNS:

```powershell
Test-NetConnection 1.1.1.1 -Port 443
```

Look for TcpTestSucceeded : True. A failure tests only this endpoint and port; by itself it does not prove that all internet access is down.

Test whether the configured DNS server answers:

```powershell
nslookup www.microsoft.com
```

Test DNS plus TCP reachability for a website name:

```powershell
Test-NetConnection www.microsoft.com -Port 443
```

How to interpret the results:

- Router ping reports General failure: investigate local Windows networking or a VPN/security filter.
- Router responds but internet TCP fails: investigate routing, filtering, or router access restrictions.
- Internet TCP succeeds but DNS lookup fails: investigate DNS settings or filtering.
- Both TCP tests succeed but browsing fails: investigate the browser, proxy settings, or website blockers.

A failed ping or one failed TCP endpoint alone is not conclusive. Keep the exact error messages or take photos, and compare the full set of results with a browser test.

## 7. Reset Windows networking only if needed

If the checks above do not restore access, try the Windows networking resets.

Save your work first. These commands affect all network adapters. The IP reset can clear a manual IP address, gateway, DNS setting, or route, and the Winsock reset can require VPN or security software to be repaired or reinstalled. If any setting was entered manually, take screenshots and get device-specific help before continuing. Do not run these resets on a remotely managed computer without its administrator.

1. Right-click Start → Terminal (Admin) or Windows PowerShell (Admin).
2. Approve the Windows prompt.
3. Run:

```powershell
netsh winsock reset
netsh int ip reset
```

4. Restart through Start → Power → Restart.
5. Reconnect and try a website.

The Ethernet connection previously used DHCP and should obtain its network settings automatically again.

## 8. Confirm both connections and stop

1. Connect Ethernet to Helix and temporarily disconnect Wi-Fi from the taskbar menu.
2. Confirm Ethernet is **Up** with the command in step 4, then open two websites.
3. Unplug Ethernet, connect Wi-Fi to Wolfky, and open two websites.
4. Restore the preferred connection when finished.

If both work, stop troubleshooting. No further network resets or driver changes are needed.

If the issue remains, collect the exact error messages, adapter list, relevant ipconfig section, and names of installed VPN/security applications before seeking further help.
