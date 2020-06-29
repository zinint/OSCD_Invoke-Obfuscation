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
#### Remember that our main goal here is to detect the obfuscation method itself, not a specific command.
Some obfuscations are already covered by the Invoke-Obfuscation author himself, even for the method commented out in the framework's code:
[Sigma Rule #1](https://github.com/Neo23x0/sigma/blob/master/rules/windows/powershell/powershell_invoke_obfuscation_obfuscated_iex.yml),
[Sigma Rule #2](https://github.com/Neo23x0/sigma/blob/master/rules/windows/process_creation/win_invoke_obfuscation_obfuscated_iex_commandline.yml),
[Sigma Rule #3](https://github.com/Neo23x0/sigma/blob/master/rules/windows/builtin/win_invoke_obfuscation_obfuscated_iex_services.yml). You'll encounter patterns from these rules further on, that's because the source code block is copy/pasted into almost every encoding function so they can maintain zero dependencies and work on their own. Due to that fact you'll see some similar obfuscation results in different cases, that shouldn't distract you from our goal - we want to be able to detect the obfuscation method itself, not an obfuscation of a particular command, e.g. in [task 25](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#stdin-launcher-obfuscation) with the STDIN+ launcher, you should pay attention to ```cmd /c``` and ```| powershell``` patterns rather than the used command example or the relevant ```$SHElLID[1]+$ShELlId[``` pattern. 

#### A little tip for your regex development:
you can copy the results from all tasks for one or more obfuscation methods and paste them in [regex101](https://regex101.com/) to find possible similarities while developing a regex (you can save your progress there and even apply a dark theme (: ). 

#### Case Sensitivity.
For the sake of this issue and until no further adieu, we shall consider that we're able to apply all regexes as not case sensitive or that all events are lowercased in a pipeline.

#### IMPORTANT! This is Open Security Collaborative Development, which is an Open international cybersecurity specialist initiative aiming to solve common problems, share knowledge, and improve general security posture - DO NOT HESITATE to check the work done and improve it, if you know how!

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

### TOKEN OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)<br/>
*TOKEN\STRING\1&2 skipped, because there are not any String tokens to obfuscate, but they do Concatenate and Reoder just like TOKEN\ARGUMENT\3&4 (Cases #4&5)*
<table style="word-break: keep-all;">
 <tr>
  <th align="center">Task #</th>
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
  <td> 
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
  <td> 
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
  <th align="center">Task #</th>
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
  <td>
   <p><strong>Covered by the Invoke-Obfuscation author himself (<i>if you'll be able to implement a non case sensitive regex</i>), even for the method commented out in the code:</strong></p>
   <p><a href="https://github.com/Neo23x0/sigma/blob/master/rules/windows/powershell/powershell_invoke_obfuscation_obfuscated_iex.yml">Rule # 1</a></p>
   <p><a href="https://github.com/Neo23x0/sigma/blob/master/rules/windows/process_creation/win_invoke_obfuscation_obfuscated_iex_commandline.yml">Rule # 2</a></p>
   <p><a href="https://github.com/Neo23x0/sigma/blob/master/rules/windows/builtin/win_invoke_obfuscation_obfuscated_iex_services.yml">Rule # 3</a></p>
   <p><strong>You'll encounter patterns from these rules further on, that's because the source code block is copy/pasted into almost every encoding function so they can maintain zero dependencies and work on their own.</p></strong>
   <p><strong>Again, don't hesitate to check the work done and improve it, if you know how.</strong></p>
  </td>
  <td align="left">These options can Concatenate entire command || Reorder entire command after concatenating || Reverse entire command after concatenating</td>
 </tr>
</table>

### ENCODING OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)
<table style="word-break: keep-all;">
 <tr>
  <th align="center">Task #</th>
  <th align="center">Option</th>
  <th align="center">Results</th>
  <th align="center">Comments</th>
 </tr>
 <tr>
  <td align="center">11</td>
  <td align="center">ENCODING\1</td>
  <td>
   <p><strong>Partialy covered by the same Sigma rules mentioned in case 10 (<i>if you'll be able to implement a non case sensitive regex</i>), that's because the source code block is copy/pasted into almost every encoding function so they can maintain zero dependencies and work on their own. These are examples of some not covered obfuscations:</strong></p>
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
  <td>
   <p><strong>Partialy covered by the same Sigma rules mentioned in case 10 (<i>if you'll be able to implement a non case sensitive regex</i>), that's because the source code block is copy/pasted into almost every encoding function so they can maintain zero dependencies and work on their own. These are examples of some not covered obfuscations:</strong></p> 
   <p>-joIn ( '49_6e-76_6fP6b_65{2d!45_78V70_72{65-73!73P69!6fG6eP20P28!4e{65G77{2d!4fV62~6a{65~63_74~20-4eP65_74!2e_57-65{62{43G6c~69P65G6eG74P29P2eG44!6f-77V6eV6cP6f{61P64~53~74_72!69G6e-67'-SpLIT '_' -SPlIt '~' -SPLIT '!'-SPlIt '{' -SPLIt 'G'-SPlit'P'-SPLit 'V'-sPLit '-' |% { ( [ConVERT]::TOinT16( ( $_.TOSTrInG() ), 16 ) -As[chAR]) } )| INvoKe-eXPReSSION</p>
   <p> ( '49}6eU76w6f:6b:65U2dV45w78V70w72:65V73,73}69}6fU6e}20:28>4e,65>77U2dV4fV62,6a-65>63,74w20V4eU65U74:2e:57>65V62>43:6c-69:65U6eU74}29}2e>44U6f:77w6e,6c:6f>61V64>53-74}72V69}6ew67'.SpLIT('VU},w>:-')|foREAch-obJect { ( [conVeRt]::ToiNT16(( [string]$_ ), 16 ) -as [ChAR])} ) -join'' |IEx</p>
   <p> IEX([StRIng]::jOin('' ,('49>6ex76~6f>6bo65x2d%45%78%70}72}65~73w73~69>6f%6e;20w28~4e;65%77>2d;4fw62;6ax65;63}74%20>4eo65o74%2e>57}65%62>43~6c>69;65~6e~74o29;2e%44w6fx77;6ew6cw6fx61o64x53%74o72~69~6e~67'.SPlIT( '%~o};>xw' )| %{ ([ConVERt]::TOINt16( ( [striNg]$_), 16 ) -as[CHar])}))) </p>
   <p> "$( sEt-ITeM  'VarIABle:ofs' '') " +[STrinG]((49 , '6e', 76,'6f' , '6b' , 65,'2d' ,45, 78,70, 72 , 65 ,73 ,73 ,69,'6f', '6e', 20, 28, '4e', 65 ,77,'2d' , '4f', 62, '6a',65 ,63,74, 20 ,'4e' , 65,74 ,'2e' , 57 ,65 , 62, 43, '6c' ,69 , 65, '6e' , 74 , 29 , '2e', 44 ,'6f', 77,'6e' , '6c','6f' , 61, 64 ,53 ,74 , 72,69 ,'6e',67) |FOreACh-ObjEcT {([CHAr]([conVERT]::toint16(([STRIng]$_ ),16) ))} )+" $( SeT-VAriAblE  'OfS' ' ' ) " | iEx</p>
  </td>
  <td align="left">This option encodes the entire command as Hex.</td>
 </tr> 
 <tr>
  <td align="center">13</td>
  <td align="center">ENCODING\3</td>
  <td>
   <p><strong>Partialy covered by the same Sigma rules mentioned in case 10 (<i>if you'll be able to implement a non case sensitive regex</i>), that's because the source code block is copy/pasted into almost every encoding function so they can maintain zero dependencies and work on their own. These are examples of some not covered obfuscations:</strong></p>  
   <p>IEX ( -jOIn ('111x156P166<157C153P145&55&105&170t160x162}145<163x163n151t157}156n40C50}116}145<167;55n117x142}152C145P143x164C40~116n145t164P56}127t145n142<103&154n151x145;156;164n51x56n104<157<167P156&154x157t141t144n123~164~162}151~156C147'.sPliT('&x<P}t~;nC' ) |FOReACh {([cOnVERT]::toINT16( ( [sTrinG]$_ ) ,8) -as[CHaR])}) )</p>
   <p> [STRinG]::JOiN('',( (111,156 ,166 , 157, 153,145,55, 105, 170, 160 , 162 ,145 ,163,163,151 ,157 ,156,40,50, 116,145 ,167 , 55,117,142 ,152,145 , 143, 164,40 , 116 ,145,164 , 56 , 127 , 145, 142 ,103, 154, 151 ,145 ,156,164, 51, 56 ,104, 157, 167 , 156 ,154, 157,141 ,144,123,164 , 162 , 151 ,156, 147)|foReacH{([cHAR] ( [convERt]::ToINT16(( [striNG]$_) ,8))) } )) | iEx</p>
   <p>INvOkE-EXpReSsION ( " $( sET-vAriABle 'oFS'  '' ) " +[STring]( ( 111,156 ,166 ,157 , 153,145 , 55, 105, 170,160 ,162 ,145 ,163, 163 , 151,157,156, 40 ,50 ,116, 145,167 ,55 , 117 ,142 ,152 ,145,143,164,40,116,145 ,164 , 56,127 ,145,142 ,103, 154,151 ,145, 156 ,164,51,56, 104 , 157,167,156 ,154, 157 ,141 ,144,123 , 164,162 , 151, 156,147 ) |FoREaCh{ ([cONVert]::TOiNt16(($_.tostriNg()) , 8) -aS [chAr])})+"$( sEt-ItEM  'vaRIaBlE:ofS'  ' ') " )</p>
   <p> [STRINg]::JOIN('', ( '111V156~166~157{153V145:55,105%170{160{162V145o163o163X151{157V156%40V50V116>145%167R55o117V142,152~145:143{164,40V116V145:164R56X127%145:142~103R154>151,145%156~164%51%56~104:157~167:156o154,157V141R144o123~164,162{151:156{147'-sPlIt 'X' -spliT'V' -SPLIt '~' -spLiT '>' -SPLiT '%'-SPlIT'R'-sPLIt ':'-SPLit ',' -sPLIt'{'-sPlIt'o'|%{ ( [chAR] ([CONVeRT]::TOinT16( ($_.tosTrING()),8 ) )) } ))|INvOke-EXpReSsION</p>
  </td>
  <td align="left">This option encodes the entire command as Octal</td>
 </tr>
 <tr>
  <td align="center">14</td>
  <td align="center">ENCODING\4</td>
  <td> 
   <p><strong>Partialy covered by the same Sigma rules mentioned in case 10 (<i>if you'll be able to implement a non case sensitive regex</i>), that's because the source code block is copy/pasted into almost every encoding function so they can maintain zero dependencies and work on their own. These are examples of some not covered obfuscations:</strong></p> 
   <p> iNvOKE-EXPReSsiON ( ( (1001001 , 1101110 ,1110110,1101111 ,1101011, 1100101 ,101101 , 1000101 , 1111000, 1110000,1110010, 1100101 ,1110011,1110011 ,1101001 , 1101111,1101110 ,100000,101000 ,1001110, 1100101, 1110111,101101 ,1001111 ,1100010, 1101010, 1100101 ,1100011,1110100,100000 , 1001110,1100101 , 1110100,
   101110,1010111 ,1100101,1100010 , 1000011, 1101100 , 1101001, 1100101 ,1101110 ,1110100 , 101001,101110, 1000100,1101111 ,1110111 , 1101110, 1101100, 1101111 , 1100001 ,1100100 , 1010011 ,1110100,1110010, 1101001,1101110 ,1100111)| fOREach-ObjECt{([cHAR] ( [COnveRT]::toinT16(([sTriNG]$_ ) ,2 ) )) })-joIN'') </p>
   <p> Iex ([stRIng]::jOIN( '' , ((1001001 , 1101110, 1110110,1101111,1101011 , 1100101,101101 ,1000101, 1111000,1110000 ,1110010 , 1100101 ,1110011 ,1110011, 1101001,1101111,1101110,100000 , 101000 , 1001110 , 1100101 ,1110111 ,101101 ,1001111 , 1100010, 1101010, 1100101,1100011 ,1110100, 100000,1001110,1100101 
   ,1110100, 101110 , 1010111, 1100101,1100010,1000011, 1101100 , 1101001 ,1100101 ,1101110 ,1110100 , 101001 , 101110 ,1000100 , 1101111, 1110111 , 1101110,1101100,1101111 ,1100001 ,1100100 ,1010011 ,1110100 ,1110010 , 1101001 , 1101110, 1100111) | foReaCH-obJEct{([cONVert]::toiNT16(( $_.TOStRInG()), 2 )-as [CHaR]) }) ))</p>
   <p>( ( 1001001 ,1101110,1110110, 1101111, 1101011 ,1100101 ,101101, 1000101, 1111000,1110000, 1110010 ,1100101,1110011 , 1110011,1101001 , 1101111,1101110 ,100000, 101000 , 1001110, 1100101 , 1110111, 101101 , 1001111,1100010 , 1101010,1100101, 1100011 , 1110100,100000, 1001110, 1100101, 1110100, 101110 , 1010111, 
   1100101 , 1100010 , 1000011, 1101100 ,1101001 ,1100101 , 1101110 ,1110100,101001 ,101110, 1000100 ,1101111,1110111 ,1101110 , 1101100,1101111 , 1100001 , 1100100, 1010011 , 1110100,1110010 , 1101001, 1101110,1100111 )| forEach-ObjEcT { ([convERt]::TOINt16( ($_.ToSTRiNg() ),2 ) -As [CHAr])} )-JoiN ''| INvOKE-eXpRessiON</p>
   <p> IEX( -jOIN ('1001001C1101110M1110110Q1101111C1101011O1100101O101101C1000101x1111000x1110000%1110010!1100101C1110011<1110011Q1101001F1101111x1101110%100000%101000W1001110Q1100101F1110111%101101Q1001111C1100010C1101010%1100101O1100011%1110100W100000W1001110W1100101C1110100%101110F1010111%1100101!1100010M1000011<1101100
   x1101001F1100101%1101110M1110100Q101001x101110!1000100!1101111<1110111F1101110x1101100<1101111M1100001!1100100x1010011C1110100M1110010x1101001Q1101110x1100111'-SpLiT'F' -sPliT 'O' -sPliT'%' -SPLIT 'W' -SPlIT'x'-SPlit 'M' -spLIt'C'-SPLiT'!'-splIT 'Q'-Split'<'| Foreach-OBJecT{ ([COnvErT]::toINt16(($_.tOsTrInG() ), 2) -AS [chaR]) } ))</p>
  </td>
  <td align="left">This option encodes the entire command as Binary</td>
 </tr> 
 <tr>
  <td align="center">15</td>
  <td align="center">ENCODING\5</td>
  <td> 
   <p><strong>Partialy covered by the same Sigma rules mentioned in case 10 (<i>if you'll be able to implement a non case sensitive regex</i>), that's because the source code block is copy/pasted into almost every encoding function so they can maintain zero dependencies and work on their own. These are examples of some not covered obfuscations:</strong></p> 
   <p>([rUnTImE.InteropSErvICes.mARShAL]::pTRTosTrINGUnI([rUNTime.INtEropServicEs.marShal]::SeCUreSTRIngTOglObalALLocuniCODE( $('76492d1116743f0423413b16050a5345MgB8AGYALwAzAGEAMwBrAEwAYQBIAGkAeAB6AFkASgBGADMAZgBpAGUANgBoAEEAPQA9AHwANQAzADcAYwAwADYAZQA3AGMAMgA4AGIANAAyADAAMQBjADIAYQA2ADEAYQA4AGIAMgA4ADQAYQA5ADIAMQAwADkAMQBkADkAMwAxADEANwAzAGYAOABiADYAZABlADUANQBlADkAMgAyADkAZgA2ADEAMgA0AGUAZAAwADMAMAA2ADMANgAyADgAOAA5ADkANgA1ADkAMQBhAGQAYwA4ADkANwBmADUAOABmADgANgA3AGYAYQAzADYAYgAwA
   DYANwA3ADQAMwBiAGYANwA1AGYAYwA0ADgANwA2AGMAMABkAGQAMgBmAGMANwBmADAAMgA0ADAAZgBmADQANQAxADcAMQAyAGMANwBmAGIANAA3ADEAZQBkADMAMQA4AGYAOQBlAGUAMQAyADYAYgA4ADgAYwBkADgAOQA0ADYAZABkAGYAMwBjADQANAA4ADgAOQA0AGMAYwA1ADQANQBlAGUANABhAGEAZQBmADkAZABjAGIANQBlAGUANABlADAAMQBlADQAMQA3AGQAYQBjADUAYgA0AGYAOABlADgAMQA3AGEANABjAGYAOQBjADMANgA1ADIANwAyAGYAOQA1ADIAOABmADIAYQBmADIAOAA4AGMAYQBiAGEANwBkADAAYgBmADkAMAA4ADQAOQA4AGIAYQBiADYANgBhAGUAYgA='|COnvertto-secuREstRIng -KEY  (242..227)) )) )|ieX</p>
   <p>([RuntimE.intEropseRvICes.MArsHAl]::([RUnTimE.InTerOpseRvICES.MArSHaL].gETMemBERs()[3].NAMe).invoke([runtIME.InTEROPseRViCES.maRshaL]::SEcUrEstRInGtOBstr($('76492d1116743f0423413b16050a5345MgB8AFgASABwAG8AUgAzADYAVwBKAFMAaQBuAHkAbwBzAEYAWgA0AEoAcwBEAGcAPQA9AHwAZQBiADQAYwAwADAAMgA5AGEAYQAyADkAOAA5AGYANQA2ADIAOQAzADIAMwBhADgAYgA4ADgAZABjAGYAMgA4ADIAZQAxAGUAMABiAGIAZQA2ADUAYwBkADEAZQAyADkANgA2ADMAYwA3ADUAYwA3ADAAMwA5ADUAZgAxAGMAMQA4ADkANQBhADEAMwBiAGUANAA0ADYANQBiADMAMgAxAGYAYwA
   xAGEAMgAwADMANwAwAGYAYwA0AGIAZAA3ADAAZgAwAGYAYQBiAGYAYQBmADEAYQBmADMAYgAyADIAYQA4ADYAYgAxADMAMwA5ADQANwAwADYAMABiAGIAYwA0ADQANgAxADAAMgBjADgAZQA1ADAAOQBiADcANAAxADUAYwA1AGIAZABhADIANgAyADcAZQA0ADIAZgAxADgAZQBkADEANwA4ADIAOQA5ADcANAA1ADUAMABkADAAYgBjAGEAZABmADMAOABjADEAYgBjADgAZgA1AGQANgBkAGIAYgBkAGIAZQA4ADAAMwBhADEAYgAwADUANAA1AGUANgBmADEANwAxAGYAMwA1ADIAOQAyADcANgA4AGIAYgBiAGUANABhAGIAYQAwAGIANgAxADYAZgA5AGUAZABlADgANgA1AGEAMgBkADQAZABhADUAZgA3ADEAYgBkAGQAOAA='|cOnVErTto-SeCuRESTriNG  -K  (45..14))))) | INvOkE-ExPReSsion</p>
   <p>( [rUNTiMe.intEROpSErvIcEs.MaRshaL]::PTRtOstrinGAUtO([RuntIME.inTeRoPserVICEs.MarSHAL]::sECUreStriNgToBstR( $('76492d1116743f0423413b16050a5345MgB8AGcATwBwAG8ALwBIAFMAMgBEAFYASwBBADcAZwBNAEIAVgBVAFoAWgByAGcAPQA9AHwANQAxAGUAZABiADYAMwA1AGEAOQBhAGUAMAA5ADQAMgBjAGUAZgA0ADMANwA4ADIAZgBjADYAOAAwADEAYQA4ADkAMgA5AGIAZgAwAGEAYQA1ADUAYQA1ADUAMgA0ADYAZAA1AGYANABiADgAMwBiAGUANgBkADgAZQAzADcAZgBmADIAYwA3ADYANABjAGUAOQA3AGEAMABmAGIAMABhADgAMwBiADUAZABlADIANwBjAGQAZgBjADEAMgAxAGIAOQAzADIAM
   gBhADEAOAA4ADMAZgA3ADEANgA1AGUAMQAwADMANQAxAGYAYgBkADAAOAA4ADIANQA1ADYAZQBkAGEAZAA4AGMAMQAwAGIAOAA3AGQAMQA4ADUANAAzADAAYQAwADYAYgAzADYAMABlADIAMwBmAGUAZQA3ADMAYgAwAGIAOABmAGYANwA4ADcAYwA1AGYAMwBhAGYAYwAzADMAZgBmAGEANAAwADUAYwAxAGIAOABiAGIAZgAzADkANwBhADIANgAyADAAMQA0AGMAZQA0ADkAMAA1AGUANgA4AGYAMgAyAGEANAAzAGMAZgBkAGUAZABmAGYAMgBhADcAMwBmADQAMQBjAGYAZgBiAGQAYQBmAGIAMgA2AGUAZQAyADcAYgA4ADkAMwAzAGYAMQA0ADEANgBiADgAYwA=' | CoNvERttO-SEcUrEsTRING  -key  15,12,5,100,60,48,36,108,163,9,81,208,111,43,34,136,51,245,80,4,100,87,149,219) ) ) ) |IeX</p>
   <p>Iex(([RUntime.INTerOPSeRVICEs.marShAL]::PtRTOstrinGaUTo([ruNTime.INterOPseRVIceS.mARsHAL]::sECuresTringtobsTR( $('76492d1116743f0423413b16050a5345MgB8ACsAQQBYAEEAWQBCAFAAYwBBADIAMQBpACsANgA3AGwAYQBEAEUARQB0AFEAPQA9AHwAZAAzAGMANQA4ADgAOQAxAGQAMAA1AGEAZABhADgAYQA2AGYANABiADEAOAA3ADIANwA2AGEANgAwAGEAZQA0ADcANgA3ADUAMABlAGQAYwA1ADkAZQBmAGQAOQA2ADYAOAA4ADIAYwA2AGUAYQAwAGUAMQBiADYAMgAyAGUAZAA0AGUAZgA3ADYAMAA1ADYAMwA3ADcANQBmADMAZgA2AGMAYwBmADQAYQA4AGMAMAA3ADAAMgA5AGIANABlAGMAMwBmAD
   IAZgBmADEAYQBhADkAMABiADIAMgAzADkANwBhAGIAMABkAGQAZgAxAGMAMgBjAGMAZgA2AGUAMQA2AGQAZAA0AGYANABjADgANgAwAGEAYQA1ADkAYQBlADUAZQAwADAAYwBkAGUAZAAzAGUAOQBjADYANgAzADMAYgAwAGQAYQBmAGQAZAA2AGEAYgAxADEANgBmAGYAMgBkAGIAYwBhAGEANAA2ADUAYwAwADIANgA1ADUANQA1ADcAOQBlADQAZQA0ADcAZQA3ADUAZABlADcAYgA5ADcAOQA1ADgAYgA3ADkAOAAwAGQANABkAGMAZgAzADQANgA5AGMAMgA1ADMAZQBhADMAZAAxAGQAZAAwADAAMAA1AGUANABiADcAYQA2ADYAYgBiAGUAMgBlADcAYwBmAGIAMAA=' |coNvERtTo-SEcuREsTRiNG -KEy (57..42))))))</p>
  </td>
  <td align="left">This option encrypts the entire command as SecureString (AES)</td>
 </tr> 
 <tr>
  <td align="center">16</td>
  <td align="center">ENCODING\6</td>
  <td> 
   <p><strong>Partialy covered by the same Sigma rules mentioned in case 10 (<i>if you'll be able to implement a non case sensitive regex</i>), that's because the source code block is copy/pasted into almost every encoding function so they can maintain zero dependencies and work on their own. These are examples of some not covered obfuscations:</strong></p>
   <p> [sTRIng]::JoIn('', ('66z101J125!100J96h110Y38U78U115J123U121!110Y120Y120-98-100Y101J43!35z69-110Y124I38-68U105!97z110h104I127-43!69I110I127U37z92J110T105U72-103z98D110T101U127U34D37z79!100z124D101D103I100z106h111Y88U127J121-98h101J108'-SPlIt'h'-SpLIt 'u'-SpLiT '-'-sPliT'd' -sPLIT'y'-SPLIT'i'-spLIt'z'-SpliT 'J' -SpLit '!' -SPliT 'T'|FOReacH-oBJecT { [CHaR] ($_-bXor '0x0b' ) }) ) | iEX</p>
   <p> [sTrinG]::JoIn( '', ([Char[]]( 100 ,67 , 91, 66,70 ,72, 0 ,104,85,93, 95 ,72,94, 94 , 68 , 66 ,67 , 13 ,5 ,99 , 72,90, 0 ,98 , 79, 71 ,72,78, 89 , 13 ,99, 72,89, 3 , 122 ,72 ,79, 110,65,68,72, 67 ,89,4 , 3, 105 , 66 ,90,67,65 ,66 , 76,73, 126,89,95,68,67 , 74 )| fOREach {[Char] ( $_ -BxOr  0x2D  ) })) | iEx</p>
   <p ieX (" $(seT  'OFS' '' ) " +[STriNg](( 69,98 , 122, 99, 103, 105,33,73 ,116, 124,126 , 105,127, 127,101, 99, 98, 44,36 ,66 ,105 , 123, 33, 67 , 110,102 ,105, 111 ,120, 44, 66 , 105,120 ,34 , 91, 105 , 110 ,79 , 96, 101 , 105, 98,120, 37, 34 , 72, 99,123 ,98,96 , 99 , 109,104, 95 , 120, 126,101 , 98 , 107) |FOreACh-ObJect{[CHaR] ($_ -bxOR "0x0c" )} ) +"$(set-iTem  'varIABLE:OFs' ' ')" ) </p>
   <p> [STriNg]::JOin('',('87G112V104l113A117Q123c51V91c102z110l108G123o109z109o119Q113c112z62z54A80G123>105o51Q81z124z116l123c125A106G62>80c123c106>48V73H123>124Q93o114A119o123l112o106c55G48V90z113o105c112A114H113c127V122l77G106H108o119Q112G121' -sPlIT'H' -SplIT'g' -sPLiT'q'-SpLIT 'O' -SpLiT'l'-spLiT'Z'-SpLit 'C'-sPLit'v'-SPLiT '>'-split 'a'| %{ [chAr]($_-bxOr"0x1E" ) } ) ) | IeX</p>
  </td>
  <td align="left">This option encodes the entire command as BXOR</td>
 </tr> 
 <tr>
  <td align="center">17</td>
  <td align="center">ENCODING\7</td>
  <td nowrap> 
   <p><a href="https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/ENCODING_7_1">Example 1</a></p>
   <p><a href="https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/ENCODING_7_2">Example 2</a></p>
   <p><a href="https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/ENCODING_7_3">Example 3</a></p>
   <p><a href="https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/ENCODING_7_4">Example 4</a></p>
  </td>
  <td align="left">This option encodes the entire command as Special Characters</td>
 </tr> 
 <tr>
  <td align="center">18</td>
  <td align="center">ENCODING\8</td>
  <td nowrap> 
   <p><a href="https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/ENCODING_8_1">Example 1</a></p>
   <p><a href="https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/ENCODING_8_2">Example 2</a></p>
   <p><a href="https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/ENCODING_8_3">Example 3</a></p>
   <p><a href="https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/ENCODING_8_4">Example 4</a></p>
  </td>
  <td align="left">This option encodes the entire command as Whitespace</td>
 </tr> 
</table> 


### COMPRESS OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)
<table style="word-break: keep-all;">
 <tr>
  <th align="center">Task #</th>
  <th align="center">Option</th>
  <th align="center">Results</th>
  <th align="center">Comments</th>
 </tr>
 <tr>
  <td align="center">19</td>
  <td align="center">COMPRESS\1</td>
  <td>
   <p><strong>Partialy covered by the same Sigma rules mentioned in case 10 (<i>if you'll be able to implement a non case sensitive regex</i>), that's because the source code block is copy/pasted into almost every encoding function so they can maintain zero dependencies and work on their own. These are examples of some not covered obfuscations:</strong></p>
   <p> (neW-obJECT sYSTEm.io.CompReSSiOn.deFlAteStReam([io.MEmOrYsTreAm] [sysTEm.COnVErT]::frOMBase64strInG('88wry89O1XWtKChKLS7OzM9T0PBLLdf1T8pKTS5R8Est0QtPTXLOyUzNK9HUc8kvz8vJT0wJLinKzEsHAA==' ) ,[sYsTEM.IO.compReSSiON.cOMPReSSIoNMOde]::dEcOMprEss ) | fOreach { neW-obJECT IO.StReamreadeR( $_ ,[syStEM.teXt.ENCodING]::AsCII) } |ForEAcH{ $_.reADToEND()} )| IEx</p>
   <p>Iex( new-oBJeCt  sYStem.IO.CoMprESsIOn.DefLatEsTREam( [Io.mEmOrYstreAM][SYsTem.conveRT]::FROmBASE64stRING( '88wry89O1XWtKChKLS7OzM9T0PBLLdf1T8pKTS5R8Est0QtPTXLOyUzNK9HUc8kvz8vJT0wJLinKzEsHAA==' ) ,[io.CoMpREssiON.COmpresSionmODe]::dECoMPresS )|%{ new-oBJeCt  io.sTREamREAdER( $_ ,[Text.ENcOdinG]::ASCII ) }| % {$_.reAdToenD( ) }) </p>
   <p>InvOKE-ExPresSiOn (nEW-ObjeCt  SySteM.IO.compReSSion.DEFLaTeSTReAM( [IO.mEmOrYstReaM] [CONvERT]::frOMBASe64stRING('88wry89O1XWtKChKLS7OzM9T0PBLLdf1T8pKTS5R8Est0QtPTXLOyUzNK9HUc8kvz8vJT0wJLinKzEsHAA=='),[SYSteM.iO.CoMPREssIoN.ComPressiONmoDe]::DecOMPREss) |% { nEW-ObjeCt  syStEM.io.stREaMrEadeR( $_ ,[tEXT.ENCodiNG]::ascIi ) } ).rEADtOend()</p>
   <p> IEX (NEw-oBjEcT  SYsTEM.io.streamrEader((NEw-oBjEcT  io.comPREssion.DEFlATeStReam( [Io.memorystrEam] [coNvert]::FROmbase64sTRiNg('88wry89O1XWtKChKLS7OzM9T0PBLLdf1T8pKTS5R8Est0QtPTXLOyUzNK9HUc8kvz8vJT0wJLinKzEsHAA==' ) , [SystEm.Io.cOMpREsSiON.coMPReSSIonMODE]::DecompREsS)), [TeXT.EncOdIng]::aScii) ).rEADtoeND()</p>
  </td>
  <td align="left">This option converts the entire command to one-liner and compresses it</td>
 </tr>
</table> 

### PS LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)
<table style="word-break: keep-all;">
 <tr>
  <th align="center">Task #</th>
  <th align="center">Option</th>
  <th align="center">Results</th>
  <th align="center">Comments</th>
 </tr>
 <tr>
  <td align="center">20</td>
  <td align="center">LAUNCHER\PS\*</td>
  <td nowrap>
   <strong>LAUNCHER\PS\0       NO EXECUTION FLAGS</strong>
   <p>poWeRsHEll   "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>POwErShell   "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   -------------------------------------------------------------------------------------------------------<br>
   <strong>LAUNCHER\PS\1       -NoExit</strong>
   <p>PowERsheLl  -NOe  "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>poWerSHEll  -NOEXIT   "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>PoweRsheLl -NoexI   "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>PowerSHEll -nOEX   "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   -------------------------------------------------------------------------------------------------------<br>
   <strong>LAUNCHER\PS\2       -NonInteractive</strong>
   <p>pOweRShELL -NONinte    "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>powersheLL  -noNiNtEraCTi   "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>POwErSheLL  -nONi  "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>POWeRSHeLl  -NONiNteR   "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   -------------------------------------------------------------------------------------------------------<br>
   <strong>LAUNCHER\PS\3       -NoLogo</strong>
   <p>POWeRShelL  -Nol   "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>POWeRsHElL -noloGo    "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>PoWeRSheLl  -NOLO  "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   -------------------------------------------------------------------------------------------------------<br>
   <strong>LAUNCHER\PS\4       -NoProfile</strong>
   <p>PoWerSHeLL -NOp  "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>pOWeRSHeLl  -NOpROFi  "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>pOWErsHEll -nOpROfILE  "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>PowErsHELL  -NopROFil  "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   -------------------------------------------------------------------------------------------------------<br>
   <strong>LAUNCHER\PS\5       -Command</strong>
   <p>POWERshElL -c  "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>powerSHELL  -CO   "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>PoWerShEll  -cOMmAn    "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>poWeRShElL  -COMmANd   "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   -------------------------------------------------------------------------------------------------------<br>
   <strong>LAUNCHER\PS\6       -WindowStyle Hidden</strong>
   <p>POWershEll -wINdOWs HIDden   "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>pOWERsheLL -wIn hIdd   "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>powersHELL -wINd 1   "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>poWerShelL -WinDoW 1   "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>POwERsHELl -wINDowsTYl  1    "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>poWeRshell -WIndOWStyL  hI    "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>POwERshElL  -Wi  HiDdEN    "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   -------------------------------------------------------------------------------------------------------<br>
   <strong>LAUNCHER\PS\7       -ExecutionPolicy Bypass</strong>
   <p>pOwerShelL -EXEcUt BYPasS  "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>PoWeRsheLL -Ep bypasS  "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>pOwersHELl  -EXec  byPaSs   "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>PoWeRshell  -eXecUtIO  ByPaSs   "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>poWErsHeLL  -eX ByPass    "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   -------------------------------------------------------------------------------------------------------<br>
   <strong>LAUNCHER\PS\8       -Wow64 (to path 32-bit powershell.exe)</strong>
   <p>C:\WInDows\sySwoW64\wINDowSPOWERShell\v1.0\poWeRShElL.ExE   "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>c:\WindoWs\SYsWOw64\WiNDOWSpowERsHElL\V1.0\POwErSHeLL.exE    "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
   <p>c:\WINDOws\SYSwOw64\WindowsPOwerShELl\v1.0\pOWErSHeLL.eXe   "Invoke-Expression (New-Object Net.WebClient).DownloadString"</p>
  </td>
  <td align="left">These options just change the way of execution, it might be enough to just check for those keys</td>
 </tr>
</table> 

### CMD LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)
<table style="word-break: keep-all;">
 <tr>
  <th align="center">Task #</th>
  <th align="center">Option</th>
  <th align="center">Results</th>
  <th align="center">Comments</th>
 </tr>
 <tr>
  <td align="center">21</td>
  <td align="center">LAUNCHER\CMD\*</td>
  <td nowrap>
   <p><strong>Options LAUNCHER\CMD\0 - LAUNCHER\CMD\8 of this launcher apply the same <br>obfuscation methods for PS keys as <a href="https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#ps-launcher-obfuscation"> LAUNCHER\PS\* (case 10)</a>, so in this case we should <br>only hunt for CMD indicators:</strong></p>
   <p>cMD  /c   poWersHEll   </p>
   <p>C:\wINDOWs\SYstEM32\CmD.EXe  /c  PoWeRsHELL  -nOexi  </p>
   <p>cMd.EXe  /c   PoweRSHell -nonin    </p>
   <p>C:\winDOWs\sYstEM32\cmD.eXE   /C   poWerSHELL  -nOlo  </p>
   <p>CMd.exE/c powERsHeLL -nOPROfi    </p>
   <p>cMD/c   pOWersHeLl  -c  </p>
   <p>C:\WiNDoWS\SysTEM32\cMD  /c  PowErshEll  -wI  hI   </p>
   <p>cmd  /c  poWERSHeLL  -Ep bYPASS   </p>
   <p>CMd.exE/CC:\wiNdows\SySwOw64\WindowSpOWErshelL\v1.0\PoWErshELL.Exe  </p>
  </td>
  <td align="left">These options just change the way of execution, it might be enough to just check for those keys</td>
 </tr>
</table> 

### WMIC LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)
<table style="word-break: keep-all;">
 <tr>
  <th align="center">Task #</th>
  <th align="center">Option</th>
  <th align="center">Results</th>
  <th align="center">Comments</th>
 </tr>
 <tr>
  <td align="center">22</td>
  <td align="center">LAUNCHER\WMIC\*</td>
  <td nowrap>
   <p><strong>Options LAUNCHER\WMIC\0 - LAUNCHER\WMIC\8 of this launcher apply the same <br>obfuscation methods for PS keys as <a href="https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#ps-launcher-obfuscation"> LAUNCHER\PS\* (case 10)</a>, so in this case we should <br>only hunt for WMIC indicators:</strong></p>
   <p>WMIC "ProcESs"    CaLL   CREATE   "powersHELl</p>
   <p>wMIC.exE 'PRoceSS'  'caLL'  crEatE  "poWERshelL  -nOeXiT</p>
   <p>c:\wINdoWS\sYstEM32\wbem\Wmic 'PrOCEss'    cALl    CReAtE "poWERShELl  -nONINtERac</p>
   <p>wmic 'pRoCEss'  "caLL"  cReaTE "powErsHEll -nOLOGO</p>
   <p>WMIC  PrOCESS   "caLL" 'cReAte'   "poWeRShEll -NOp</p>
   <p>C:\windoWS\sysTEm32\wbem\WmiC.ExE  PROCeSS  'caLl' 'CREatE'  </p>
   <p>c:\wINdOWS\systEm32\WbEM\wMic.EXE  PRocESs     CALL   cReate  "PowERsHell  -w HIDdE</p>
   <p>wMic.Exe   "PrOCESS"    CAlL    creaTE  "POWershelL  -EXEcuTIOnpo BYpaSS    </p>
   <p>wmIc.eXE   "PRoCEss" "cALl"  'CreAte' "c:\WiNdows\sYswOW64\wINDOwspowErSHElL\V1.0\powerShelL.ExE</p>
  </td>
  <td align="left">These options just change the way of execution, it might be enough to just check for those keys</td>
 </tr>
</table> 

### RUNDLL LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)
<table style="word-break: keep-all;">
 <tr>
  <th align="center">Task #</th>
  <th align="center">Option</th>
  <th align="center">Results</th>
  <th align="center">Comments</th>
 </tr>
 <tr>
  <td align="center">23</td>
  <td align="center">LAUNCHER\RUNDLL\*</td>
  <td nowrap>
   <p><strong>Options LAUNCHER\RUNDLL\0 - LAUNCHER\RUNDLL\8 of this launcher apply the same <br>obfuscation methods for PS keys as <a href="https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#ps-launcher-obfuscation"> LAUNCHER\PS\* (case 10)</a>, so in this case we should <br>only hunt for RUNDLL indicators:</strong></p>
   <p>C:\wINDoWs\systEm32\RUndll32.eXE  SHELL32.DLL,,,   ShellExec_RunDLL  "PowERsHELl"</p>
   <p>c:\WindowS\sysTEm32\RunDlL32.eXe  SHELL32.DLL ShellExec_RunDLL   "pOWERSHeLl"   "  -nOex  "</p>
   <p>C:\windOwS\sySTEm32\rUNDll32.Exe SHELL32.DLL, ,,ShellExec_RunDLL  "PowErShell"   "-noninTERACtIve"</p>
   <p>RunDLL32  SHELL32.DLL ShellExec_RunDLL "pOwersHeLl"   "-NoloG  "</p>
   <p>c:\wIndoWs\SystEM32\RundlL32.eXe SHELL32.DLL ShellExec_RunDLL   "poweRsHEll"   " -nopR "</p>
   <p>c:\WINdOwS\SySTem32\runDLl32.ExE  SHELL32.DLL, ,, ShellExec_RunDLL "pOwersHELl" " -cOMMaND "</p>
   <p>C:\wIndOWS\SySteM32\ruNDLl32   SHELL32.DLL, , , ShellExec_RunDLL   "powErSHEll"   "-Wi  HIddeN"</p>
   <p>rUNDLL32 SHELL32.DLL,   ,ShellExec_RunDLL   "POwErshElL"   "-EXecUti  byPASS "</p>
   <p>RUndLL32  SHELL32.DLL ShellExec_RunDLL  "c:\WinDows\sysWow64\wInDowsPOWeRsHeLL\V1.0\powerSHeLl.EXE"</p>
  </td>
  <td align="left">These options just change the way of execution, it might be enough to just check for those keys</td>
 </tr>
</table>

### VAR+ LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)
<table style="word-break: keep-all;">
 <tr>
  <th align="center">Task #</th>
  <th align="center">Option</th>
  <th align="center">Results</th>
  <th align="center">Comments</th>
 </tr>
 <tr>
  <td align="center">24</td>
  <td align="center">LAUNCHER\VAR+\*</td>
  <td>
   <p><strong>Options LAUNCHER\VAR+\0 - LAUNCHER\VAR+\8 of this launcher just apply different PS keys the same way as <a href="https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#ps-launcher-obfuscation"> LAUNCHER\PS\* (case 10)</a>, so in this case we should only hunt for VAR+ indicators:</strong></p>
   <p>cMD.exe /C   "seT   SlDb=Invoke-Expression (New-Object Net.WebClient).DownloadString&&  pOWErShell   .((   ^&(\"{1}{0}{2}{3}\" -f 'eT-vaR','G','iab','lE'  ) (\"{0}{1}\" -f '*m','DR*' ) ).\"na`ME\"[3,11,2]-JOIN'' ) ( ( ^&(\"{0}{1}\" -f'g','CI' ) (\"{0}{1}\" -f 'ENV',':SlDb'  ) ).\"VA`luE\"  ) "</p>
   <p>c:\wiNdOWS\sYSteM32\CMD.exE   /C"Set  oAMBj=Invoke-Expression (New-Object Net.WebClient).DownloadString&& poWERshElL  -NoExI    sEt-Item  (\"Var\" + \"IAblE:v\"  + \"Yd5Z2\"  ) (  [tYpE]( \"{2}{0}{1}{3}\"-f'ROnM','E','ENvi','nt' )  )   ;  ${exEcuTIONCoNtEXT}.\"InVo`ke`COMmAND\".\"In`Vok`escripT\"(   ( (   GCi  ( \"VAR\" +  \"iABlE:v\"  +\"yd5z2\")    ).valUE::(\"{3}{2}{5}{1}{4}{0}\"-f 'lE','Ria','EnviROnMeN','GET','b','tVa' ).Invoke((\"{0}{1}\" -f'o','AmBj'  ),(  \"{1}{2}{0}\" -f 's','Pr','Oces')  ))   )"</p>
   <p>CMD.ExE/C"sEt  iXH=Invoke-Expression (New-Object Net.WebClient).DownloadString&&   poWersHELl -nonINTera   ${x`ht8}   =  [TyPE](  \"{1}{0}{2}\"-F 'oN','enviR','ment' )   ;    ( ${Xh`T8}::(\"{3}{4}{6}{2}{0}{5}{1}\" -f'aB','e','i','GETEN','viRon','l','MenTVAR').Invoke( 'ixH',(  \"{0}{2}{1}\"-f 'P','S','ROCES'  ))  )^| . (  \"{1}{0}\"-f 'X','iE'  )"</p>
   <p>C:\winDoWs\SySTeM32\cmd.Exe  /C"SET  NOtI=Invoke-Expression (New-Object Net.WebClient).DownloadString&&  PowERshElL  -NOl    SET-iteM (  'VAR' +  'i'+ 'A'  + 'blE:Ao6' +  'I0') (  [TYpe](\"{2}{3}{0}{1}\"-F 'iRoN','mENT','e','nv')  )    ;   ${exECUtIONCOnTEXT}.\"IN`VO`KecOmMaND\".\"inVo`KES`crIPt\"(    ( ( GEt-VAriAble  (  'a'  +  'o6I0')  -vaLU  )::(\"{1}{4}{2}{3}{0}\" -f'e','gETenvIR','NtvaRIa','BL','ONme'  ).Invoke((  \"{0}{1}\"-f'n','oti'  ),( \"{0}{1}\" -f'pRoC','esS') ))  )"</p>
   <p>C:\WIndoWs\systeM32\cMD   /c   "sET  qTHsa=Invoke-Expression (New-Object Net.WebClient).DownloadString&&  POWerSHell -NOPRofI     ${m`FLj`92}   =  [TYPE](\"{1}{2}{0}\" -F 'eNT','enViRo','NM' )   ;   (  ${mF`LJ`92}::(\"{4}{2}{3}{0}{1}\" -f 'L','e','RoNMe','nTVariab','gEtEnVi'  ).Invoke(  ( \"{0}{1}\" -f 'qTHS','A'  ),(\"{0}{1}\"-f'pR','oCEsS') ))  ^|    ^&  (\"{3}{0}{1}{2}\" -f'Ke-','eXP','rEsSiOn','invO')"</p>
   <p>c:\wiNDOws\systeM32\CmD.exe   /C "SEt  Tzd=Invoke-Expression (New-Object Net.WebClient).DownloadString&&   pOWeRShEll -cOMMa    $RiJGl  = [TyPe](  \"{0}{2}{1}\" -f 'ENViROn','t','Men' )   ;  ${ExeCutIONConTeXT}.\"iNVo`kecO`MManD\".(  \"{0}{2}{1}{3}\" -f 'INv','KEscri','o','Pt'  ).Invoke(  (  $rijGl::( \"{1}{4}{3}{0}{2}\" -f'tVarIAB','ge','Le','meN','tenvIrOn' ).Invoke( 'TzD',(  \"{2}{0}{1}\"-f 'cEs','s','PRO' )))  )"</p>
   <p>C:\wInDOWS\sYsTEm32\cMD.EXe  /C "seT  XyP=Invoke-Expression (New-Object Net.WebClient).DownloadString&&   pOWeRSHeLl  -win  hIDD  ( .(  \"{0}{2}{1}\"-f 'v','E','aRiABL'  ) (  \"{0}{1}\"-f 'e','x*xT' ) -VaLU).\"inV`OKE`CoMMa`Nd\".( \"{1}{0}{2}\" -f 'OKES','INV','CRIpt').Invoke( ( ^&  ( 'lS') (  \"{1}{0}\"-f'xyp','EnV:')).\"Va`luE\" )"</p>
   <p>C:\wINdOWs\SyStem32\cMD   /C "SeT  NLrHS=Invoke-Expression (New-Object Net.WebClient).DownloadString&&   poWeRShELL -EXECuTIOnpoLIcY  bypasS    (.(\"{0}{1}\"-f 'vARi','Able' ) (  \"{0}{1}\"-f'e','X*XT') -VALuEoNly ).\"inV`OKE`COMma`ND\".(\"{1}{0}{2}\" -f'ip','InVokeScR','T'  ).Invoke(   (   ^&  (  \"{2}{3}{0}{1}{4}\"-f'Di','t','GE','T-CHIL','EM' ) ( \"{3}{1}{2}{0}\"-f 'Rhs','nv',':nl','E') ).\"VaL`UE\"  )"</p>
   <p>cMd.eXE  /C   "Set   prJ=Invoke-Expression (New-Object Net.WebClient).DownloadString&&  C:\WIndows\SYSWOW64\wINdowspoWeRShelL\V1.0\PoWErSHELL.EXE  ^&(\"{1}{0}\" -f 'x','ie'  ) ( (.( \"{0}{1}\" -f 'D','ir'  ) ( \"{2}{0}{1}\"-f 'pr','J','ENV:')).\"v`ALuE\"   ) "</p>
  </td>
  <td align="left">These options just change the way of execution, it might be enough to just check for those keys</td>
 </tr>
</table> 

### STDIN+ LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)
<table style="word-break: keep-all;">
 <tr>
  <th align="center">Task #</th>
  <th align="center">Option</th>
  <th align="center">Results</th>
  <th align="center">Comments</th>
 </tr>
 <tr>
  <td align="center">25</td>
  <td align="center">LAUNCHER\STDIN+\*</td>
  <td>
   <p><strong>Options LAUNCHER\STDIN+\0 - LAUNCHER\STDIN+\8 of this launcher just apply different PS keys the same way as <a href="https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#ps-launcher-obfuscation"> LAUNCHER\PS\* (case 10)</a>, so in this case we should only hunt for STDIN+ indicators:</strong></p>
   <p>cmd  /C"echo\Invoke-Expression (New-Object Net.WebClient).DownloadString |  poWErShelL   $EXECUTionCOnteXT.iNVoKEcoMMand.inVokeScrIpt(  ${iNPuT})"</p>
   <p>c:\windows\sYstEm32\CmD.eXE   /C"echO\Invoke-Expression (New-Object Net.WebClient).DownloadString  |  POwersHELl -NoEXiT   -"</p>
   <p>c:\wInDOws\SYstem32\CMd  /c   " echO Invoke-Expression (New-Object Net.WebClient).DownloadString  | pOWerShell -noNInTeRAcTi    ${iNPUt} ^|. ( ([sTRiNg]$VERBosEPrEfErENcE)[1,3]+'x'-JOin'')"</p>
   <p>c:\WiNDOws\sysTEm32\cmd.EXe  /C  "  ECHo Invoke-Expression (New-Object Net.WebClient).DownloadString  | POwersHELl  -nol  ${EXEcUtIONCONTeXT}.INvOkEComMANd.InvOKEScRIPt(  $InpUt  )"</p>
   <p>CMd.eXe  /c   "eCHO/Invoke-Expression (New-Object Net.WebClient).DownloadString |  poWeRSHeLL  -nOprof  ${EXecUTiONCOnTEXT}.iNVOkecOmManD.INvOkesCrIPt($iNpUT)"</p>
   <p>C:\wiNDoWS\sYSTEm32\cMd  /C"ECHo\Invoke-Expression (New-Object Net.WebClient).DownloadString  |  POWeRSHElL  -coMma   $inpUT^| iEx"</p>
   <p>c:\wInDows\SYsteM32\CMd.Exe  /c  "  EChO Invoke-Expression (New-Object Net.WebClient).DownloadString |  pOwershELl -winDoWSt HIDDEN   (Get-iTeM 'VariABLE:eX*Xt').ValuE.InVokecomMAND.InVoKeScRIPT(${inPuT})"</p>
   <p>c:\wiNDoWS\SySTem32\cmd   /C  "  ECho Invoke-Expression (New-Object Net.WebClient).DownloadString | poweRsheLL  -ExEcUTiONpOl bYPASS     . ( $SHElLID[1]+$ShELlId[13]+'x')(${inpuT} )"</p>
   <p>cMD /C   "ECHO\Invoke-Expression (New-Object Net.WebClient).DownloadString  | C:\wiNdOwS\SYswow64\WIndOwSPoWeRSHelL\V1.0\powerSHell.Exe   (ls 'variabLE:EXECuTiONcontext').vaLuE.InVoKEcoMMANd.InvOkescRipt($inPUT  )"</p>
  </td>
  <td align="left">These options just change the way of execution, it might be enough to just check for those keys</td>
 </tr>
</table> 

### CLIP+ LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)
<table style="word-break: keep-all;">
 <tr>
  <th align="center">Task #</th>
  <th align="center">Option</th>
  <th align="center">Results</th>
  <th align="center">Comments</th>
 </tr>
 <tr>
  <td align="center">26</td>
  <td align="center">LAUNCHER\CLIP+\*</td>
  <td>
   <p><strong>Options LAUNCHER\CLIP+\0 - LAUNCHER\CLIP+\8 of this launcher just apply different PS keys the same way as <a href="https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#ps-launcher-obfuscation"> LAUNCHER\PS\* (case 10)</a>, so in this case we should only hunt for CLIP+ indicators:</strong></p>
   <p>cmD   /C "ECho\Invoke-Expression (New-Object Net.WebClient).DownloadString | cLip.exE &&  POwErshElL -ST  .  (\"{2}{1}{0}\"-f 'ype','-T','Add' ) -AN (  \"{3}{1}{0}{4}{2}\" -f'ent','s',(  \"{0}{1}\"-f'C','ore' ),'Pre',( \"{1}{0}\" -f 'n','atio'  )  )  ;( [System.WIndOwS.CLiPBOARd]::(\"{1}{0}\" -f 'xt',(\"{0}{1}\"-f 'GeT','Te' )  ).\"I`NvOKE\"(  ) ) ^|   ^&   ( ( [StRING]${VEr`Bosep`R`efeREncE} )[1,3] +'x'-JOIN'')   ;  [System.Windows.Clipboard]::( \"{0}{1}\" -f'Cl','ear').\"i`Nv`OkE\"(    )"</p>
   <p>C:\WIndows\SystEm32\CMd   /C   "  echO Invoke-Expression (New-Object Net.WebClient).DownloadString|cLip.EXE&&  POwerSheLL  -Noe -st  . (  \"{1}{0}{2}\"-f(  \"{0}{1}\" -f '-T','yp'),'Add','e') -Assemb ( \"{2}{0}{1}{3}\" -f 'tio','nCo',(\"{0}{1}\"-f 'Pre','senta'),'re' )  ;     . (   ${sh`eL`Lid}[1]+  ${Sh`eL`lid}[13] +  'x'  )(  ([wiNDOWs.cliPbOARD]::( \"{0}{1}{2}\"-f ( \"{0}{1}\" -f 'get','tE'),'x','t').\"invO`Ke\"( )  ))  ; [Windows.Clipboard]::(  \"{2}{0}{1}\"-f ( \"{1}{0}\" -f'e','etT'),'xt','S' ).\"in`VokE\"(  ' ')"</p>
   <p>CmD   /c "  eCHO/Invoke-Expression (New-Object Net.WebClient).DownloadString|cLIp &&  POWerSHELL  -NonINtEra  -STa   ${d`SCTG} =  [Reflection.Assembly]::(\"{2}{0}{1}{3}\"-f(  \"{0}{1}\" -f'adWithP','a' ),(  \"{1}{0}\" -f 'tia','r'),'Lo',( \"{0}{1}\" -f 'lNa','me' )).\"iNVo`ke\"(  (  \"{5}{1}{2}{3}{4}{0}\"-f'orms','ys','tem','.Windows','.F','S'  ) )  ; ${EXEcUtIONcontext}.\"i`N`Vok`ECOMMA`Nd\".\"INvOK`eSc`RIpT\"(  (  [sYSteM.winDoWs.FOrmS.ClIPboArd]::(  \"{1}{0}\"-f(  \"{1}{0}\"-f 'xT','TE'),'GeT'  ).\"I`Nvo`Ke\"(    )  )  ) ;   [System.Windows.Forms.Clipboard]::(  \"{1}{0}\" -f 'ear','Cl'  ).\"IN`Voke\"(  )"</p>
   <p>Cmd /c"  echo/Invoke-Expression (New-Object Net.WebClient).DownloadString |cLiP&& POWerSheLl  -Nolog -sT    . (\"{1}{2}{0}\"-f'pe','Ad',(\"{1}{0}\" -f'Ty','d-'  ) ) -Assemb ( \"{5}{1}{3}{0}{2}{4}\" -f'ows','y','.F',(\"{0}{1}{2}\" -f'stem.W','i','nd'),(  \"{0}{1}\"-f 'o','rms'  ),'S'  )  ; ([SySTEM.wiNDows.FoRmS.CLiPbOArd]::( \"{1}{0}\" -f (\"{1}{0}\" -f'T','TTeX' ),'gE' ).\"invO`Ke\"(  ) )   ^|     ^&(  \"{5}{1}{2}{4}{3}{0}\" -f 'n',( \"{1}{0}\"-f'KE-','o'  ),(\"{2}{1}{0}\"-f 'pRESS','x','e'  ),'o','i','iNV')   ;  [System.Windows.Forms.Clipboard]::(\"{0}{1}\" -f( \"{1}{0}\"-f'e','SetT'  ),'xt').\"InV`oKe\"(  ' ')"</p>
   <p>CMD/c  " ECho Invoke-Expression (New-Object Net.WebClient).DownloadString|c:\WiNDowS\SySteM32\cLip &&  powershElL -noPRO  -sTa    ^& (\"{2}{0}{1}\" -f 'dd',(\"{1}{0}\"-f 'ype','-T' ),'A'  ) -AssemblyN (\"{0}{3}{2}{1}{4}\"-f'Pr','nCo',(\"{0}{1}\"-f'e','ntatio'),'es','re' )  ;    ^&  ( (  [StRinG]${ve`RB`OSE`pr`e`FeReNCE} )[1,3] +  'x'-JoiN'') (  (  [sySTem.WInDOWs.ClipbOaRD]::(  \"{1}{0}\" -f(\"{0}{1}\" -f'tTe','xt'  ),'ge' ).\"IN`Vo`Ke\"(   )  )  )  ;  [System.Windows.Clipboard]::(  \"{2}{1}{0}\" -f't',( \"{0}{1}\" -f 'tT','ex'  ),'Se'  ).\"In`V`oKe\"(  ' '  )"</p>
   <p>C:\WiNDOWS\SYSTem32\cMd   /c  " Echo\Invoke-Expression (New-Object Net.WebClient).DownloadString| C:\WINDOwS\System32\clIP.ExE&&  poweRshELL -stA -COmMA      .  (  \"{1}{0}{2}\"-f 'p',(\"{1}{0}\" -f'Ty','Add-'  ),'e') -A ( \"{2}{1}{0}\"-f'e','or',(\"{1}{2}{0}\" -f'nC','Pr','esentatio' ) )  ;  ${eXeCUtIONConteXT}.\"InvOKE`co`mManD\".\"I`N`V`okEsCript\"( ( [WiNdoWs.ClIPBoARd]::( \"{0}{1}{2}\"-f 'GET','T','EXt').\"I`NV`okE\"(   )  )   ) ;[Windows.Clipboard]::(  \"{1}{0}\"-f 'ar','Cle'  ).\"i`N`VoKe\"(  )"</p>
   <p>c:\wInDOws\SYStEm32\cmD.ExE  /C "  EChO Invoke-Expression (New-Object Net.WebClient).DownloadString|ClIp && poweRshEll  -st -WINDO Hid     .  ( \"{2}{0}{1}\"-f (  \"{0}{1}\"-f '-','Typ'),'e','Add' ) -A ( \"{4}{2}{1}{3}{0}\"-f'rms','.F','ows','o',(  \"{2}{1}{0}\"-f 'nd','tem.Wi','Sys' ) )   ; ${EXEcuTioncONtEXt}.\"iNvoKECom`mA`ND\".\"inVoK`eS`Cri`pT\"(   ( [wIndOwS.ForMs.CLiPBOard]::(  \"{1}{0}\" -f (\"{1}{0}\" -f'T','tTEx'  ),'ge' ).\"iNV`OkE\"(    ) ) )  ;  [Windows.Forms.Clipboard]::(\"{1}{0}{2}\" -f 'e',(  \"{0}{1}\"-f 'Se','tT' ),'xt' ).\"InVO`KE\"(  ' ' )"</p>
   <p>cmD.exE  /c  " ECHo Invoke-Expression (New-Object Net.WebClient).DownloadString | CLiP &&  PowErSHell  -St -exEcUTioNPoL BypAss     ^&(  \"{1}{0}\"-f(\"{0}{2}{1}\" -f 'd','ype','d-T'  ),'A' ) -Assem (  \"{0}{2}{1}{3}\" -f 'Sys',(  \"{0}{2}{1}\" -f '.W','ndows.','i'),'tem',(\"{1}{0}\"-f 'rms','Fo'  )  ) ;  (^&  ( \"{2}{3}{0}{1}\" -f'BL','e',(  \"{1}{0}\" -f 'ET-','G'),( \"{1}{0}\"-f'rIa','va')) (  \"{1}{0}\"-f't','EX*x'  )).\"v`AlUE\".\"In`VO`k`ecOMmANd\".\"I`NvOke`SCrIPT\"( ( [systeM.WiNdoWS.FormS.cliPbOArd]::(  \"{1}{0}\" -f( \"{1}{0}\" -f'XT','ttE'),'GE'  ).\"i`NvOke\"(   ) )   )  ; [System.Windows.Forms.Clipboard]::( \"{0}{1}\"-f'Cle','ar'  ).\"I`N`VOKe\"(  )"</p>
   <p>CMd.eXE   /C "ECho/Invoke-Expression (New-Object Net.WebClient).DownloadString|C:\WINDOWS\system32\cLIP && C:\wINdowS\SYSwOW64\windoWSPOWeRshell\V1.0\pOwERsHELl.eXe -StA   ${Nu`ll}  = [Reflection.Assembly]::(  \"{0}{3}{5}{1}{4}{2}\" -f( \"{0}{1}\"-f 'Load','W'  ),'a','e','ith',(  \"{0}{1}\" -f'lN','am' ),(  \"{0}{1}\" -f'Part','i')).\"I`Nvo`ke\"( (  \"{2}{0}{3}{4}{1}\"-f 'tem.Window','s','Sys','s','.Form'  ) );   (  [Windows.fOrms.clIpboaRd]::( \"{1}{0}{2}\" -f'x',( \"{0}{1}\" -f'GETt','E'  ),'T' ).\"Inv`o`kE\"(   )  )^|  .( ${eNV`:c`o`MSPEc}[4,24,25]-JoiN'');[Windows.Forms.Clipboard]::(  \"{2}{0}{1}\"-f 'etT','ext','S'  ).\"INVo`kE\"(' ' )"</p>
  </td>
  <td align="left">These options just change the way of execution, it might be enough to just check for those keys</td>
 </tr>
</table> 

### VAR++ LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)
<table style="word-break: keep-all;">
 <tr>
  <th align="center">Task #</th>
  <th align="center">Option</th>
  <th align="center">Results</th>
  <th align="center">Comments</th>
 </tr>
 <tr>
  <td align="center">27</td>
  <td align="center">LAUNCHER\VAR++\*</td>
  <td>
   <p><strong>Options LAUNCHER\VAR++\0 - LAUNCHER\VAR++\8 of this launcher just apply different PS keys the same way as <a href="https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#ps-launcher-obfuscation"> LAUNCHER\PS\* (case 10)</a>, so in this case we should only hunt for VAR++ indicators:</strong></p>
   <p>C:\wINDOwS\SYStEM32\CmD  /C   "SeT  jxGL=Invoke-Expression (New-Object Net.WebClient).DownloadString&&Set   wtI=poweRsHELL     ^^^&(  \"{1}{0}\"-f'ex','I' ) ( (  .(\"{1}{0}\" -f'I','gc' ) ( \"{0}{1}{2}\" -f'E','nv',':jXgL')).\"v`AluE\" ) &&   C:\wINDOwS\SYStEM32\CmD  /C%wTi%"</p>
   <p>c:\WiNDOWS\sYSTEm32\CmD.exE  /C   "sEt   DeJLz=Invoke-Expression (New-Object Net.WebClient).DownloadString&&set  yBKM=PoWERShelL  -noeX       ^^^&(\"{2}{0}{1}\"-f '-ItE','m','seT')  ( 'V'  + 'a'+  'RiAblE:z8J'  +'U2'  + 'l'  ) ([TYpE]( \"{2}{3}{0}{1}\"-f 'e','NT','e','NViRONM'  )   )    ;   ^^^&  (   ( [sTrIng]${VE`Rbo`SepReFER`Ence})[1,3] +  'X'-joIN'')(   (    (.('gI') ('V' + 'a'  + 'RIAbLe:z8j' +  'u2'  +'l'  ) ).vALUe::( \"{2}{5}{0}{1}{6}{4}{3}\" -f 'IRo','Nm','GETE','ABlE','I','nv','enTVAr').Invoke((  \"{0}{1}\"-f'd','ejLz' ),(  \"{1}{2}{0}\"-f'cEss','P','RO') ))   )&& c:\WiNDOWS\sYSTEm32\CmD.exE  /C %ybkm%"</p>
   <p>cMD /c   "SeT   xClr=Invoke-Expression (New-Object Net.WebClient).DownloadString&&SET  Fck=pOWersheLL -NOninTe   ${L3`V`BF6}  = [TypE](  \"{0}{2}{1}\"-F'envIro','t','NMEN'  );   ${ExEcUtionCoNteXt}.\"i`NvOkeCoM`manD\".\"I`NVOk`es`CrIPT\"(( (  .( \"{2}{1}{0}\" -f 'itEM','-ChIld','GeT'  ) variaBLE:l3VbF6    ).vAlue::(\"{1}{0}{4}{2}{3}\" -f 'V','GEtEn','riA','BLE','IronMenTvA' ).Invoke(( \"{0}{1}\"-f'XC','lr'  ),(\"{1}{0}\"-f'eSs','PROc') ))    )&& cMD /c %FcK%"</p>
   <p>C:\WINdOws\sYStEM32\cMD   /C   "Set   GjQ=Invoke-Expression (New-Object Net.WebClient).DownloadString&&seT  QbzO=poWersHELL  -nOl     (.(\"{0}{1}{2}{3}\"-f 'g','Et','-VA','RIAblE') (\"{0}{2}{1}\" -f'EXECUTiOnCOnT','t','eX'  )).\"va`lUE\".\"INV`okeC`o`MmAnd\".(\"{2}{1}{3}{0}\" -f'rIpt','keS','invO','c' ).Invoke(  ( .(\"{2}{0}{1}\"-f'-I','Tem','gET'  ) ( \"{0}{1}\"-f 'eNV:G','jQ' ) ).\"VAl`UE\"  )&& C:\WINdOws\sYStEM32\cMD   /C %qBZO%"</p>
   <p>C:\WIndOwS\sYStem32\Cmd.Exe   /C  "Set  IdwE=Invoke-Expression (New-Object Net.WebClient).DownloadString&&seT   QExio=pOwersHelL -NOPROFiL      Set-iTEM  VArIAbLe:8u5q (  [TYpe]( \"{0}{2}{1}\" -f 'eNVi','Nt','ronme'  )   );  ( .(  \"{2}{1}{0}\"-f '-iTem','eT','G') ( \"{0}{2}{3}{1}\"-f 'VaRIa','X*xT','ble',':E') ).\"V`ALuE\".\"I`NV`Ok`ECO`mMand\".(\"{3}{2}{1}{0}\"-f't','RIp','c','invoKes' ).Invoke(    ( ${8u`5Q}::(\"{0}{1}{2}{5}{3}{6}{4}\"-f'g','et','E','roN','iabLe','NVI','MEnTVAR'  ).Invoke((  \"{1}{0}\" -f 'We','iD' ),( \"{0}{1}\"-f'pRo','cEss')  ) )   )&&  C:\WIndOwS\sYStem32\Cmd.Exe   /C%QexIO%"</p>
   <p>C:\WINDoWs\SYsTeM32\Cmd   /C  "sEt  lzXrV=Invoke-Expression (New-Object Net.WebClient).DownloadString&&SeT   ytw=pOwErShelL -co      ^^^&(  ${s`helL`iD}[1]  + ${sh`El`liD}[13] +'x') (   (  .(\"{1}{0}\" -f 'm','iTE') ( \"{1}{2}{3}{0}\"-f 'V','E','n','v:lzxR' )).\"v`AluE\"   )&&C:\WINDoWs\SYsTeM32\Cmd   /C %yTW%"</p>
   <p>CMD.EXe   /C "sEt  cDpyq=Invoke-Expression (New-Object Net.WebClient).DownloadString&&Set  kuxSF=pOWeRSHeLl  -WIndowsTyle  hIDDEN     (.(\"{0}{1}\" -f'C','HilDITem' ) (\"{1}{0}{2}\" -f 'v:CdPy','en','q' ) ).\"VA`LUe\"   ^^^|  ^^^&( ${verBOse`PreFE`R`ENCe}.(  \"{1}{0}\"-f'INg','ToSTR').Invoke(    )[1,3]+'X'-jOIn'')&&CMD.EXe   /C%kUXsF%"</p>
   <p>cMD.ExE  /C "SET   BudG=Invoke-Expression (New-Object Net.WebClient).DownloadString&&SeT   KhJC=PowersHeLL  -exECUtiOn  bypasS        ^^^& ( 'sV')  ( \"{1}{2}{0}\" -f'17j','X','W6'  )  (  [tYPE](\"{0}{2}{1}\" -f'En','T','ViROnmeN'  )   ) ;  (  .( \"{1}{0}{2}\" -f'rI','VA','ABlE') (  \"{0}{2}{1}{3}\"-f'EXECUtiONC','Nt','o','eXt'  ) ).\"V`AluE\".\"Inv`okecom`Mand\".(\"{2}{1}{3}{0}\"-f 'ript','vOke','In','SC' ).Invoke((  $XW617j::(  \"{2}{3}{5}{0}{1}{4}{6}\"-f 'NmE','N','gEtEnv','Ir','tVArIAb','o','lE' ).Invoke(( \"{0}{1}\" -f'bUd','g'  ),(\"{1}{0}\"-f'SS','PROCE'  ) ) )  )&&   cMD.ExE  /C%KHjC%"</p>
   <p>CMD /C"sET   KUR=Invoke-Expression (New-Object Net.WebClient).DownloadString&&Set   MxI=C:\wINDowS\sYsWow64\winDOWspoWERSheLl\V1.0\PowerShelL.EXe     ${ExEcut`IoN`cON`TExT}.\"invo`kEcoMm`A`ND\".(  \"{2}{1}{0}\" -f 'pt','EscRi','INvOk' ).Invoke( ( .(  \"{0}{1}\" -f'D','IR'  ) (  \"{0}{1}\"-f'ENV:kU','R')).\"vAl`Ue\" )&&  CMD /C%mXI%"</p>
  </td>
  <td align="left">These options just change the way of execution, it might be enough to just check for those keys</td>
 </tr>
</table> 

### STDIN++ LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)
<table style="word-break: keep-all;">
 <tr>
  <th align="center">Task #</th>
  <th align="center">Option</th>
  <th align="center">Results</th>
  <th align="center">Comments</th>
 </tr>
 <tr>
  <td align="center">28</td>
  <td align="center">LAUNCHER\STDIN++\*</td>
  <td>
   <p><strong>Options LAUNCHER\STDIN++\0 - LAUNCHER\STDIN++\8 of this launcher just apply different PS keys the same way as <a href="https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#ps-launcher-obfuscation"> LAUNCHER\PS\* (case 10)</a>, so in this case we should only hunt for STDIN++ indicators:</strong></p>
   <p>cmD  /c "SEt  nEp=  Invoke-Expression (New-Object Net.WebClient).DownloadString  &&set  EcPq=Echo  (DIr vaRIAblE:*XeC*T).valuE.iNvOKeCOmMaNd.InVOKEscrIpT( ([eNViROnMenT]::geTenvIRONmentVArIabLE('nEP','PROCeSS')) )^|PowersHElL     (VArIABle 'eXeCUtIoNContext' -VAL).InVokeCoMmand.InvOkEscRipt(  ${InPuT} ) && cmD  /c %eCPQ%"</p>
   <p>C:\wiNdOWs\SystEm32\cMD.EXe   /c  "sET  XnK=  Invoke-Expression (New-Object Net.WebClient).DownloadString &&  sET  PZVh=ECho  ${EXECutIoNcOnTExT}.inVokecommaNd.iNvoKeSCrIPt(  ([eNvirOnMEnT]::GETenVIrOnmENtVARIABLe('XNk','pRoceSS'))) ^| poweRSHelL -NoE      -  &&  C:\wiNdOWs\SystEm32\cMD.EXe   /c%PzVh%"</p>
   <p>CmD.ExE/c   "SEt   jqP=  Invoke-Expression (New-Object Net.WebClient).DownloadString &&  sET  BvZ=eChO InVOKe-eXPreSsioN  ([enviRONMent]::GEteNVIrONmENTvArIAblE('JQP','pROceSS'))  ^| POWerSHELl -NoNinTE     $INPUt^^^| ^^^&( $sheLlid[1]+$ShELlid[13]+'x')&&  CmD.ExE/c%bVz%"</p>
   <p>cMd.EXE  /C  "SET   RiJ= Invoke-Expression (New-Object Net.WebClient).DownloadString && sET  KTpFR=Echo ${eXEcuTIONcOnTEXT}.iNVOkeCommAND.INvOKeScrIpT( (GCi eNV:rIj).vaLUe )  ^|PoWeRsheLL -NOLoG    (GET-chiLDIteM 'VArIaBlE:ex*XT').vAlue.InvokECOMmand.iNvokEScrIpT($iNPut)&&  cMd.EXE  /C%ktpfR%"</p>
   <p>CmD.EXE /C "SeT   khW=Invoke-Expression (New-Object Net.WebClient).DownloadString&&Set   XWPGa=ecHO ${EXECuTIonCOntext}.inVOKeCommand.iNVoKESCRipt((GeT-iTem EnV:khW).vaLuE  )  ^|PoWERsHell -nOproF      .( $Env:cOmSPec[4,26,25]-jOiN'')( ${inPuT} )   &&CmD.EXE /C%XWpGA%"</p>
   <p>c:\wiNDOwS\syStem32\CMd.Exe   /C "sEt   xjIow=  Invoke-Expression (New-Object Net.WebClient).DownloadString&&sEt  niG=Echo iEx (GI ENv:XjIOW).valUE  ^|  powersheLl -coMm      (chIlditeM 'vARIaBle:eX*XT').vAlUE.iNvoKEcoMMaNd.invokEScrIpT( $InpuT  )&& c:\wiNDOwS\syStem32\CMd.Exe   /C %NIg%"</p>
   <p>CMd/C "sEt   Guz= Invoke-Expression (New-Object Net.WebClient).DownloadString  &&set   Cpa=echO  INVoKe-exprESSiOn  (iteM env:gUZ).vALuE ^| POWeRSHElL  -wInD  hIddEn    ${ExecutioncOntexT}.invokECOmmaND.invokescriPt( ${iNpuT} ) &&  CMd/C%Cpa%"</p>
   <p>C:\wInDOWS\sYsTEM32\cMD   /c "SET  RnK= Invoke-Expression (New-Object Net.WebClient).DownloadString &&sEt   ryP=ECHo  (GCi vaRIABlE:E*oNTe*).VaLUe.iNvokecOmMaNd.inVOKeScrIPt( ([eNVirONmENT]::GEtENVirOnMeNTvArIAblE('rNk','PROcEsS')) )  ^| PowershelL  -EXecu  byPAsS    $eXecutiOnCONTeXT.invokeCoMmAND.iNVOKEsCrIpT($iNPUT )  && C:\wInDOWS\sYsTEM32\cMD   /c %RyP%"</p>
   <p>C:\winDowS\SysteM32\Cmd   /C  "set   sHM=Invoke-Expression (New-Object Net.WebClient).DownloadString  && SEt  gBc=ECHO  $eXECutionconTeXt.inVoKECOmmanD.InVoKESCripT(  ([ENVirOnment]::geTenVIrONMEnTvaRIAble('shM','PRoCEss')) )  ^| C:\WiNDoWS\SYSwoW64\WindoWSpoWerSHelL\V1.0\pOwersheLl.EXe    ^^^&( $PShOME[4]+$psHOMe[30]+'X') ( $InPUt)  && C:\winDowS\SysteM32\Cmd   /C %gbc%"</p>
  </td>
  <td align="left">These options just change the way of execution, it might be enough to just check for those keys</td>
 </tr>
</table> 

### CLIP++ LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)
<table style="word-break: keep-all;">
 <tr>
  <th align="center">Task #</th>
  <th align="center">Option</th>
  <th align="center">Results</th>
  <th align="center">Comments</th>
 </tr>
 <tr>
  <td align="center">29</td>
  <td align="center">LAUNCHER\CLIP++\*</td>
  <td>
   <p><strong>Options LAUNCHER\CLIP++\0 - LAUNCHER\CLIP++\8 of this launcher just apply different PS keys the same way as <a href="https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#ps-launcher-obfuscation"> LAUNCHER\PS\* (case 10)</a>, so in this case we should only hunt for CLIP++ indicators:</strong></p>
   <p>C:\WINdoWS\sySteM32\CMd  /c  " ECho\Invoke-Expression (New-Object Net.WebClient).DownloadString|Clip.Exe&&C:\WINdoWS\sySteM32\CMd  /c  pOWerSheLl  -STa      .  ( \"{2}{0}{1}\"-f'dd-',(\"{0}{1}\" -f 'T','ype' ),'A'  ) -Assembly ( \"{4}{1}{3}{0}{2}\"-f (\"{0}{1}\" -f 'nd','ow'),(  \"{1}{0}\"-f'.W','stem' ),( \"{2}{1}{0}\" -f 'rms','Fo','s.'),'i','Sy')   ;  ${exeCUtIOnCONTeXT}.\"INV`oKECOM`m`ANd\".\"INV`ok`ESCriPT\"(  (  [sYSteM.wiNDoWS.forMs.ClIPboaRD]::( \"{2}{0}{1}\" -f'Ex','t',(\"{0}{1}\" -f'Get','t' )  ).\"iNvo`Ke\"(  ))   )   ;   [System.Windows.Forms.Clipboard]::(\"{1}{0}\" -f 'ar','Cle' ).\"in`V`oKE\"(   )"</p>
   <p>C:\WInDows\System32\cMd  /c " echO Invoke-Expression (New-Object Net.WebClient).DownloadString |C:\wiNDOwS\SyStEm32\cLiP.exE &&C:\WInDows\System32\cMd  /c  poWErsheLl -sT -NoexiT   ^^^& (\"{0}{1}{2}\"-f 'Ad','d-T','ype'  ) -A (  \"{4}{0}{1}{2}{3}\"-f 'y',( \"{0}{2}{1}\"-f'stem','indow','.W' ),'s.F',( \"{0}{1}\"-f'orm','s' ),'S'  ) ;  ${EXEcUtIONcONtEXT}.\"IN`Vo`kECoMm`AnD\".\"I`N`VoKESCriPT\"(    (  [WInDoWS.FoRMS.ClipboArD]::(\"{0}{1}\"-f'GE',(\"{0}{1}\"-f 'TT','EXt')  ).\"INV`Oke\"(  )  )  )  ; [Windows.Forms.Clipboard]::(\"{0}{1}\" -f'C',(\"{0}{1}\" -f'le','ar' ) ).\"iN`V`oKe\"(    )"</p>
   <p>C:\wiNdowS\syStEm32\cmd   /C" ecHO Invoke-Expression (New-Object Net.WebClient).DownloadString | clIp&&C:\wiNdowS\syStEm32\cmd   /CPoWeRSHEll -sta -NonIntERaCTI   ${nu`LL} = [System.Reflection.Assembly]::( \"{2}{1}{3}{0}\" -f(\"{0}{1}\" -f'l','Name' ),'it',( \"{1}{0}\"-f 'adW','Lo' ),(\"{0}{1}\" -f 'hPart','ia')).\"i`NvOke\"((\"{3}{4}{1}{0}{2}\" -f'Windows.For','tem.','ms','Sy','s'))   ;  ${eX`Ec`UT`ioN`coNteXt}.\"I`N`VOKEcOMm`And\".\"In`VOkES`CRipt\"(   ([WInDowS.fORmS.cLipbOArD]::( \"{1}{0}\"-f'EXt',(\"{1}{0}\" -f 'T','gET' )).\"INV`okE\"(    ) )   );    [Windows.Forms.Clipboard]::( \"{1}{0}{2}\"-f 'x',( \"{1}{0}\" -f 'tTe','Se' ),'t' ).\"i`NvoKe\"(' '  )"</p>
   <p>C:\WINDowS\sYsTEM32\CmD.eXE   /C"  echo\Invoke-Expression (New-Object Net.WebClient).DownloadString| C:\WIndOWs\SYSteM32\CLip &&C:\WINDowS\sYsTEM32\CmD.eXE   /C   POWERSHeLL  -sT  -noL    [Void][System.Reflection.Assembly]::( \"{0}{3}{4}{1}{2}\" -f(  \"{0}{1}\"-f'Lo','adW'  ),( \"{0}{1}\"-f 'Par','t'),(  \"{0}{1}{2}\"-f 'ial','N','ame'),'it','h'  ).\"in`VO`KE\"( ( \"{3}{1}{4}{5}{2}{0}\"-f'rms','ystem.Windo','Fo','S','w','s.'  ))   ;  ( [wIndows.fOrms.cLIPBOArD]::(  \"{1}{0}\"-f'T',(  \"{1}{0}\" -f'tEX','gET'  )).\"i`Nvoke\"(    )  )   ^^^|  ^^^&  (  ( ^^^& ( \"{2}{1}{0}\"-f 'e',( \"{2}{1}{0}\"-f'IABl','aR','v'  ),(  \"{0}{1}\"-f'Get','-' )  ) ( \"{1}{0}\"-f'*','*MDr'  )).\"n`Ame\"[3,11,2]-jOin'')   ;  [Windows.Forms.Clipboard]::(  \"{0}{1}\" -f (\"{1}{0}\"-f'tT','Se' ),'ext').\"in`VoKe\"(' ' )"</p>
   <p>C:\WINdOws\sYsTeM32\Cmd.EXE  /C"EcHO/Invoke-Expression (New-Object Net.WebClient).DownloadString |CLIp&&C:\WINdOws\sYsTeM32\Cmd.EXE  /C powErShELl  -StA  -NOpRoFIl     . (\"{2}{0}{1}\" -f'-T','ype','Add') -Assem ( \"{1}{3}{0}{4}{2}\" -f'ent','Pre',(\"{2}{0}{1}\"-f 'nCor','e','io' ),'s','at'  ) ; ( ^^^&(  \"{1}{0}{2}\" -f(  \"{0}{1}\"-f'rIab','L'),'va','e'  ) (  \"{1}{0}{4}{3}{2}\" -f'xEc','e','OncontEXt','tI','u' )  ).\"va`lUe\".\"invok`E`cOmM`AnD\".\"INv`o`k`EscRIPt\"(  ( [SySTEm.wINDoWs.CLipbOARd]::( \"{1}{0}\" -f'xt',(\"{0}{1}\"-f 'gEt','Te' )).\"i`NVO`ke\"(   ) )  )  ;   [System.Windows.Clipboard]::(\"{1}{0}\" -f't',( \"{0}{1}\" -f'Se','tTex')).\"INvo`KE\"(' ')"</p>
   <p>CmD/C "Echo/Invoke-Expression (New-Object Net.WebClient).DownloadString|c:\windOWs\systEM32\ClIP &&CmD/C poweRshell  -ST -comMaNd      ^^^&  (  \"{0}{1}\"-f( \"{0}{1}\" -f'A','dd-'),(\"{0}{1}\"-f'Ty','pe'  )) -AssemblyNam (  \"{0}{3}{1}{2}\"-f(\"{0}{1}{2}\" -f'Pre','se','nt' ),'onC','ore','ati'  )  ; ${exECUtioncONText}.\"iNVOkEC`o`MMA`Nd\".\"I`N`VokESCR`IPT\"(  ([WInDowS.clIPBOARD]::(\"{0}{1}\" -f 'g',( \"{0}{1}\" -f'Ette','Xt' )).\"iN`V`OKE\"())  )  ;[Windows.Clipboard]::(\"{1}{0}\" -f'ear','Cl').\"iN`Voke\"(   )"</p>
   <p>cmd  /C"  eChO\Invoke-Expression (New-Object Net.WebClient).DownloadString |CliP&&cmd  /C   pOWeRshELl -ST -WINdOwStY  HiddeN    ${U`A`TVRY}  = [System.Reflection.Assembly]::(  \"{0}{3}{4}{1}{2}\" -f (  \"{1}{0}\" -f'd','Loa'  ),'l',( \"{0}{1}\"-f 'N','ame'  ),( \"{2}{0}{1}\" -f'Pa','rti','With'  ),'a' ).\"in`VokE\"( (  \"{5}{2}{3}{6}{4}{0}{1}\" -f 'ws.','Forms','y','st','Windo','S','em.'  )) ;  ([wIndoWS.formS.cLipbOARD]::(  \"{1}{0}\"-f (\"{0}{1}\"-f 'e','tTExT'),'G'  ).\"inVO`kE\"(  ))  ^^^|^^^&  (   ${v`e`RbOsePRe`FErENCE}.(  \"{1}{2}{0}\"-f 'G','tos',( \"{1}{0}\" -f 'riN','t') ).\"In`V`OKe\"(    )[1,3]+'x'-JOIn'' )  ;   [Windows.Forms.Clipboard]::(\"{0}{1}\" -f 'C',( \"{1}{0}\"-f 'r','lea'  )  ).\"iN`VOke\"(  )"</p>
   <p>c:\WINdoWS\SYsteM32\cmd.Exe  /c " Echo Invoke-Expression (New-Object Net.WebClient).DownloadString |C:\wInDows\sYSTEM32\ClIp.EXE&&c:\WINdoWS\SYsteM32\cmd.Exe  /c powERshelL  -EXEcUtionpol BYPaSs  -ST  ^^^&(\"{0}{2}{1}\"-f (  \"{0}{1}\"-f'Ad','d-T'),'pe','y'  ) -As (\"{2}{0}{1}{3}\" -f're','s','P',(\"{2}{1}{0}\"-f 're','nCo','entatio'  )  )  ;    ([WiNdOwS.cLIPBOArd]::(  \"{2}{1}{0}\" -f( \"{1}{0}\"-f 'tEXt','t' ),'e','G' ).\"INV`OKe\"(  )  )  ^^^|  . (   (  [sTRING]${ve`RBosEp`ReFe`Re`NcE} )[1,3] +  'x'-join'' )  ;   [Windows.Clipboard]::(\"{2}{1}{0}\" -f(\"{0}{1}\"-f't','Text'  ),'e','S'  ).\"In`VO`kE\"(  ' ')"</p>
   <p>CMd/C   "  ecHo Invoke-Expression (New-Object Net.WebClient).DownloadString| C:\wiNdows\system32\ClIp.ExE&&CMd/Cc:\WinDows\sysWow64\wiNdowsPOWersHelL\v1.0\PoweRsHElL.exE -Sta     .  (\"{1}{0}{2}\" -f 'T',( \"{0}{1}\"-f 'A','dd-' ),'ype'  ) -AN (  \"{1}{0}{2}{3}{4}\"-f(\"{0}{2}{3}{1}\" -f 'tem','s.F','.','Window' ),'Sys','or','m','s'  )  ;   ${exECUTIOncONTeXT}.\"in`VokeC`O`MManD\".\"invOke`S`C`RipT\"(  ( [wiNDOWs.fOrmS.clIPbOARd]::(  \"{1}{2}{0}\"-f 't',(\"{0}{1}\" -f'gE','TT' ),'Ex'  ).\"in`V`OkE\"(  ) )  ) ;    [Windows.Forms.Clipboard]::(  \"{0}{1}\" -f (\"{1}{0}\"-f'lea','C'),'r' ).\"iNV`oke\"(    )"</p>
  </td>
  <td align="left">These options just change the way of execution, it might be enough to just check for those keys</td>
 </tr>
</table> 

### RUNDLL++ LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)
<table style="word-break: keep-all;">
 <tr>
  <th align="center">Task #</th>
  <th align="center">Option</th>
  <th align="center">Results</th>
  <th align="center">Comments</th>
 </tr>
 <tr>
  <td align="center">30</td>
  <td align="center">LAUNCHER\RUNDLL++\*</td>
  <td>
   <p><strong>Options LAUNCHER\RUNDLL++\0 - LAUNCHER\RUNDLL++\8 of this launcher just apply different PS keys the same way as <a href="https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#ps-launcher-obfuscation"> LAUNCHER\PS\* (case 10)</a>, so in this case we should only hunt for RUNDLL++ indicators:</strong></p>
   <p>c:\WiNdOws\sySTeM32\cMd   /c "SeT  jgXU=Invoke-Expression (New-Object Net.WebClient).DownloadString&&RuNdLL32.exe  SHELL32.DLL , ,, ShellExec_RunDLL   "pOWERshelL"      " (.('GI'  ) (  '{0}{1}'-f'ENv:jG','Xu') ).'VALUe' ^|  .  ( '{1}{0}'-f'ex','i' )"</p>
   <p>C:\wIndows\sysTEM32\cMd.eXE   /C"sET   EvXC=Invoke-Expression (New-Object Net.WebClient).DownloadString&&RunDLL32   SHELL32.DLL, ,  ,ShellExec_RunDLL   "POWeRsheLl"   "-NoEXi "    "  $pctJ7F  =  [TYpE]('{2}{1}{0}{3}'-F 'O','NVir','E','NmeNT')  ;   ( ^&  ( '{0}{1}' -f 'i','tem' ) ('{0}{5}{1}{2}{4}{3}'-f 'v','LE',':EXECu','IoNcOnTexT','T','aRiaB')).'vALUe'.'invoKeCommaND'.(  '{0}{2}{1}{3}'-f'I','KE','Nvo','sCRIpt').Invoke(  (    $Pctj7f::('{2}{0}{3}{1}{5}{4}' -f 'NvIrO','VA','getE','nMEnt','E','rIAbl' ).Invoke(  (  '{1}{0}'-f'C','EvX'),('{1}{0}{2}' -f's','Proce','s' )  ))    )"</p>
   <p>c:\wInDOWS\SySTeM32\CMD.exe  /c  "Set  gsJ=Invoke-Expression (New-Object Net.WebClient).DownloadString&&C:\WInDoWs\SYSTEM32\RUndll32.exe  SHELL32.DLL ShellExec_RunDLL   "pOwershELL"   " -NONiNter"     "   .('sV' ) je3  (   [TypE]('{2}{0}{1}' -F'NMen','t','envIRO'  )   )   ;    .(  '{4}{3}{0}{1}{2}' -f'pR','EsSio','n','ex','iNVokE-'  )( (   (  . (  '{1}{2}{0}' -f 'ITeM','gE','t-') VAriaBLe:je3  ).VAlUe::( '{3}{5}{0}{4}{1}{6}{2}'-f'nV','Me','IABLE','g','IroN','ETE','NTVar' ).Invoke( 'gSj',( '{1}{0}{2}' -f'OCE','Pr','ss') ) ) )"</p>
   <p>C:\winDoWS\sYStem32\CMD  /c"sEt  iQw=Invoke-Expression (New-Object Net.WebClient).DownloadString&&C:\WIndoWS\sYSTEm32\runDll32.eXE  SHELL32.DLL,ShellExec_RunDLL   "PoweRShell"  "-NoLOGO "    "  ^&(   ( [strinG]${VERBoSEPReFEReNcE}  )[1,3]  +'X'-JOIn'' ) ( (   ^& ('{2}{0}{1}' -f 'iTe','m','chILD'  ) (  '{1}{0}' -f ':Iqw','EnV')).'VALUE'  ) "</p>
   <p>CmD.EXE /c  "SEt  igfM=Invoke-Expression (New-Object Net.WebClient).DownloadString&&RuNdll32   SHELL32.DLL ShellExec_RunDLL  "PoWERsheLl"  "  -noPRoFIL "     " (    ^&  ( '{1}{2}{3}{0}' -f 'eM','GE','t-child','IT'  ) ( '{0}{1}' -f'E','nV:igFm' ) ).'VAlUE' ^|    .  ( '{1}{0}'-f 'x','ie')"</p>
   <p>C:\wINdoWs\sYsTEm32\CMD.eXE  /C  "set  Ahi=Invoke-Expression (New-Object Net.WebClient).DownloadString&&rundLL32 SHELL32.DLL,  , ShellExec_RunDLL  "pOweRshELL" "  -C  "   " ( .(  '{0}{1}'-f 'iT','em') ( '{1}{2}{0}'-f'ahI','EN','V:')).'ValUE' ^|  . ( ${eNV:cOMspEC}[4,15,25]-Join''  )"</p>
   <p>cmd   /C "seT  LFM=Invoke-Expression (New-Object Net.WebClient).DownloadString&&c:\WinDoWs\sYsTeM32\ruNdll32  SHELL32.DLL ShellExec_RunDLL   "powERshELL"  " -WIndOW  hIdD"    "$PGRV4H = [TyPe](  '{3}{2}{1}{0}'-F 'Nt','E','OnM','ENvIr'  )    ; ${exeCUTIoNcONText}.'INVoKEcOMmaNd'.( '{1}{2}{0}'-f'CRIpT','iNvOkE','s' ).Invoke(    (   (  gi  variAbLE:pgRV4h  ).'vALuE'::(  '{1}{4}{0}{5}{3}{2}{6}' -f'M','GEtEn','vA','t','ViRoN','En','rIabLe' ).Invoke('lfm',('{0}{1}{2}' -f'PROc','E','SS') ) )   )"</p>
   <p>c:\WINDOws\SysTEm32\CMD.exE  /c "sEt  uCQSx=Invoke-Expression (New-Object Net.WebClient).DownloadString&&RundLL32  SHELL32.DLL,ShellExec_RunDLL   "POWerShELL" "  -eXeCuTIonPOl  bYpaSs  "    "(   ^& ( '{2}{1}{3}{0}'-f 'ItEM','eT-ch','g','iLD') ('{1}{0}{2}'-f 'RIABLe:ex*','va','xT' )).'VAlUE'.'InVokeCommaND'.('{2}{3}{0}{1}'-f 'c','Ript','iNvoKe','S').Invoke( (.('{3}{0}{2}{1}'-f 't-','m','CHIldiTE','GE') ('{0}{1}' -f 'E','NV:UcQsx' )  ).'VAlUE'  )"</p>
   <p>CMD.ExE   /C "SeT  vPu=Invoke-Expression (New-Object Net.WebClient).DownloadString&&rUnDlL32   SHELL32.DLL,ShellExec_RunDLL   "C:\WinDOWs\SYSwOw64\WiNDOWSPOWERshELl\v1.0\PoWERshELL.exe"     "(  .( '{1}{0}' -f'Ci','g'  ) ( '{0}{2}{1}' -f'e','VPu','nV:'  )).'VaLUE' ^|  ^&  (   ${eNV:cOMSPeC}[4,26,25]-JoIN'')"</p>
  </td>
  <td align="left">These options just change the way of execution, it might be enough to just check for those keys</td>
 </tr>
</table> 

### MSHTA++ LAUNCHER OBFUSCATION
[Back to the Contents :page_facing_up:](https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#contents)
<table style="word-break: keep-all;">
 <tr>
  <th align="center">Task #</th>
  <th align="center">Option</th>
  <th align="center">Results</th>
  <th align="center">Comments</th>
 </tr>
 <tr>
  <td align="center">31</td>
  <td align="center">LAUNCHER\MSHTA++\*</td>
  <td>
   <p><strong>Options LAUNCHER\MSHTA++\0 - LAUNCHER\MSHTA++\8 of this launcher just apply different PS keys the same way as <a href="https://github.com/zinint/oscd_Invoke-Obfuscation/blob/master/OSCD%20-%20Issue%20-%20Invoke-Obfuscation.md#ps-launcher-obfuscation"> LAUNCHER\PS\* (case 10)</a>, so in this case we should only hunt for MSHTA++ indicators:</strong></p>
   <p>c:\winDowS\syStEM32\CmD   /c  "SeT   vaw=Invoke-Expression (New-Object Net.WebClient).DownloadString&&C:\windoWs\SYsTem32\msHTa VBSCrIpt:CREatEObJeCT("WScriPT.ShEll").Run("POwERShElL      (  ^& ( '{1}{0}'-f'I','GC') ('{0}{2}{1}' -f'eNv:','w','Va'  )).'vAlue'   ^|   . (   ${PshOmE}[21] +${psHOme}[34] +'x')",(11-1-9),TRuE)(WiNdOW.ClOsE)"</p>
   <p>CMD.exE/C  "SeT   Qsk=Invoke-Expression (New-Object Net.WebClient).DownloadString&&C:\windoWS\SYStEm32\MSHtA  VBScRIpT:CREATeObjECt("WSc"+"RIP"+"T."+"SHeLL").RuN("POWERShell  -NoeX     (   ^&(  '{1}{2}{0}' -f 'tEm','get-C','hilDI'  ) ('{1}{0}'-f 'Sk','ENV:Q' ) ).'vAlue'^|^& ( (    ^&  (  'GV'  ) ( '{1}{0}'-f 'dR*','*M')).'name'[3,11,2]-JOIn'')",15-11-3,TRUE)(WiNDOW.CLOSE)"</p>
   <p>C:\WinDOwS\SystEm32\cMD.EXe   /c "sET  mQn=Invoke-Expression (New-Object Net.WebClient).DownloadString&&MsHta  VBScript:CReATEOBjeCt("WS"+"c"+"r"+"IPT."+"ShelL").Run("POWerSHEll  -NOniNtera    ${EXECUtIonCONText}.'iNVokEcOmmaNd'.(  '{3}{2}{0}{1}' -f 'P','t','OkescrI','iNv'  ).Invoke( (  ^&  ('{0}{1}'-f'GC','I') (  '{0}{1}' -f'EN','v:MQn') ).'VAlUE'    )",(12-11),TrUe)(WIndoW.ClosE)"</p>
   <p>C:\WindOws\SySTeM32\cmd.exE   /c  "sET   Hlyd=Invoke-Expression (New-Object Net.WebClient).DownloadString&&c:\wInDOws\SYstEM32\mShTA  VBSCRipT:CrEATeOBjecT("WSCRipT.ShElL").RUn("POwErSheLL  -NoLoG  ( .('{1}{0}' -f 'ITem','CHILD') ( '{0}{2}{1}'-f 'eNV','lyd',':H' )).'VAlUE'   ^| .( ${pshomE}[4]  +  ${pSHome}[30]  +  'X')",(24-23),True)(WInDow.Close)"</p>
   <p>cMD/C   "sET  Nkl=Invoke-Expression (New-Object Net.WebClient).DownloadString&&c:\WINDOWS\sYStEm32\MsHTa  VBSCRIPT:CreaTEObjeCT("WScRIPT.ShelL").RuN("POwersheLl  -nOPRoFIL    ${exEcUtioncONTEXt}.'invoKecOMMAND'.( '{3}{1}{2}{0}' -f 'pT','nvoKEs','cRI','I').Invoke(    (  ^&  (  '{0}{1}' -f'ite','m'  ) ('{2}{0}{1}'-f':n','KL','EnV'  )).'VaLUE' )",1,TrUe)(WINdow.CLOse)"</p>
   <p>C:\WinDOWs\sySTEm32\CMD   /c"SET   lheP=Invoke-Expression (New-Object Net.WebClient).DownloadString&&C:\WIndows\sYStEm32\MshTA   VBScript:CReaTeObJeCt("WSC"+"RiPT"+".ShElL").RUN("POwErSHeLL -COMma    (.(  '{1}{0}' -f 'i','GC') ('{1}{0}{2}' -f 'v','EN',':lhEp') ).'value'  ^|  ^&  ( ( ^& ('{2}{0}{1}'-f 'ET-va','rIable','g' ) ( '{1}{0}' -f'r*','*MD'  ) ).'NamE'[3,11,2]-JoIN'' )",(9-2-6),TRUe)(WiNdow.ClosE)"</p>
   <p>c:\wiNDoWs\sYStEm32\cmd.EXe  /c"Set  sPvk=Invoke-Expression (New-Object Net.WebClient).DownloadString&&msHTa.exe  VBSCripT:CreaTEObjeCT("WSCRI"+"pT.SHe"+"l"+"L").RuN("POWERshELL -WindowSTyL 1    (^&  (  '{0}{1}{2}'-f 'cHIldIt','e','M'  ) (  '{0}{1}{2}' -f'E','Nv:spv','K' )).'VAlUe' ^|   .   ( ${PShOmE}[4] + ${psHOME}[30] + 'X' )",1,TRuE)(WindOW.Close)"</p>
   <p>c:\WIndOws\SYStem32\CMd.exe   /c   "SET  Xuz=Invoke-Expression (New-Object Net.WebClient).DownloadString&&mSHta.Exe VBScriPt:CREatEObJECT("WSCRIPT.SHeLL").RUn("pOwErsHell -eXecuTIO  BYPAsS  ${eXeCUTiONCONText}.'inVOkecOmMANd'.( '{2}{0}{1}' -f'vOkEScRi','Pt','in' ).Invoke(   ( .('{1}{0}{2}' -f'iTe','child','M') ('{2}{1}{0}' -f 'z','U','EnV:X'  ) ).'vAlue'  )",1,TRuE)(WindoW.ClOsE)"</p>
   <p>cMd  /C "sET   yAt=Invoke-Expression (New-Object Net.WebClient).DownloadString&&MSHTA  VBSCRiPT:CrEaTeOBjECT("WSC"+"R"+"i"+"p"+"t.ShELL").RuN("c:\WIndOWS\sYSWow64\WInDoWspOWErSHeLL\v1.0\pOWErsHeLL.exe     (  .('gV'  ) (  '{0}{1}'-f'eX','*xT' )).'ValUE'.'inVokECoMmand'.( '{2}{3}{1}{0}' -f 'iPt','EsCR','I','nVoK').Invoke((   ^&('{1}{0}' -f 'm','ITe' ) ('{0}{2}{1}'-f'env','AT',':y'  ) ).'vAlUE'    )",(14-13),TRUE)(WinDOW.CLoSe)"</p>
  </td>
  <td align="left">These options just change the way of execution, it might be enough to just check for those keys</td>
 </tr>
</table> 
