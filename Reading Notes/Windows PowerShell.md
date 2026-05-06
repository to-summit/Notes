$PSversionTable....
1. v-n
	+ v: get, set, remove, new
	+ n: process, childitem ...
	example: get-process (-Name ... ) | sort-object CPU
2. cmdlet: powershell command  

get-command
get-help, update-help

> get-command -Name \*service\*
> get-help get-service -examples -online 

get-member: all elements in powershell is object

3. file operations
set-location -path
get-childitem
new-item -path  -itemtype 

> new-item -path README.md -itemtype file/directory/...

copy-item/move-item -path -destination 
remove-item -path (-recurse)
rename-item
4. variables and data types
	1. variables: 
		+ $serviceName = "Spooler"; $serviceName; $timeout = 30
		+ $serviceName.gettype(), $timeout.gettype()
		+ $servers = "ServerA" "ServerB" "ServerC"; servers[0]
		+ $user = @{ Name= "Admin"; ID = 1001; Department = "IT" }; $user.Name
		$_ is a special variable standing for currently processed object.
		Get-Process | Where-Object { $\_.WorkingSet -gt 100MB }
5. Fundamental Objects and The Pipeline
~~traditional: text string~~ powershell: objects
	+ get-process | where-object { $\_.name -eq "powershell" } # filter
	+ get-process | select-object name, id, cpu # select
	+ get-process | sort-object cpu -descending  # sort
	get-process | where-object { $\_.VM -gt 1MB } | sort-object VM -Descending | select-object name, vm -first 5

6. fundamental practical cmdlets
	1. get-process | sort-object CPU -descending | select-object -first 5
	2. get-netipconfiguration 
	3. get-service | where-object { $\_.status -eq "Running" -and $\_.Displayname -match "Windows" } 
	4. get-psdrive -PSProvider FileSystem 
	5. get-winevent -filterhashtable @{ logname = "System"; level = 2 } -maxevent 5
7. IF statement 
``` powershell
# if (condition) { }
$number = 10
if ($number -eq 10) {
	# -eq, -ne, -gt, -ge, -lt, -le 
	write-host "The number is exactly 10."
} elseif {
	 ... 
} else {...}

# -and, -or

```
8.  loops and iterations 
``` powershell
$services = "BITS", "Spooler", "WinRM"
foreach ($s in $services) {
	write-host "Check service: $s"
}

for ($i = 0; $i -lt 3; $i++) { ... }

while ($counter -lt 5) { ... }
```
9. functions 
function func-name {
	 Param(
		 [Parameter(Mandatory=$true, HelpMessage = "..." )]
		 [String] $Name, 
		 ...
	 )
	/body/
	return ...
}
10.  manage windows services
	+ Get-service -Name TermService  -DispalyName # * for globbing
	+ Stop-sevice -Name ...
	+ Start-Service -Name ...
	+ Restart-Service -Name ... 
	+ Set-Service -Name ... -StartupType Manual 
11. manage windows server 
	+ Get-Credential 
	+ Export-CliXml , Import-CliXml
	+ Install-Module 
12. WinRM & ssh (who f\*\*king use this ?)
	+ Enter-PSSession -ComputerName "..." -Credential (Get-Credential)
	+ Invoke-Command -ComputerName "..." -ScriptBlock { ... } 
	+ ssh: Enter-PSSession -HostName "..." -UserName "..."  
	> need to configure on server first : 
> 	Subsystem powershell "path/to/pwsh.exe" -sshs -NoLogo

13.  Asynchronously
``` powershell
	$networkScanScript = {
		# This define a script block
	}
	. /path/to/script.ps1
	$job = Start-job -ScriptBlock $networkScanScript -Argument "..."
```
+ $job | get-job
+ $job | receive-job # just can do once  or use -keep
+ $job | remove-job
+ remove-job -id 1
+ get-job -state completed | remove-job 

14.  DHCP, DNS and Active Directory (on Server)
Get-DhcpServerv4Scope -ComputerName ...
get-dnsserverzone -computerName ...
Get-DnsServerResourceRecord -ComputerName ... -Zonename "..." -RRType ...

