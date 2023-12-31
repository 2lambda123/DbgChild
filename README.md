# DbgChild - Debug Child Process Tool 

![](/Images/DbgChildProcess.png?raw=true) DbgChild is a stand alone tool for debugging child processes (auto attach). DbgChild can be used in conjunction with a plugin for a debugger. Currently DbgChild supports a plugin for the x86/x64 x64dbg debugger. 

Support for DbgChild can be extended to OllyDbg and Immunity debugger if so required via donations. Consider make a donation: https://github.com/sponsors/therealdreg

---

**WARNING: If you are using an AV this plugin can fail (AV hooks in ZwCreateUserProcess etc...)**

# Recommended settings

You have to select the checkboxes in the DbgChild plugin to automatically attach x64dbg to any process started by the executable you’re currently debugging:

![recommended_settings.png](https://github.com/David-Reguera-Garcia-Dreg/DbgChild/blob/master/recommended_settings.png)

---

**WARNING: You must select the checkboxes in both versions of x64dbg, openning x64dbg.exe and x32dbg.exe**

Keep open always the NewProcessWatcher.exe

# Example of usage

DbgChild x64dbg plugin how to use example video. x32_cmd -> x64_cmd -> x32_cmd -> x32_calc:
https://www.youtube.com/watch?v=NfA2HAJa0Rk

https://mrexodia.github.io/reversing/2017/07/12/Analyzing-torrent-repack-malware

https://ragegorilla08.medium.com/qakbot-analysis-d5ea5f5a38c4

# Features

* Hook process creation for x86 or x64 child processes
* Patching and unpatching of NTDLL process creation for x86 and x64 child processes
* Process watcher for auto launching of new x64dbg instance when child process detected
* Modify the suspend (pre) and resume (post) logic to adapt to your own requirements

# Content
The DbgChild comprises a number of components to accomplish the task of launching a new x64dbg instance when a child process is hooked and detected. These components are:

* CreateProcessPatch.exe - Hook ZwCreateUserProcess (two separate exe files for x86 and x64) and loads DbgChildHookDLL.dll
* DbgChildHookDLL.dll - (two separate dll files for x86 and x64) - outputs process id's to CPIDS folder
* NTDLLEntryPatch.exe - Patches or unpatches LdrInitializeThunk (two separate exe files for x86 and x64)
* DbgChild.dp32 - x64dbg plugin x86 
* DbgChild.dp64 - x64dbg plugin x64
* NewProcessWatcher.exe - Watches for new child processes from the CPIDS folder
* x64_post.unicode.txt - Support file
* x64_pre.unicode.txt - Support file
* x86_post.unicode.txt - Support file
* x86_pre.unicode.txt - Support file

# Download
Download the latest release of DbgChild [here](https://github.com/David-Reguera-Garcia-Dreg/DbgChild/releases)

# Installation

* Download the latest version of x64dbg [here](https://github.com/x64dbg/x64dbg/releases)
* Extract the contents of the latest release archive to your x64dbg folder

Once extracted the contents should look something like this:

```
\x64dbg\NewProcessWatcher.exe
\x64dbg\x64_post.unicode.txt
\x64dbg\x64_pre.unicode.txt
\x64dbg\x86_post.unicode.txt
\x64dbg\x86_pre.unicode.txt
\x64dbg\x32\CreateProcessPatch.exe
\x64dbg\x32\DbgChildHookDLL.dll
\x64dbg\x32\NTDLLEntryPatch.exe
\x64dbg\x32\plugins\DbgChild.dp32
\x64dbg\x32\CPIDS\
\x64dbg\x64\CreateProcessPatch.exe
\x64dbg\x64\DbgChildHookDLL.dll
\x64dbg\x64\NTDLLEntryPatch.exe
\x64dbg\x64\plugins\DbgChild.dp64
\x64dbg\x64\CPIDS\
```

* Menu options for the DbgChild plugin is available under the "Plugins" menu in the main x64dbg window


# Plugin Menu Overview

![](/Images/HookProcess.png?raw=true) `Hook Process Creation` - CreateProcessPatch.exe hooks ZwCreateUserProcess and loads DbgChildHookDLL.dll. There is a x86 version and x64 version of CreateProcessPatch.exe

![](/Images/Checkmark.png?raw=true)` Auto from x32dbg/x64dbg Hook Process Creation` - Toggle option to switch on or off the automatic hooking of the process creation. If it is off, then user must manually select Hook Process Creation at some point before child processes are spawned.

![](/Images/ClearCPIDS.png?raw=true) `Clear x32|x64\CPIDS` - Clear all process id file entries from the x32\CPIDS or x64\CPIDS folder

![](/Images/BrowseCPIDS.png?raw=true) `Open x32|x64\CPIDS` - Opens in explorer the x32\CPIDS or x64\CPIDS folder 

![](/Images/AddCPIDS.png?raw=true) `Create New Entry x32|x64\CPIDS` - Adds a new entry to the x32\CPIDS or x64\CPIDS folder 

![](/Images/PatchNTDLL.png?raw=true) `Patch NTDLL Entry` - Patches the ntdll.dll LdrInitializeThunk function.

![](/Images/UnpatchNTDLL.png?raw=true) `Unpatch NTDLL Entry` - Unpatches the ntdll.dll LdrInitializeThunk if it has previously been patched

![](/Images/Checkmark.png?raw=true) `Auto From x32dbg|x64dbg Unpatch NTDLL Entry` - Toggle option to switch on or off the automatic unpatch of the NTDLL entry when 2nd x64dbg instance is launched for child process. If it is off, then user must manually select Unpatch NTDLL Entry in the 2nd x64dbg instance after it has launched

![](/Images/NewProcessWatcher.png?raw=true) `Launch NewProcessWatcher` - Starts NewProcessWatcher.exe which monitors the x32\CPIDS or x64\CPIDS folder for new process id files that are created by DbgChildHookDLL.dll when a child process is detected and is about to be spawned

![](/Images/NewProcessWatcher.png?raw=true) `Launch NewProcessWatcher With Old Processes` - 

![](/Images/Checkmark.png?raw=true) `Launch from x32dbg|x64dbg NewProcessWatcher Without Ask` - Toggle option to switch on or off the automatic prompt to launch NewProcessWatcher. If on then when Hook Process Creation is selected, NewProcessWatcher will automatically launch. If off, then it will display a prompt asking user if they wish to launch NewProcessWatcher

![](/Images/GotoHook.png?raw=true) `Go to Hook Process Creation` - Shows in the x32dbg|x64dbg cpu disassembly window the location of the hook code

![](/Images/GotoNTDLL.png?raw=true) `Go to NTDLL Patch` - Shows in the x32dbg|x64dbg cpu disassembly window the location of the ntdll.dll patch

![](/Images/EditSuspended.png?raw=true) `Edit x32|x64 Suspended Command` - Opens x86_pre.unicode.txt or x64_pre.unicode.txt in notepad for editing

![](/Images/EditResumed.png?raw=true) `Edit x32|x64 Resumed Command` - Opens x86_post.unicode.txt or x64_post.unicode.txt in notepad for editing

![](/Images/RemoteHookProcess.png?raw=true) `Remote x32|x64 PID Hook Process Creation` - Asks for a process id to remotely hook process creation for

![](/Images/RemoteNTDLLPatch.png?raw=true) `Remote x32|x64 PID Patch NTDLL Entry` - Asks for a process id to remotely patch the ntdll.dll LdrInitializeThunk function for

![](/Images/RemoteNTDLLUnpatch.png?raw=true) `Remote x32|x64 PID Unpatch NTDLL Entry` - Asks for a process id to remotely unpatch the ntdll.dll LdrInitializeThunk if it has previously been patched

![](/Images/OpenLogs.png?raw=true) `Open Logs` - Open log files

![](/Images/ClearLogs.png?raw=true) `Clear Logs` - Clear log files

![](/Images/Checkmark.png?raw=true) `Auto From x32|x64 Open Logs` - Toggle option to switch on or off the automatic opening of the log file

![](/Images/Help.png?raw=true) `Help` - Displays information on the usage of the plugin and its operations

![](/Images/DbgChildProcess.png?raw=true) `Plugin Info By Dreg` - About dialog box showing information about this plugin

# Credits

* [David Reguera Garcia - Dreg](https://github.com/therealdreg): DbgChild coding and design
* [Keith Robertson - mrfearless](https://github.com/mrfearless): Documentation, GUI and icons


