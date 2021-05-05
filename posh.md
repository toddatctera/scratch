# PowerShell

## PSReadLine - Like GNU Readline but for PowerShell.

### Usage

Run these to import module and set readline mode.

	Import-Module PSReadLine
	Set-PSReadLineOption -EditMode Emacs

To always import and set PSReadLine, add it to PowerShell profile

	notepad $PROFILE

In case of SecurityError:

```
. : File C:\Users\Username\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1 cannot be loaded because running scripts is disabled on this system. For more information, see
about_Execution_Policies at https:/go.microsoft.com/fwlink/?LinkID=135170.
At line:1 char:3
+ . 'C:\Users\Username\Documents\WindowsPowerShell\Microsoft.PowerSh ...
+   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : SecurityError: (:) [], PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
```

Make sure scripts are allowed to be excuted.

Run PowerShell as Administrator

	Set-ExecutionPolicy RemoteSigned
	
And re-launch PowerShell

### Install
[install](https://github.com/PowerShell/PSReadLine#installation) - Probably already done on recent versions of Windows 10
> If you are using Windows PowerShell on Windows 10 or using PowerShell 6+, PSReadLine is already installed. Windows PowerShell on the latest Windows 10 has version 2.0.0-beta2 of PSReadLine. PowerShell 6+ versions have the newer prerelease versions of PSReadLine
>

### Sources:
[ms docs](https://docs.microsoft.com/en-us/powershell/module/psreadline/?view=powershell-7.1)

[github](https://github.com/PowerShell/PSReadLine)
