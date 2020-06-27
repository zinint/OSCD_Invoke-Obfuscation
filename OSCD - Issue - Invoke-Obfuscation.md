## Hello, fellow OSCD participant!
This is your guide on how you can help us all with soving this issue.     
You can pick up some of the methods of PowerShell command and script obfuscation provided by the [Invoke-Obfuscation](https://github.com/danielbohannon/Invoke-Obfuscation) framework below and develop Sigma rules for them. You will need to use [regular expressions](https://github.com/Neo23x0/sigma/wiki/Specification#types) in Sigma rules. Moreover the Invoke-Obfuscation User Guide:
* Part 1 is [here](https://www.danielbohannon.com/blog-1/2017/12/2/the-invoke-obfuscation-usage-guide); 
* Part 2 is [here](https://www.danielbohannon.com/blog-1/2017/12/2/the-invoke-obfuscation-usage-guide-part-2); 
* etc.  
It's always good to know the instrument (:

#### Original code (before obfuscation)
```powershell
# command example
Invoke-Expression (New-Object Net.WebClient).DownloadString
# variable example
$env:path
```
#### Just pick the obfuscation method and the relevant сases you prefer and develop Sigma rule(s) for them. When you're done, create a Pull Request to OSCD Sigma branch and specify this issue's number and the case numbers you've solved:  
(*e.g., "Develop Sigma rules for Invoke-Obfuscation #578 Case #1,3"*) <br/> (*e.g., "Develop Sigma rules for Invoke-Obfuscation #578 Case #1-15"*) <br/>
#### But remember that our main goal is to detect the method itself, not a specific command.
#### A little tip for your regex development:
you can copy the results from all cases for one or more obfuscation methods and paste them in [regex101](https://regex101.com/) to find possible similarities while developing a regex (you can save your progress there and even apply a dark theme (: ). 

## Contents
##### SINGLE OBFUSCATION
  - [TOKEN OBFUSCATION](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#token-obfuscation)
  - [STRING OBFUSCATION](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#string-obfuscation)
  - [ENCODING OBFUSCATION](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#encoding-obfuscation)
  - [COMPRESS OBFUSCATION](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#compress-obfuscation)
  - [PS LAUNCHER OBFUSCATION](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#ps-launcher-obfuscation)
  - [CMD LAUNCHER OBFUSCATION](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#cmd-launcher-obfuscation)
  - [WMIC LAUNCHER OBFUSCATION](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#wmic-launcher-obfuscation)     
  - [RUNDLL LAUNCHER OBFUSCATION](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#rundll-launcher-obfuscation)
  - [VAR+ LAUNCHER OBFUSCATION](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#var-launcher-obfuscation)
  - [STDIN+ LAUNCHER OBFUSCATION](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#stdin-launcher-obfuscation)
  - [CLIP+ LAUNCHER OBFUSCATION](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#clip-launcher-obfuscation)
  - [VAR++ LAUNCHER OBFUSCATION](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#var-launcher-obfuscation-1)
  - [STDIN++ LAUNCHER OBFUSCATION](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#stdin-launcher-obfuscation-1)
  - [CLIP++ LAUNCHER OBFUSCATION](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#clip-launcher-obfuscation-1)
  - [RUNDLL++ LAUNCHER OBFUSCATION](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#rundll-launcher-obfuscation-1)
  - [MSHTA++ LAUNCHER OBFUSCATION](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#mshta-launcher-obfuscation)
##### MULTIPLE OBFUSCATION
  - *Will be added later as well as the [second original command](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#original-command-before-obfuscation) (to find similarities in one obfuscation method for different commands)*

### TOKEN OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)<br/>
*TOKEN\STRING\1&2 skipped, because there are not any String tokens to obfuscate, but they do Concatenate and Reoder just like TOKEN\ARGUMENT\3&4 (Cases #4&5)*
<table style="word-break: keep-all;">
 <tr>
  <th align="center">Case #</th>
  <th align="center">Option</th>
  <th align="center">Results</th>
  <th align="center">Comments</th>
 </tr>
 <tr>
  <td align="center">1</td>
  <td align="center" nowrap>
   <p>TOKEN\COMMAND\1</p>
   <p>TOKEN\ARGUMENT\2</p>
   <p>TOKEN\MEMBER\2</p>
  </td>
  <td nowrap>
   <strong>TOKEN\COMMAND\1</strong>
   <p>IN`V`o`Ke-eXp`ResSIOn (Ne`W-ob`ject Net.WebClient).DownloadString</p>
   <p>IN`V`OKE-exPRE`Ss`i`oN (n`eW-O`BjECT Net.WebClient).DownloadString</p>
   <p>IN`VOke-expr`eSS`ioN (NE`w-`o`BjECt Net.WebClient).DownloadString</p>
   <strong>TOKEN\ARGUMENT\2</strong>
   <p>Invoke-Expression (New-Object n`eT.Web`Clie`Nt).DownloadString</p>
   <p>Invoke-Expression (New-Object Ne`T.WEb`CLIe`Nt).DownloadString</p>
   <p>Invoke-Expression (New-Object n`ET.w`E`BCLIEnt).DownloadString</p>   
   <strong>TOKEN\MEMBER\2</strong>
   <p>Invoke-Expression (New-Object Net.WebClient)."Do`W`NLOa`dStriNg"</p>
   <p>Invoke-Expression (New-Object Net.WebClient)."D`OWnlOa`DSTring"</p>
   <p>Invoke-Expression (New-Object Net.WebClient)."D`O`wnLo`AD`StrinG"</p>
  </td>
  <td align="left">These options apply ticks.</td>
 </tr>
 <tr>
  <td align="center">2</td>
  <td align="center">TOKEN\COMMAND\2</td>
  <td nowrap>
   <p>&('In'+'voke-Expressi'+'o'+'n') (.('New-Ob'+'jec'+'t') Net.WebClient).DownloadString</p>
   <p>.('Inv'+'oke-Ex'+'pr'+'ess'+'ion') (&('Ne'+'w'+'-O'+'bject') Net.WebClient).DownloadString</p>
   <p>.('Invok'+'e-'+'Ex'+'pressio'+'n') (.('Ne'+'w-Ob'+'ject') Net.WebClient).DownloadString</p>
   <p>&('Invok'+'e-'+'Expr'+'ession') (&('New'+'-O'+'bj'+'ect') Net.WebClient).DownloadString</p>
  </td>
  <td align="left">This option does Splatting + Concatenate.</td>
 </tr>
 <tr>
  <td align="center">3</td>
  <td align="center">TOKEN\COMMAND\3</td>
  <td nowrap> 
   <p>&("{3}{4}{2}{1}{0}{5}"-f'o','essi','pr','Invo','ke-Ex','n') (.("{0}{2}{1}"-f 'Ne','t','w-Objec') Net.WebClient).DownloadString</p>
   <p>.("{0}{3}{2}{1}{4}" -f'I','-Ex','oke','nv','pression') (&("{2}{0}{1}" -f 'Obje','ct','New-') Net.WebClient).DownloadString</p>
   <p>.("{2}{3}{0}{1}"-f'o','n','Invoke-E','xpressi') (.("{0}{1}{2}"-f'Ne','w-O','bject') Net.WebClient).DownloadString</p>
   <p>&("{2}{3}{0}{4}{1}"-f 'e','Expression','I','nvok','-') (&("{0}{1}{2}"-f'N','ew-O','bject') Net.WebClient).DownloadString</p>
  </td>
  <td align="left">This option does Splatting + Reorder</td>
 </tr>
 <tr>
  <td align="center">4</td>
  <td align="center" nowrap>
   <p>TOKEN\ARGUMENT\3</p>
   <p>TOKEN\MEMBER\3</p>
  </td>
  <td nowrap> 
   <strong>TOKEN\ARGUMENT\3</strong>
   <p>Invoke-Expression (New-Object ('Ne'+'t.W'+'ebClient')).DownloadString</p>
   <p>Invoke-Expression (New-Object ('Net.W'+'eb'+'Client')).DownloadString</p>
   <p>Invoke-Expression (New-Object ('Net.We'+'b'+'Client')).DownloadString</p>   
   <strong>TOKEN\MEMBER\3</strong>
   <p>Invoke-Expression (New-Object Net.WebClient).('Download'+'S'+'t'+'ring')</p>
   <p>Invoke-Expression (New-Object Net.WebClient).('Down'+'lo'+'adS'+'tring')</p>
   <p>Invoke-Expression (New-Object Net.WebClient).('Down'+'load'+'Stri'+'ng')</p>
  </td>
  <td align="left">Just Concatenate</td>
 </tr> 
 <tr>
  <td align="center">5</td>
  <td align="center">TOKEN\ARGUMENT\4</td>
  <td nowrap> 
   <p>Invoke-Expression (New-Object ("{2}{3}{0}{1}{4}"-f'bClie','n','N','et.We','t')).DownloadString</p>
   <p>Invoke-Expression (New-Object ("{0}{1}{2}{3}"-f'Net','.W','ebClie','nt')).DownloadString</p>
   <p>Invoke-Expression (New-Object ("{1}{0}{2}" -f 't.W','Ne','ebClient')).DownloadString</p>
  </td>
  <td align="left">Just Reorder</td>
 </tr> 
 <tr>
  <td align="center">6</td>
  <td align="center"></td>
  <td nowrap> 
   <p></p>
   <p></p>
   <p></p>
   <p></p>
  </td>
  <td align="left"></td>
 </tr> 
 <tr>
  <td align="center"></td>
  <td align="center"></td>
  <td nowrap> 
   <p></p>
   <p></p>
   <p></p>
   <p></p>
  </td>
  <td align="left"></td>
 </tr>
</table> 

### STRING OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)


### ENCODING OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)


### COMPRESS OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)


### PS LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)

### CMD LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)

### WMIC LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)


### RUNDLL LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)


### VAR+ LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)


### STDIN+ LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)


### CLIP+ LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)

### VAR++ LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)


### STDIN++ LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)

### CLIP++ LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)


### RUNDLL++ LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)

### MSHTA++ LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)

