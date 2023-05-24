# Modulo 02 - Gerenciamento Domínios, Usuários, Grupos, Recursos e Licenças.

# Licenciamento
![image](https://github.com/alexosousa/treinamento-ms365/assets/49683486/bc779219-01e8-4122-bc7b-59dc2555b3fb)

# Domínio
![image](https://github.com/alexosousa/treinamento-ms365/assets/49683486/3e8a8636-52aa-40e4-94db-2f031c07798c)

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

<br>#Registros DNS<br>
<br>Exchange Online
<br>MX
<br>br.com.empresa.mail.protection.outlook.com (priority 0) =>MX
<br>Autodiscover<br>
Autodiscover autodiscover.outlook.com =>CNAME
<br>SPF
<br> v=spf1 include:spf.protection.outlook.com –all =>TXT

<br>Teams
<br>Sip sipdir.online.lync.com (CNAME)
<br>Lyncdiscover webdir.online.lync.com (CNAME)
<br>Service, Port, Weight, Priority, Target
<br>_sip _tls 443 1 100 sipdir.online.lync.com
<br>_sipfederationtls _tcp 5061 1 100 sipfed.online.lync.com

<br>#Install Modules
<br>#Azure AD (AzureAD and MSOnline Module)
<br>Install-Module MSOnline -AllowClobber
<br>Install-Module AzureAD -AllowClobber
<br>Install-Module AzureADPreview -AllowClobber

<br>#Import Modules
<br>#Azure AD (AzureAD and MSOnline Module)
<br>Import-Module MSOnline
<br>Import-Module AzureAD
<br>Import-Module AzureADPreview

<br>#Conectar Modulo
<br>Connect-MsolService 
<br>Connect-MsolService -Credential $Credential 

<br>Gerenciamento Domínios
<br>#Criar um Domínio
<br>New-MsolDomain –Authentication Managed –Name empresa.com.br

<br>#Consulta Domínio
<br>Get-MsolDomain
<br>Get-MsolDomain -Status Verified
<br>Get-MsolDomainVerificationDns –DomainName empresa.com.br –Mode DnsTxtRecord

<br>#Remover Domínio
<br>Remove-MsolDomain -DomainName empresa.com.br
<br>Remove-MsolDomain -DomainName empresa.com.br -Force

<br>Gerenciamento Usuários

<br>#Consultar Usuários
<br>Get-MsolUser 
<br>Get-MsolUser | Select DisplayName, ObjectID, -Synchronized
<br>Get-MsolUser -UserPrincipalName "admin@tn0001.onmicrosoft.com" | Select DisplayName, FirstName, LastName, Department, WHENCREATED

<br>#Criar Usuário
<br>New-MsolUser -UserPrincipalName "user@tn0001.onmicrosoft.com" -DisplayName "User XX" -FirstName "Userxx" -LastName "XX"

<br>#Modificar Usuário
<br>Set-MsolUser -UserPrincipalName "user@tn0001.onmicrosoft.com" -DisplayName "David Chew" -FirstName "David" -LastName "Chew" -Department "Finance"

<br>#Remover Usuário
<br>Remove-MsolUser -UserPrincipalName "user@tn0001.onmicrosoft.com"
<br>Remove-MsolUser -UserPrincipalName "user@tn0001.onmicrosoft.com" -Force

<br>Gerenciamento Licenças

<br>#Consultar Licenças
<br>Get-MsolAccountSku
<br>Get-MsolUser | Where-Object {($_.licenses).AccountSkuId -match "SPB"} #EnterprisePremium

<br>Gerenciamento Grupos
<br>#Consultar Grupos
<br>Get-MsolGroup

<br>#Criar Grupos
<br>New-MsolGroup -DisplayName "MeuGrupo" -Description "Meu Grupo"

<br>#Remover Grupos 
<br>Remove-MsolGroup -objectid 05bc8e6f-81a4-455d-af2c-70bd6358da16 #exemplo
