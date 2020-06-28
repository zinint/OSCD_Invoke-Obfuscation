## Hello, fellow OSCD participant!
This is your guide on how you can help us all with soving this issue.     
You can pick up some of the methods of PowerShell command and script obfuscation provided by the [Invoke-Obfuscation](https://github.com/danielbohannon/Invoke-Obfuscation) framework below and develop Sigma rules for them. You will need to use [regular expressions](https://github.com/Neo23x0/sigma/wiki/Specification#types) in Sigma rules. Moreover the Invoke-Obfuscation User Guide:
* Part 1 is [here](https://www.danielbohannon.com/blog-1/2017/12/2/the-invoke-obfuscation-usage-guide); 
* Part 2 is [here](https://www.danielbohannon.com/blog-1/2017/12/2/the-invoke-obfuscation-usage-guide-part-2);<br/>
*It's always good to know the instrument (:*
* And a great presentation is [here](https://www.sans.org/cyber-security-summit/archives/file/summit-archive-1492186586.pdf), it basically shows why we are doomed...joking, there's just a lot of work to do together:<br/> "*Real Security versus Hope fueled by Ignorance*" – Jeffrey Snover.       

#### Original code (before obfuscation)
```powershell
# command example
Invoke-Expression (New-Object Net.WebClient).DownloadString
# variable example
$env:path
# type token example
[Scriptblock]::Create("Write-Host $env:path")
```
#### Just pick the obfuscation method and the relevant сases you prefer and develop Sigma rule(s) for them. When you're done, create a Pull Request to OSCD Sigma branch and specify this issue's number and the case numbers you've solved:  
(*e.g., "Develop Sigma rules for Invoke-Obfuscation #578 Case #1,3"*) <br/> (*e.g., "Develop Sigma rules for Invoke-Obfuscation #578 Case #1-15"*) <br/>
#### Remember that our main goal is to detect the method itself, not a specific command.
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
  <td align="left">These options apply Ticks.</td>
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
  <td align="center" nowrap>
   <p>TOKEN\ARGUMENT\4</p>
   <p>TOKEN\MEMBER\4</p>
  </td>
  <td nowrap> 
   <strong>TOKEN\ARGUMENT\4</strong>
   <p>Invoke-Expression (New-Object ("{2}{3}{0}{1}{4}"-f'bClie','n','N','et.We','t')).DownloadString</p>
   <p>Invoke-Expression (New-Object ("{0}{1}{2}{3}"-f'Net','.W','ebClie','nt')).DownloadString</p>
   <p>Invoke-Expression (New-Object ("{1}{0}{2}" -f 't.W','Ne','ebClient')).DownloadString</p>
   <strong>TOKEN\MEMBER\4</strong>
   <p>Invoke-Expression (New-Object Net.WebClient).("{2}{1}{4}{0}{3}"-f 'dStrin','n','Dow','g','loa')</p>
   <p>Invoke-Expression (New-Object Net.WebClient).("{2}{3}{1}{0}"-f 'String','d','D','ownloa')</p>
   <p>Invoke-Expression (New-Object Net.WebClient).("{2}{1}{3}{0}"-f 'g','nl','Dow','oadStrin')</p>
  </td>
  <td align="left">Just Reorder</td>
 </tr> 
 <tr>
  <td align="center">6</td>
  <td align="center">TOKEN\VARIABLE\1</td>
  <td nowrap> 
   <p>${En`V:`p`ATh}</p>
   <p>${e`Nv:pATh}</p>
   <p>${ENv:`path}</p>
  </td>
  <td align="left">This option applies Random Case + {} + Ticks</td>
 </tr> 
 <tr>
  <td align="center">7</td>
  <td align="center">TOKEN\TYPE\1</td>
  <td nowrap> 
   <p>  Set-ItEM  VaRIABLe:Lcx (  [TyPE]('SC'+'rIP'+'TB'+'LOck') );  (vARIABlE  lCx ).vALUE::Create("Write-Host $env:path")</p>
   <p> sV  ("5Y"+"X")  (  [typE]('SCrIpTBLo'+'C'+'k'))  ;    ( iTEm  ('vaR'+'iabL'+'e:5'+'yx') ).VALue::Create("Write-Host $env:path")</p>
   <p>  SET  F9cg  (  [tYpE]('scr'+'I'+'PTBLo'+'Ck') ) ;   ( gCI  vaRiABLe:F9CG ).vALuE::Create("Write-Host $env:path")</p>
   <p> SET-Variable  ('V'+'IR') ([TyPE]('SC'+'rI'+'PtBlo'+'CK')  )  ;  $VIr::Create("Write-Host $env:path")</p>
  </td>
  <td align="left">This option applies Type Cast + Concatenate</td>
 </tr> 
 <tr>
  <td align="center">8</td>
  <td align="center">TOKEN\TYPE\2</td>
  <td nowrap> 
   <p>Set-itEM  vaRiAbLE:YsB  (  [tYPe]("{1}{3}{0}{2}"-f'C','SCrIP','K','tblO') ) ;   (  GET-vArIAblE  YSb  ).vAlUE::Create("Write-Host $env:path")</p>
   <p> set-ITEm ('VAri'+'aBL'+'E'+':Y'+'7w8o') ([typE]("{2}{0}{3}{1}"-f'c','LoCK','s','RIPTb')  ) ; ( geT-ChILditEM ('VARI'+'aBL'+'e'+':y'+'7w8O') ).vALue::Create("Write-Host $env:path")</p>
   <p>SEt-ItEM  ('vAriAb'+'l'+'e:p87z2')  ([TyPe]("{2}{0}{1}"-F 'tBl','OCK','ScriP') ) ; (  ItEM ('VaRiab'+'L'+'E:P87Z2')).vaLUe::Create("Write-Host $env:path")</p>
   <p>  $094 =  [tyPE]("{1}{0}{3}{2}"-F'C','s','TbLoCK','riP')  ;   $094::Create("Write-Host $env:path")</p>
  </td>
  <td align="left">This option applies Type Cast + Reorder</td>
 </tr> 
 <tr>
  <td align="center">9</td>
  <td align="center">TOKEN\ALL\1</td>
  <td nowrap> 
   <p>.("{0}{3}{1}{2}{4}{5}" -f 'Inv','Expre','s','oke-','si','on') (                      .("{2}{1}{0}" -f'ct','je','New-Ob') ("{2}{0}{1}"-f 'e','bClient','Net.W')               ).("{2}{0}{1}{3}" -f 'ownl','oad','D','String')</p>
   <p>.("{1}{0}{4}{3}{2}" -f'e-E','Invok','on','ressi','xp') (.("{1}{2}{0}" -f 'Object','New','-') ("{1}{2}{0}{3}"-f 'en','Net.WebC','li','t')).("{0}{3}{2}{4}{1}" -f'Do','ing','l','wn','oadStr')</p>
   <p>&("{0}{1}{3}{2}"-f'I','nvoke','ession','-Expr') (&("{1}{0}{2}"-f'Obj','New-','ect') ("{2}{0}{4}{1}{3}" -f 'Cl','en','Net.Web','t','i')).("{1}{2}{3}{0}" -f'g','DownloadSt','r','in')</p>
   <p>&("{3}{4}{1}{0}{2}" -f'si','pres','on','Invoke-','Ex') (.("{1}{2}{0}"-f't','N','ew-Objec') ("{1}{2}{0}"-f 't','Ne','t.WebClien')).("{1}{2}{3}{0}" -f'g','Down','load','Strin')</p>
   <p>.("{3}{2}{0}{1}"-f 're','ssion','-Exp','Invoke') (.("{2}{0}{3}{1}" -f'-Ob','t','New','jec') ("{2}{1}{3}{4}{0}"-f'Client','t.','Ne','We','b')).("{0}{2}{3}{1}" -f 'Dow','String','nl','oad')</p>
  </td>
  <td align="left"></td>
 </tr>
</table> 

### STRING OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)
<table style="word-break: keep-all;">
 <tr>
  <th align="center">Case #</th>
  <th align="center">Option</th>
  <th align="center">Results</th>
  <th align="center">Comments</th>
 </tr>
 <tr>
  <td align="center">10</td>
  <td align="center" nowrap>
   <p>STRING\1</p>
   <p>STRING\2</p>
   <p>STRING\3</p>
  </td>
  <td nowrap>
   <p><strong>Covered by the great author himself (even for a method commented out in the code)</strong></p>
   <p>https://github.com/Neo23x0/sigma/blob/master/rules/windows/powershell/powershell_invoke_obfuscation_obfuscated_iex.yml</p>
   <p>https://github.com/Neo23x0/sigma/blob/master/rules/windows/process_creation/win_invoke_obfuscation_obfuscated_iex_commandline.yml</p>
   <p>https://github.com/Neo23x0/sigma/blob/master/rules/windows/builtin/win_invoke_obfuscation_obfuscated_iex_services.yml</p>
   <p><strong>Again, don't hesitate to check the work done and improve it, if you know how.</strong></p>
  </td>
  <td align="left">These options can Concatenate entire command || Reorder entire command after concatenating. || Reverse entire command after concatenating</td>
 </tr>
 </table> 

### ENCODING OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)
#### ENCODING
<table style="word-break: keep-all;">
 <tr>
  <th align="center">Case #</th>
  <th align="center">Option</th>
  <th align="center">Results</th>
  <th align="center">Comments</th>
 </tr>
 <tr>
  <td align="center">11</td>
  <td align="center">ENCODING\1</td>
  <td>
   <p><strong>Partialy covered by the same Sigma rules mentioned in case 10, that's because the source code block is copy/pasted into almost every encoding function so they can maintain zero dependencies and work on their own. These are examples of some not covered obfuscations:</strong></p>
   <p> IEx([StrING]::JOin('', ( '34@32@36:40k32R83P101k116~32R32u39~111T102k83~39u32k32~39@39z41~32z34T43:32@91R115u84_114k73k78P71R93~40u32u40@55u51~32u44T49u49k48@44P49P49T56@44_32u49z49@49R44~32k49R48k55~44k49_48T49:32k44:52u53z32@44u32z54~57k32R44z49R50z48P32P44z32k49u49_50~44_32k49z49R52R32:44:49@48u49k32T44@32R49T49P53:32@44:32u49u49k53_44~32@49@48:53@32z44R32:49R49_49@44~49P49:48T44_32u51z50:
   32P44z52T48u32@44T55_56u44_49P48:49:44P32:49u49T57u32z44k52k53z44@32z55k57P32_44@32z57k56k32T44~32:49@48R54P44P49~48T49P32k44~32T57R57_44~49u49u54:44R32:51~50P32z44P32@55P56_32_44k32@49T48:49R44T49u49@54~44R52z54z44z56R55_44~49T48k49u32R44_57k56k44:54~55:32:44:32R49k48~56k44R32~49_48k53z32P44:49~48:49~32u44k32u49_49_48:
   32T44u49R49_54R44T52T49u44~52z54u44R32T54k56:32k44u49:49~49z44_32T49P49_57~44@49R49u48u32:44T32R49z48z56u44k32T49P49~49k44P57u55:44z32z49@48z48~32u44@32T56@51R32_44@49T49@54k44T32:49u49~52R44u32:49:48~53_32P44u32:49:49u48~44R49z48z51R41_124@32@70:79u114P101@65:67:104k45k79z98_74@69k67:
   116@123~32z40T91k105T110~116u93z36_95P32_45@97R83P32z91k99R104R65_82P93k41k125P41R43k34@32T36P40z83u69u116T45_105:116:69R109k32R39P86@65k82u105@65@98k76:101:58u111:70k83R39~32k39P32P39k32k41P34z124:32P46~32P40z32_36k80k115P104z111R77u101R91P50_49T93T43k36u112P115P72:79_77u69P91k51~52@93P43u39k120P39_41'.sPLiT( 'uz@kT_:~RP' )|ForeACH-ObJeCT{([ChaR][int]$_) }) ) )</p>
   <p> "$( SET-ItEM  'vARiABLE:oFs' '')"+[STrIng]( ( 73 ,110,118, 111,107, 101, 45 , 69, 120,112 ,114 , 101, 115, 115 , 105 , 111, 110 ,32,40 ,78 ,101, 119, 45 , 79,98 ,106, 101 ,99 , 116 , 32,78 , 101,116,46 , 87 ,101 , 98 , 67,108, 105,101,110 , 116 ,41 ,46, 68, 111 ,119, 110,108,111,97 ,100 ,83 ,116 ,114 ,105,110, 103) | FOrEAch{ ( [iNt]$_-AS[chAr]) }) +" $(seT-VaRiABLe  'ofS' ' ' )" | InvoKE-exprESsiOn</p>
   <p>( '73%110q118q111<107x101K45!69d120d112x114x101v115K115!105!111d110q32}40x78}101>119q45q79<98%106!101d99>116x32q78K101>116<46q87!101v98<67v108%105v101!110v116%41v46>68>111q119}110v108q111q97}100x83!116%114%105q110>103'-SPLiT'<'-SpLit 'd'-SpliT'%'-sPLiT'}' -SPLIT'!' -split 'q' -spLit 'K'-SPLIT'>' -SpLIT'v'-spLit'x'|%{ ( [ChAR] [iNt]$_)})-joiN''| InvOkE-eXPRESSIon</p>
   <p> inVoKe-ExPResSion ( -jOiN((73 , 110,118, 111, 107,101, 45 ,69 ,120, 112 , 114 ,101 , 115,115,105,111, 110 ,32,40 , 78,101, 119 ,45, 79,98 , 106 ,101, 99,116,32 , 78,101 , 116 ,46 ,87,101,98 , 67 , 108,105, 101, 110 ,116 , 41 ,46, 68 ,111,119,110 ,108 , 111, 97, 100, 83, 116 ,114, 105, 110,103)|foreAch{([INt] $_ -aS [cHAr]) }) ) </p>
  </td>
  <td align="left">This option encodes the entire command as ASCII.</td>
 </tr>
 <tr>
  <td align="center">12</td>
  <td align="center">ENCODING\2</td>
  <td nowrap> 
   <p></p>
   <p></p>
   <p></p>
   <p></p>
  </td>
  <td align="left">This option encodes the entire command as Hex.</td>
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

