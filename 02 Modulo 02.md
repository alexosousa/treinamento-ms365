# Modulo 02 - Gerenciamento Domínios, Usuários, Grupos, Recursos e Licenças.

# Download .csv Import Users
https://github.com/alexosousa/treinamento-ms365/blob/main/02%20Modulo%2002%20ImportUserFile.csv

# PowerShell
MSOnline<br>
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

Install Modulos
<br>Install-Module MSOnline
<br>Install-Module AzureADPreview

Importe Modulos
<br>Import-Module MSOnline

Conectar Modulo
<br>Connect-msolservice

Gerenciamento Domínios
#Criar um Domínio
<br>New-MsolDomain –Authentication Managed –Name filial36502.cf

#Consulta Domínio
<br>Get-MsolDomain
<br>Get-MsolDomain -Status Verified
<br>Get-MsolDomainVerificationDns –DomainName filial36502.cf –Mode DnsTxtRecord

#Remover Domínio
<br>Remove-MsolDomain -DomainName filial36502.cf
<br>Remove-MsolDomain -DomainName filial3652.cf -Force

Gerenciamento Usuários

#Consultar Usuários
<br>Get-MsolUser 
<br>Get-MsolUser | Select DisplayName, ObjectID, -Synchronized
<br>Get-MsolUser -UserPrincipalName "userxx@matriz365.cf" | Select DisplayName, FirstName, LastName, Department, WHENCREATED

#Criar Usuário
<br>New-MsolUser -UserPrincipalName "userxx@matriz365.cf" -DisplayName "User XX" -FirstName "Userxx" -LastName "XX"

#Modificar Usuário
<br>Set-MsolUser -UserPrincipalName "userxx@matriz365.cf" -DisplayName "David Chew" -FirstName "David" -LastName "Chew" -Department "Finance"

#Remover Usuário
<br>Remove-MsolUser -UserPrincipalName "userxx@matriz365.cf"
<br>Remove-MsolUser -UserPrincipalName "userxx@matriz365.cf" -Force

Gerenciamento Licenças

#Consultar Licenças
<br>Get-MsolAccountSku
<br>Get-MsolUser | Where-Object {($_.licenses).AccountSkuId -match "EnterprisePremium"}

Gerenciamento Grupos
#Consultar Grupos
<br>Get-MsolGroup

#Criar Grupos
<br>New-MsolGroup -DisplayName "MeuGrupo" -Description "Meu Grupo"

#Remover Grupos 
<br>Remove-MsolGroup -objectid 05bc8e6f-81a4-455d-af2c-70bd6358da16
