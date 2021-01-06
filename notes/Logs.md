---
title: Logs
created: '2020-12-20T05:19:11.973Z'
modified: '2020-12-20T05:27:21.503Z'
---

# Logs
# Posershell execution policy
Powershell may show error while exucuting a script like `vue create app`.
The reason is execution policy not set correctly. To set the execution policy for running such scripts open up elevated CMD and execute following command.
```bash
powershell Set-ExecutionPolicy RemoteSigned
```
Once done, The execution policy may be restricted again form powershell itself.
```bash
Set-ExecutionPolicy Restricted
```
Execution policy of X64 and X86 version are required to be set separately from corresponding version of CMD.

> x86 (32 bit)
Open C:\Windows\SysWOW64\cmd.exe
Run the command powershell Set-ExecutionPolicy RemoteSigned
>
> x64 (64 bit)
Open C:\Windows\system32\cmd.exe
Run the command powershell Set-ExecutionPolicy RemoteSigned

[Stack overflow](https://stackoverflow.com/questions/4037939/powershell-says-execution-of-scripts-is-disabled-on-this-system)
[Microsoft Doc](https://docs.microsoft.com/en-in/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.1)



