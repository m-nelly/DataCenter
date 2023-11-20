## General Syntax:
PowerShell follows a Verb-Noun -Parameter Value syntax pretty strictly, for example: 
`Get-Process -Name svchost` - Returns running svchost processes.

Common Verbs: 
`Add | Disable | Enable | Get | New | Remove | Set | Start | Stop | Update | Write`
Basic Commands:
```powershell
Get-Command #Returns all available commands.
Get-Command -Name *Process*

Get-Alias #Returns all assigned aliases.
Get-Alias cd

Get-Help #Returns the help page for a specified command.
Get-Help Find-Command -Online

Set-Alias #Creates an alias.
Set-Alias -Name help -Value Get-Help

Get-Process #Returns a list of running processes. 
(ps)Get-Process -Name svchost -IncludeUserName

Set-Location #Changes location to specified file path. 
(cd, chdir)Set-Location C:\\

Get-ChildItem #Returns files in the current directory. 
(ls)Get-ChildItem -Recurse    #Hint: Ctrl+C Gotcha!

Invoke-WebRequest #Transfer data to or from a server. 
(curl, wget)Invoke-WebRequest <https://www.google.com/>

Stop-Process #Stops a running process. 
(kill)Stop-Process -Name explorer

Get-Location #Shows current directory path. 
(pwd)Get-Location

Get-Member #Gets properties, methods, and objects for a command.
Get-Command | Get-Member
```
## Scripting:
### Variables:
```powershell
$VarName = Value$VarArray = 1,2,3,4
$VarHashTable = @{Color = Red; Size = Large; Shape = Square} 
# Make big arrays faster by using the ArrayList object instead of a regular array:
$bigarray =  [System.Collections.ArrayList]@()@(0..99998).foreach({$null = $bigArray.Add($_)})
```
### Operators:
```powershell
+ #Addition
- #Subtraction
/ #Division
* #Multiplication
-gt #Greater than
-lt #Less than
-eq #Equal
-ne #Not equal
-ge #Greater or equal
-le #Less or equal
| #Inputs the output of one command into another
= #Assignment (C = A + B)
+= #Add AND (C+=1 adds 1 to C and assigns the new value.)
-= #Subtract AND (C-=1 subtracts 1 from C and assigns the new value.)
AND #Logical 
ANDOR #Logical 
ORNOT #Logical 
NOT #Logical
> #Redirects output to specified file.
% #ForEach-Object
? #Where-Object
$_ #THIS Operator (Represents current pipeline object)Get-Process | ? {$_.name -Match "svchost"}

#Make big arrays faster by using the ArrayList object instead of a regular array:

$bigarray =  [System.Collections.ArrayList]@()@(0..99998).foreach({$null = $bigArray.Add($_)})
```
### Advanced Help:
```powershell
Get-Help #can be used with wildcards to get cmdlets. 
Get-Help Get*Get-Help -ShowWindow #opens full help in a separate window which is very helpful. 
Get-Help -ShowWindow Get-Process#Within the help output, there are a few things to note. I have copied a sample below. 
SYNTAXGet-Command [[-ArgumentList] <Object[]>] [-Verb <string[]>] [-Noun <string[]>] [-Module <string[]>][-FullyQualifiedModule <ModuleSpecification[]>] [-TotalCount <int>] [-Syntax] [-ShowCommandInfo] [-All][-ListImported] [-ParameterName <string[]>] [-ParameterType <PSTypeName[]>] [<CommonParameters>]Get-Command [[-Name] <string[]>] [[-ArgumentList] <Object[]>] [-Module <string[]>] [-FullyQualifiedModule<ModuleSpecification[]>] [-CommandType {Alias | Function | Filter | Cmdlet | ExternalScript | Application | Script| Workflow | Configuration | All}] [-TotalCount <int>] [-Syntax] [-ShowCommandInfo] [-All] [-ListImported][-ParameterName <string[]>] [-ParameterType <PSTypeName[]>] [<CommonParameters>] 
#The [] denote optional parameters. #The <> gives you information about the type of information expected for arguments. 
#The second syntax block offers another parameter stack to use with the cmdlet. 
Get-Help *about #will return useful about pages for various topics in PowerShell 
-whatif #outputs the actions powershell will take if you pass a specific command without executing the command. 
-confirm #asks for confirmation before executing each step of a command.
```
### Useful Commands:
Run Commands as Remote User:
`runas /user:uname@domain.local /netonly powershell`
- Spawns a PowerShell window authenticated as the remote user. 