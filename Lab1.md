# Lab 1. Program analysis to determine its malicious nature

The goal is gathering evidence to confirm or deny the malicious nature of the analyzed program.

### Sources for identifying programs with questionable reputations

For an experience as close as the real life, it's recommended to look for malicious programs from the following sources:

1. Cracked apps, like: windows/office activator, cracked adobe products, free antivirus, etc;
2. Intrusively promoted apps on websites for: free movies, crypto, bets, dating;
3. Untrusted groups/forums;
4. Own creativity, past experiences, currently used cracked apps.

## Analysis tools
- <span id="pe-bear">[pe-bear](https://github.com/hasherezade/pe-bear)</span>;
- <span id="ghidra">[Ghidra](https://ghidra-sre.org/)</span>;
- <span id="ilspy">[ILSpy](https://github.com/icsharpcode/ILSpy)</span>;
- <span id="process-monitor">[ProcessMonitor](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon)</span>;
- <span id="system-informer">[System Informer (former Process Hacker)](https://github.com/winsiderss/systeminformer/)</span>;
- <span id="wireshark">[Wireshark](https://www.wireshark.org/)</span>;
- <span id="autoruns">[Autoruns](https://learn.microsoft.com/en-us/sysinternals/downloads/autoruns)</span>;
- <span id="tcpview">[TCPView](https://learn.microsoft.com/en-us/sysinternals/downloads/tcpview)</span>;
- <span id="regshot">[Regshot](https://github.com/Seabreg/Regshot)</span>.
<!-- - <span id="api-monitor">[API Monitor](http://www.rohitab.com/apimonitor)</span>; -->

## Steps

### 1. Malware analysis environment

1. Download and install [virtualbox](https://www.virtualbox.org/);
2. Set up the virtual machine:
    1. Download and import the [virtual machine](https://drive.google.com/file/d/1O5W01PXMTLXvzyNJ_qwlHRW2r-7JUk1M/view?usp=sharing) (You can follow [these](https://www.maketecheasier.com/import-export-ova-files-in-virtualbox/) steps for vm importing);
    2. Learn about snapshots and why do we need them. [Here](https://www.howtogeek.com/150258/how-to-save-time-by-using-snapshots-in-virtualbox/) you have a small and clear article on this subject.<br>
    You should have a snapshot:
   - after import;
   - after installing the required tools you need for analysis;
   - before any run of the malicious program. This allows you to run the program as many times as you need, and every time "on clean", like it never run before on that machine.
3. Install the tools (read until the end to know exactly what you'll need).
4. It's recommended to remove any [shared folders ](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fi.ytimg.com%2Fvi%2F9-teQnZ8LEY%2Fmaxresdefault.jpg&f=1&nofb=1&ipt=5e92e63d84e0ebbefcc30a916ce628e443edfdee594b3c0cc2a4053f092923c2&ipo=images) and set the [network](https://www.codesandnotes.be/wp-content/uploads/2018/10/VirtualBox-virtual-machine-adapter2-settings.jpg) type as `NAT`.

### 2. Static analysis

At this step the following tools will be used:
- [pe-bear](#pe-bear)
  - Obtain human readable strings. Find imported functions from libraries that are not present into include table of the executable file. Find domain names. Look for suspicious text, like: hacked, ransomware, tor browser, bitcoin;
  - WinAPI functions to predict the behavior;
  - The structure of the binary file. If it has empty holes (parts filled with 0 and not real instructions), it is most likely the program wants to avoid detection by antivirus software.

- [ghidra](#ghidra) - software reverse engineer tool which can generate a pseudo cod based on the executable file.
- [ILSpy](#ilspy) - .NET assembly browser and decompiler.

### 3. Dynamic analysis

Don't think you can do this step from the first try. The learning path is made of multiple retries.

Start [wireshark](#wireshark), [procmon](#process-monitor), analyze the data they catch, try to master the filters for your needs. Try saving the data to an offline file (offline - nothing is being analyzed).

Run [System Informer](#system-informer), [autoruns](#autoruns), [TCPView](#tcpview), [regshot](#regshot), play with them, learn what they do.

> Before running any malicious program, don't forget to create a snapshot

> A good idea would be to record the screen during the analysis. VirtualBox has this feature build in.

There are many malware analysis articles online, it's fine to learn from them, but:
> If the final report has results from tools any other than the ones recommended here, it may be considered as cheating!