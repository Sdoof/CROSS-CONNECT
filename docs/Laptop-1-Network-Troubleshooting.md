# Laptop 1: Fixing Internet Access

## Scope and verification status

This guide is for Laptop 1 if it runs Windows 10 or Windows 11. Its exact Windows version, model, network adapters, and installed VPN software have not been provided. Do not install Laptop 2's drivers on this computer.

You previously confirmed Laptop 1 had working internet through Helix during Laptop 2's outage. No repair or diagnostic commands have been executed on Laptop 1 during this chat. These instructions are reviewed guidance, not a repair tested on this device.

The related confirmed result was on Laptop 2: internet access returned after uninstalling Windscribe, and you confirmed both Ethernet and Wi-Fi worked. That makes Windscribe worth checking if installed here, but does not establish the cause of any future outage on Laptop 1.

## 1. Check the basics

1. Try a website on another device using the same Helix router.
2. If other devices also fail, check Helix or your provider before changing this computer.
3. If only Laptop 1 fails, continue below.
4. Save work and restart this computer once if you have not already. If the laptop is sluggish, record the Task Manager information described in step 9 first.

For Wi-Fi, turn airplane mode off and connect to Wolfky if that is your usual network. For Ethernet, connect the laptop’s Ethernet port or a USB Ethernet adapter to Helix. If using a dock, check its power and USB connection too.

After each successful fix, skip to the final connection checks. If it does not work, continue to the next section. Do not perform every repair unnecessarily.

## 2. Check Windscribe, if installed

Skip this section if Windscribe is not installed.

1. Open Windscribe and disconnect the VPN.
2. Turn only the **Firewall switch inside Windscribe** OFF temporarily. Leave Microsoft Defender Firewall on.
3. If Firewall mode is Always On, change it to Manual, then turn it off.
4. Try a website.

Closing Windscribe alone may leave its firewall active. This setting is Windscribe's firewall; do not disable Windows Defender Firewall for this procedure.

If disabling Windscribe's Firewall restores access, go to step 8. If Windscribe will not open and its Firewall may remain active, you can use the fix that worked on Laptop 2:

1. Save your work.
2. Open Settings → Apps → Installed apps (Windows 11), or Apps & features (Windows 10).
3. Find Windscribe and select Uninstall.
4. Complete the uninstall and restart Windows.
5. Try a website again.

Uninstalling removes Windscribe's VPN protection. If turning its Firewall off made no difference, continue the diagnostics before uninstalling other software. If this is a managed work computer, have its administrator handle VPN changes instead.

## 3. Check Cold Turkey for website-only blocking

Cold Turkey is relevant when the raw network tests in step 6 work but a browser or particular websites remain blocked. It would not normally explain a missing adapter, a `169.254` address, or failure to reach the Helix gateway.

1. Open Cold Turkey and inspect active website or internet blocks.
2. Pause unlocked blocks using the application.
3. For locked blocks, follow their unlock conditions or wait for expiry.
4. Try a website again.

## 4. Check whether Windows sees the network adapter

Open Start, type Windows PowerShell, and open it. Run:

```powershell
Get-NetAdapter -IncludeHidden | Format-Table Name, InterfaceDescription, Status, LinkSpeed
```

Find the physical Ethernet or Wi-Fi adapter. Its manufacturer and name may differ from Laptop 2's Intel AX201 and Realtek USB adapter.

- Up: a network link is active; this alone does not prove internet access.
- Disconnected: reconnect Wi-Fi or check the cable, router port, and any USB adapter.
- Disabled: if you did not intentionally disable it, enable it through Settings → Network & Internet → Advanced network settings, or Control Panel → Network and Sharing Center → Change adapter settings.
- Missing: open Device Manager → Network adapters and Other devices. Record any warning symbol and the device's Properties → General error code before changing drivers.

Ignore WAN Miniports and virtual adapters for this basic physical-adapter check. If a driver is needed, identify this computer and adapter first and obtain the correct driver from their manufacturer.

## 5. Check the assigned address

Run:

```powershell
ipconfig /all
```

Find the connected physical adapter's section. Record its IPv4 address, default gateway, DHCP status, and DNS servers.

On Laptop 2, Helix previously provided an address beginning with 10.0.0 and gateway 10.0.0.1. Those are reference observations, not verified settings for Laptop 1. Use the current gateway shown on this computer; do not copy Laptop 2's IP address or manually enter its DNS settings.

An IPv4 address beginning with 169.254 usually means Windows did not obtain an IPv4 DHCP lease. A missing IPv4 gateway can also explain lack of IPv4 internet access. Recheck the connection and router access. IPv6-only networks can differ.

If this computer intentionally uses a static IP, record it and ask whoever configured it before changing settings.

## 6. Test where connectivity stops

Run each command separately. Press Esc first if the console is in text-selection mode.

Test one connection at a time. For Ethernet, temporarily disconnect Wi-Fi. For Wi-Fi, unplug Ethernet. If the current default gateway is 10.0.0.1, run the command below. Otherwise replace that address with the current gateway from ipconfig. If no gateway is listed, stop here: reconnect the selected adapter and recheck step 5. If it remains absent, collect the adapter and `ipconfig` details and get device-specific help before running reset commands.

```powershell
ping -n 4 10.0.0.1
```

Test one direct public-IP TCP endpoint without using DNS:

```powershell
Test-NetConnection 1.1.1.1 -Port 443
```

Test whether the configured DNS server answers:

```powershell
nslookup www.microsoft.com
```

Test DNS plus TCP reachability for a website name:

```powershell
Test-NetConnection www.microsoft.com -Port 443
```

