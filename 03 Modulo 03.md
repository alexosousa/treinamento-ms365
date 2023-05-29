# Modulo 03 - Configurar Sincronismo Active Directory

#Links Documentações Ambiente Híbrido e Ad Connect <br>
<br>https://www.microsoft.com/pt-br/security/business/identity-access-management/azure-ad-pricing?rtc=1
<br>https://azure.microsoft.com/pt-br/pricing/details/active-directory-ds/
<br>https://azure.microsoft.com/pt-br/pricing/details/active-directory/external-identities/
<br>https://docs.microsoft.com/pt-br/azure/active-directory/hybrid/ 
<br>https://docs.microsoft.com/pt-br/azure/active-directory/cloud-sync/what-is-cloud-sync?toc=https%3A%2F%2Fdocs.microsoft.com%2Fpt-br%2Fazure%2Factive-directory%2Fcloud-sync%2Ftoc.json&bc=https%3A%2F%2Fdocs.microsoft.com%2Fpt-br%2Fazure%2Fbread%2Ftoc.json

#Identidades Híbridas Disponíveis

![image](https://user-images.githubusercontent.com/49683486/173201329-5281ab4d-9cb8-4795-a7ee-0ec07729cccc.png)

#Identidades Híbridas - Métodos de Autenticação

![image](https://user-images.githubusercontent.com/49683486/173201363-f0988e86-7054-4bd7-a719-f3be4b7f004b.png)

![image](https://user-images.githubusercontent.com/49683486/173201419-bc878509-1f29-4c5f-934d-84290724c4bd.png)

![image](https://user-images.githubusercontent.com/49683486/173201438-dae2cd72-afe8-407c-9804-4ff604f86182.png)

#Identidades Híbridas - Métodos de Login

![image](https://user-images.githubusercontent.com/49683486/173201477-104c47d4-f073-47e7-b7a2-74c30744b662.png)

# Topologia e Diferenças AD Connect e AD Connect Cloud Sync

![image](https://user-images.githubusercontent.com/49683486/173202407-500c1586-db51-41e3-9a8f-974587e78b81.png)

# Script para Criar Usuários no AD (copie o código até # fim e salve como .PS1) <br>
<br>#Importe Modulo
<br>Import-Module ActiveDirectory
#Senhas
$secpass = Read-Host "Password" -AsSecureString
<br>#Usuários
<br>New-ADUser -Name "Thiago Alves" -SamAccountName thiago.alves -UserPrincipalName "thiago.alves@totalnuvemtecnologia.com.br" -department "RH" -AccountPassword $secpass -Path "OU=Users,OU=Matriz,DC=totalnuvemtecnologia,DC=local" -Enabled:$true
<br>New-ADUser -Name "Thiago Moraes" -SamAccountName thiago.moraes -UserPrincipalName "thiago.moraes@totalnuvemtecnologia.com.br" -department "RH" -AccountPassword $secpass -Path "OU=Users,OU=Matriz,DC=totalnuvemtecnologia,DC=local" -Enabled:$true
<br>New-ADUser -Name "Jessica Simoes" -SamAccountName jessica.simoes -UserPrincipalName "jessica.simoes@totalnuvemtecnologia.com.br" -department "Administrativo" -AccountPassword $secpass -Path "OU=Users,OU=Matriz,DC=totalnuvemtecnologia,DC=local" -Enabled:$true
<br>New-ADUser -Name "Danilo Hora" -SamAccountName danilo.hora -UserPrincipalName "danilo.hora@totalnuvemtecnologia.com.br" -department "Financeiro"-AccountPassword $secpass -Path "OU=Users,OU=Matriz,DC=totalnuvemtecnologia,DC=local" -Enabled:$true
<br>New-ADUser -Name "Daniel Macedo" -SamAccountName daniel.macedo -UserPrincipalName "daniel.macedo@totalnuvemtecnologia.com.br" -department "TI"-AccountPassword $secpass -Path "OU=Users,OU=Matriz,DC=totalnuvemtecnologia,DC=local" -Enabled:$true
<br>New-ADUser -Name "Erick Hanada" -SamAccountName erick.hanada -UserPrincipalName "erick.hanada@totalnuvemtecnologia.com.br" -department "TI" -AccountPassword $secpass -Path "OU=Users,OU=Matriz,DC=totalnuvemtecnologia,DC=local" -Enabled:$true
<br>New-ADUser -Name "Jose Xavier" -SamAccountName jose.xavier -UserPrincipalName "jose.xavier@totalnuvemtecnologia.com.br" -department "Financeiro" -AccountPassword $secpass -Path "OU=Users,OU=Matriz,DC=totalnuvemtecnologia,DC=local" -Enabled:$true
<br>#Grupos
<br>New-ADGroup -Name "RH" -Path "OU=Groups,OU=Matriz,DC=totalnuvemtecnologia,DC=local" -GroupCategory Security -GroupScope Global 
<br>New-ADGroup -Name "Administrativo" -Path "OU=Groups,OU=Matriz,DC=totalnuvemtecnologia,DC=local" -GroupCategory Security -GroupScope Global 
<br>New-ADGroup -Name "Financeiro" -Path "OU=Groups,OU=Matriz,DC=totalnuvemtecnologia,DC=local" -GroupCategory Security -GroupScope Global 
<br>New-ADGroup -Name "TI" -Path "OU=Groups,OU=Matriz,DC=totalnuvemtecnologia,DC=local" -GroupCategory Security -GroupScope Global 
<br>#Permissões
<br>Add-AdGroupMember -Identity RH -Members thiago.alves, thiago.moraes
<br>Add-AdGroupMember -Identity Administrativo -Members jessica.simoes
<br>Add-AdGroupMember -Identity Financeiro -Members danilo.hora, jose.xavier
<br>Add-AdGroupMember -Identity TI -Members daniel.macedo, erick.hanada

#fim script

# Instalação e Configuração ADConnect Cloud Sync

<br>#passo 1
- usuário adconnect.cloud no ad como Domain Admin
- usuário adconnect.cloud no ms365 com Admin Global

<br>#passo 2
- review Agents
- New Configuration
- Configurar parametros de agent
- filtro de usuários = OU=Users,OU=Matriz,DC=totalnuvemtecnologia,DC=local
- validação usuários = CN=AD Connect Cloud,CN=Users,DC=totalnuvemtecnologia,DC=local 

# Script para modificar Alias no Active Directory

##Adição e Alteração

<br>#Principal<br>
$Users=Get-ADUser -SearchBase "OU=Users AD Connect,OU=Matriz,DC=totalnuvemtecnologia,DC=local" -filter * | Select SamAccountName, DistinguishedName
$NewEmail="@totalnuvemtecnologia.com.br"

Foreach ($User in $Users) { 

$NewSMTP= 'SMTP:'+$User.SamAccountName+$NewEmail

Set-ADUser -Identity $user.DistinguishedName -Add @{ProxyAddresses= $NewSMTP} -Server ad-01.totalnuvemtecnologia.local <br>
                          } <br>
<br>#Alias<br>
$Users=Get-ADUser -SearchBase "OU=Users AD Connect,OU=Matriz,DC=totalnuvemtecnologia,DC=local" -filter * | Select SamAccountName, DistinguishedName
$NewEmail="@totalnuvemtecnologia.com.br"

Foreach ($User in $Users) { 

$NewSMTP= 'smtp:'+$User.SamAccountName+$NewEmail

Set-ADUser -Identity $user.DistinguishedName -Add @{ProxyAddresses= $NewSMTP} -Server ad-01.totalnuvemtecnologia.local
                          }	  
<br>##Remover<br>
$Users=Get-ADUser -SearchBase "OU=Users AD Connect,OU=Matriz,DC=totalnuvemtecnologia,DC=local" -filter * | Select SamAccountName, DistinguishedName
$OldEmail="@totalnuvemtecnologia.com.br"

Foreach ($User in $Users) { 

$OldSMTP= 'smtp:'+$User.SamAccountName+$OldEmail

Set-ADUser -Identity $user.DistinguishedName -Remove @{ProxyAddresses= $OldSMTP} -Server AD-01.totalnuvemtecnologia.local
                           } <br>
<br>#fim

# Script para desativar e ativar serviço de sincronismo 

<br>#Desativar <br>
Import-Module MSOnline
Connect-MsolService
Set-MsolDirSyncEnabled –EnableDirSync $false

<br>#Ativar <br>
Import-Module MSOnline
Connect-MsolService
Set-MsolDirSyncEnabled –EnableDirSync $true

FIM
