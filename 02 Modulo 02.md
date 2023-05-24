# Modulo 02 - Gerenciamento Domínios, Usuários, Grupos, Recursos e Licenças.

# Download .csv Import Users
https://github.com/alexosousa/treinamento-ms365/blob/main/02%20Modulo%2002%20ImportUserFile.csv

# PowerShell
Biblioteca<br>
https://learn.microsoft.com/en-us/powershell/module/
<br>MSOnline<br>
https://docs.microsoft.com/en-us/powershell/module/msonline/get-msoldomain?view=azureadps-1.0

![image](https://user-images.githubusercontent.com/49683486/172762015-17351d13-8341-4cb3-bdb7-19afabce3555.png)

#Alguns Modulos - Install , Import , Remove , Update
<br>MSOnline
<br>AzureAD
<br>AzureADPreview
<br>ExchangeOnlineManagement
<br>SharePointPnPPowerShellOnline
<br>MicrosoftTeams 

#Comandos PowerShell
<br>ADD/New *
<br>GET *
<br>SET *
<br>RESTORE *
<br>REMOVE *
<br>CONFIRM
<br>CONNECT
<br>CONVERT
<br>DISABLE/ENABLE
<br>RESET
<br>UPDATE

# Consultas Propagação DNS
<br>https://www.whatsmydns.net/
<br>https://www.digwebinterface.com/
<br>https://mxtoolbox.com/Public/Content/Toolhandler.aspx?command=mx
<br>nslookup -type=TXT matriz365.cf 8.8.8.8
<br>nslookup -type=MX matriz365.cf 8.8.8.8

#Registros DNS
<br>Exchange Online
MX Record matriz.cf.mail.protection.outlook.com (priority 0)
<br>Autodiscover
Autodiscover autodiscover.outlook.com (CNAME)
<br>SPF
TXT v=spf1 include:spf.protection.outlook.com –all

<br>Teams
Sip sipdir.online.lync.com (CNAME)
Lyncdiscover webdir.online.lync.com (CNAME)
Service, Port, Weight, Priority, Target
_sip _tls 443 1 100 sipdir.online.lync.com
_sipfederationtls _tcp 5061 1 100 sipfed.online.lync.com

#Install Modules
#Azure AD (AzureAD and MSOnline Module)
Install-Module MSOnline -AllowClobber
Install-Module AzureAD -AllowClobber
Install-Module AzureADPreview -AllowClobber

#Import Modules
#Azure AD (AzureAD and MSOnline Module)
Import-Module MSOnline
Import-Module AzureAD
Import-Module AzureADPreview

#Conectar Modulo
Connect-MsolService 
Connect-MsolService -Credential $Credential 

Gerenciamento Domínios
#Criar um Domínio
New-MsolDomain –Authentication Managed –Name empresa.com.br

#Consulta Domínio
Get-MsolDomain
Get-MsolDomain -Status Verified
Get-MsolDomainVerificationDns –DomainName empresa.com.br –Mode DnsTxtRecord

#Remover Domínio
Remove-MsolDomain -DomainName empresa.com.br
Remove-MsolDomain -DomainName empresa.com.br -Force

Gerenciamento Usuários

#Consultar Usuários
Get-MsolUser 
Get-MsolUser | Select DisplayName, ObjectID, -Synchronized
Get-MsolUser -UserPrincipalName "admin@tn0001.onmicrosoft.com" | Select DisplayName, FirstName, LastName, Department, WHENCREATED

#Criar Usuário
New-MsolUser -UserPrincipalName "user@tn0001.onmicrosoft.com" -DisplayName "User XX" -FirstName "Userxx" -LastName "XX"

#Modificar Usuário
Set-MsolUser -UserPrincipalName "user@tn0001.onmicrosoft.com" -DisplayName "David Chew" -FirstName "David" -LastName "Chew" -Department "Finance"

#Remover Usuário
Remove-MsolUser -UserPrincipalName "user@tn0001.onmicrosoft.com"
Remove-MsolUser -UserPrincipalName "user@tn0001.onmicrosoft.com" -Force

Gerenciamento Licenças

#Consultar Licenças
Get-MsolAccountSku
Get-MsolUser | Where-Object {($_.licenses).AccountSkuId -match "SPB"} #EnterprisePremium

Gerenciamento Grupos
#Consultar Grupos
Get-MsolGroup

#Criar Grupos
New-MsolGroup -DisplayName "MeuGrupo" -Description "Meu Grupo"

#Remover Grupos 
Remove-MsolGroup -objectid 05bc8e6f-81a4-455d-af2c-70bd6358da16 #exemplo