Look for TcpTestSucceeded : True in the TCP tests. A failure tests only that endpoint and port; it does not by itself prove all internet access is down. Keep the full error messages and compare the complete set with a browser test.

| Result | Next area to investigate |
|---|---|
| Router ping says General failure | Local Windows networking, VPN software, or a network filter; this is not proof of a specific cause |
| Router responds but internet TCP fails | Routing, filtering, or router restrictions for this device |
| Internet TCP succeeds but DNS fails | DNS configuration or filtering |
| Both TCP tests succeed but websites fail | Browser, proxy settings, or website-blocking software |

Some devices block ping. A single failed ping or external test is not conclusive. A successful TCP connection does not prove a full website loads; also test in the browser.

## 7. Reset Windows networking only if still needed

Use this only after the earlier checks. These commands affect all network adapters. The IP reset can clear a manual IP address, gateway, DNS setting, or route, and the Winsock reset can require VPN or security software to be repaired or reinstalled. If any setting was entered manually, take screenshots and get device-specific help before continuing. For a managed work computer, contact its administrator and do not run the resets yourself.

1. Open Start and type Windows PowerShell.
2. Right-click it → Run as administrator, and approve the prompt.
3. Run:

```powershell
netsh winsock reset
netsh int ip reset
```

4. Read and keep any errors.
5. Restart using Start → Power → Restart.
6. Reconnect and try a website. Restore any required custom settings with appropriate help.

Do not uninstall drivers, disable Windows Firewall, or repeatedly reset networking if the problem remains unexplained.

## 8. Verify the fix and stop

Test each connection this computer actually supports:

1. For Ethernet: connect to Helix. Temporarily disconnect Wi-Fi through its taskbar menu so it cannot mask an Ethernet failure. Open two websites.
2. For Wi-Fi, if available: unplug Ethernet, connect to your usual network, and open two websites.
3. Restore your preferred connection when finished.
4. If the supported connections work, stop. No additional driver changes or resets are needed.

If still blocked, collect the exact error messages, adapter list, relevant ipconfig section, Windows version, computer model, and installed VPN/security software. Ask for help using those results rather than repeating failed tests without a change.

## 9. Protect work and handle sluggish performance

The network repair steps do not diagnose or fix a slow computer. Laptop 1 was reported as sluggish and switched off overnight, but its performance has not been measured here. An overnight shutdown does not establish that the cause is fixed.

### When the laptop is sluggish

1. Save all open work. For files inside OneDrive, wait until OneDrive reports **Up to date**.
2. Open Task Manager with Ctrl+Shift+Esc and note the apps using the most CPU, Memory, and Disk. Check Settings → System → Storage for available space.
3. Choose Start → Power → **Restart**. Restart reloads Windows and its drivers more completely than Shut down can when Fast Startup is enabled.
4. After signing in, compare performance with the notes from step 2. If slowness returns, also open Start and search for **View reliability history**. Record recent failures and driver or Windows updates for a separate performance diagnosis.

Hibernate is useful for preserving a healthy planned session, but it can preserve the same sluggish state. Do not use Hibernate as the fix for a slow session. Close only apps you recognize, and do not end unfamiliar Windows processes. If the laptop is unusually hot, its fan runs constantly, or it remains slow after Restart, diagnose that separately before changing network drivers.

### What each power choice preserves

| Choice | What happens when you return |
|---|---|
| Lock or sign out | Lock keeps the Windows session open; sign out closes it. Local apps cannot keep working after sign-out unless they were specifically designed to run as a Windows service or scheduled task. |
| Sleep | Usually resumes the open session quickly, but still uses some battery and is less suitable for a long unattended pause. |
| Hibernate | Writes the session to disk. You normally press Power once and sign in to resume. It improves recovery but does not guarantee unsaved work. |
| Restart | Closes the session and reloads Windows. Supported apps may reopen, but their exact state and unsaved files are not guaranteed. |
| Shut down or forced power-off | Closes or abruptly ends the session. It cannot guarantee restoration of the previous state or unsaved work. |

A scheduled BIOS power-on is a separate hardware feature on some computers. It does not restore unsaved work, and it has not been configured or tested on Laptop 1.

### Configure safer recovery

1. Save documents normally. For OneDrive files, confirm **Up to date** before a planned shutdown. Sync does not save an unsaved document.
2. Enable each application's AutoSave or recovery feature and confirm its recovery location. Office AutoSave normally requires the document to have already been saved in OneDrive or SharePoint.
3. If available, enable Windows restartable apps: Settings → Accounts → Sign-in options → Additional settings → **Automatically save my restartable apps and restart them when I sign back in**. This supports some apps; it does not preserve every app or unsaved file.
4. For a planned long pause with a healthy session, use Start → Power → **Hibernate**. If Hibernate is missing, open Control Panel → Hardware and Sound → Power Options → Choose what the power buttons do → Change settings that are currently unavailable, then enable Hibernate if Windows offers it.
5. Test recovery with a disposable document: save it in OneDrive, wait for **Up to date**, Hibernate, press Power once, sign in, and confirm the document and applications return. Never test recovery using the only copy of important unsaved work.

After an unexpected interruption, connect the charger, press Power once, and sign in. Wait for OneDrive to finish syncing. Use the affected application's **Recover** or **Recent** files and OneDrive version history if needed. Avoid repeated long power-button holds; use one only when Windows is completely frozen and ordinary shutdown is impossible.

## Optional: record a working baseline

While internet works, keep the outputs of Get-NetAdapter and ipconfig /all with this manual, plus the computer's model and Windows version (Start → Settings → System → About). Store these locally; they can contain network identifiers. Never include Wi-Fi passwords or other credentials.
