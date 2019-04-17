---
title: Configurer le tunnel de périphérique VPN dans Windows 10
description: Découvrez comment créer un tunnel de périphérique VPN dans Windows 10.
ms.prod: windows-server-threshold
ms.date: 11/05/2018
ms.technology: networking-ras
ms.topic: article
ms.assetid: 158b7a62-2c52-448b-9467-c00d5018f65b
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: 005721873ad3a0df942bc9e23eba13728965ccba
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067124"
---
# Configurer des tunnels d’appareil VPN dans Windows 10

>S’applique à: Windows 10 version 1709

Toujours actif (AlwaysOn) vous donne la possibilité de créer un profil VPN dédié pour appareil ou un ordinateur. Connexions toujours actif (AlwaysOn) sont les deux types de tunnels suivantes: 

- _Tunnel de périphérique_ se connecte à des serveurs VPN spécifiés avant ouvrez une session les utilisateurs sur l’appareil. Scénarios de connexion avant ouverture de session et à des fins de gestion de périphérique utilisent tunnel de périphérique.

- _Tunnel de l’utilisateur_ se connecte uniquement après un utilisateur se connecte à l’appareil. Tunnel utilisateur permet aux utilisateurs d’accéder aux ressources de l’organisation par le biais de serveurs VPN.

Contrairement aux _tunnel utilisateur_, ce qui se connecte uniquement après un utilisateur se connecte à l’appareil ou l’ordinateur, _tunnel de périphérique_ permettant la connexion VPN établir la connectivité avant que l’utilisateur ouvre une session. _Tunnel de périphérique_ et de _tunnel utilisateur_ fonctionnent indépendamment avec leurs profils VPN, peuvent être connectés en même temps et peuvent utiliser différentes méthodes d’authentification et autres paramètres de configuration VPN selon le cas. Tunnel utilisateur prend en charge SSTP et IKEv2 et tunnel de périphérique prend en charge IKEv2 uniquement avec aucune prise en charge de secours SSTP.

Tunnel utilisateur est pris en charge sur joints au domaine, joints à un domaine (groupe de travail), ou à Azure AD – pour autoriser les scénarios entreprise et BYOD. Il est disponible dans toutes les éditions de Windows et les fonctionnalités de la plateforme sont disponibles à des tiers par le biais prise en charge du plug-in VPN UWP.

Tunnel de périphérique peut uniquement être configuré sur les appareils joints au domaine exécutant Windows 10 entreprise ou éducation version 1709 ou ultérieure. Il n’existe aucune prise en charge pour un contrôle tiers du tunnel de périphérique.


## Fonctionnalités et les exigences de Tunnel de périphérique
Vous devez activer l’authentification par certificat de machine pour les connexions VPN et définir une autorité de certification racine pour authentifier les connexions VPN entrantes. 

```PowerShell
$VPNRootCertAuthority = “Common Name of trusted root certification authority”
$RootCACert = (Get-ChildItem -Path cert:LocalMachine\root | Where-Object {$_.Subject -Like “*$VPNRootCertAuthority*” })
Set-VpnAuthProtocol -UserAuthProtocolAccepted Certificate, EAP -RootCertificateNameToAccept $RootCACert -PassThru
```

![Configuration requise et les fonctionnalités de Tunnel de périphérique](../../media/device-tunnel-feature-and-requirements.png)

## Configuration de Tunnel de périphérique VPN

L’exemple de profil XML ci-dessous fournit des conseils bon pour les scénarios où uniquement client initié attire sont nécessaires via le tunnel de périphérique.  Filtres de trafic sont optimisés pour restreindre le tunnel de périphérique pour le trafic de gestion uniquement.  Cette configuration fonctionne bien pour mettre à jour de Windows, la stratégie de groupe standard (GP) et scénarios de mise à jour de System Center Configuration Manager (SCCM), mais aussi une connectivité VPN pour la première ouverture de session sans les informations d’identification mises en cache, ou des scénarios de réinitialisation de mot de passe. 

Pour les cas push exécutée par le serveur, comme Windows Remote Management (WinRM), GPUpdate à distance et scénarios de mise à jour à distance SCCM –, vous devez autoriser le trafic entrant sur le tunnel de périphérique, afin que les filtres de trafic ne peuvent pas être utilisés.  Si dans le profil de tunnel de périphérique, vous activez des filtres de trafic, puis le Tunnel de périphérique n’autorise le trafic entrant.  Cette limitation va être supprimée dans les futures versions.


### Exemple VPN profileXML

Voici le profileXML VPN exemple.

``` xml
<VPNProfile>  
  <NativeProfile>  
<Servers>vpn.contoso.com</Servers>  
<NativeProtocolType>IKEv2</NativeProtocolType>  
<Authentication>  
  <MachineMethod>Certificate</MachineMethod>  
</Authentication>  
<RoutingPolicyType>SplitTunnel</RoutingPolicyType>  
 <!-- disable the addition of a class based route for the assigned IP address on the VPN interface -->
<DisableClassBasedDefaultRoute>true</DisableClassBasedDefaultRoute>  
  </NativeProfile> 
  <!-- use host routes(/32) to prevent routing conflicts -->  
  <Route>  
<Address>10.10.0.2</Address>  
<PrefixSize>32</PrefixSize>  
  </Route>  
  <Route>  
<Address>10.10.0.3</Address>  
<PrefixSize>32</PrefixSize>  
  </Route>  
<!-- traffic filters for the routes specified above so that only this traffic can go over the device tunnel --> 
  <TrafficFilter>  
<RemoteAddressRanges>10.10.0.2, 10.10.0.3</RemoteAddressRanges>  
  </TrafficFilter>
<!-- need to specify always on = true --> 
  <AlwaysOn>true</AlwaysOn> 
<!-- new node to specify that this is a device tunnel -->  
 <DeviceTunnel>true</DeviceTunnel>
<!--new node to register client IP address in DNS to enable manage out -->
<RegisterDNS>true</RegisterDNS>
</VPNProfile>
```

En fonction des besoins de chaque scénario de déploiement particulier, une autre fonctionnalité VPN qui peut être configurée avec le tunnel de périphérique est [Détection de réseaux approuvés](https://social.technet.microsoft.com/wiki/contents/articles/38546.new-features-for-vpn-in-windows-10-and-windows-server-2016.aspx#Trusted_Network_Detection).

```
 <!-- inside/outside detection --> 
  <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection> 
```

## Déploiement et test

Vous pouvez configurer des tunnels d’appareil en utilisant un script Windows PowerShell et en utilisant le pont Windows Management Instrumentation \(WMI\). Le tunnel de périphérique toujours actif (AlwaysOn) doit être configuré dans le contexte du compte **Système LOCAL** . Pour ce faire, il sera nécessaire d’utiliser [PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec), un des [PsTools](https://docs.microsoft.com/sysinternals/downloads/pstools) inclus dans la suite de [Sysinternals](https://docs.microsoft.com/sysinternals/) d’utilitaires.

Pour connaître les instructions sur la façon de déployer un par appareil `(.\Device)` ou un par utilisateur `(.\User)` de profil, consultez [l’écriture de scripts à l’aide de PowerShell avec le fournisseur de pont WMI](https://docs.microsoft.com/windows/client-management/mdm/using-powershell-scripting-with-the-wmi-bridge-provider). 

Exécutez la commande Windows PowerShell suivante pour vérifier que vous avez déployé avec succès un profil de périphérique:

    `Get-VpnConnection -AllUserConnection`

La sortie affiche une liste des profils VPN device\ à l’échelle qui sont déployées sur l’appareil.


### Exemple de Script de Windows PowerShell

Vous pouvez utiliser le script Windows PowerShell suivant pour vous aider à créer votre propre script pour la création de profil.

```PowerShell
Param(
[string]$xmlFilePath,
[string]$ProfileName
)

$a = Test-Path $xmlFilePath
echo $a

$ProfileXML = Get-Content $xmlFilePath

echo $XML

$ProfileNameEscaped = $ProfileName -replace ' ', '%20'

$Version = 201606090004

$ProfileXML = $ProfileXML -replace '<', '&lt;'
$ProfileXML = $ProfileXML -replace '>', '&gt;'
$ProfileXML = $ProfileXML -replace '"', '&quot;'

$nodeCSPURI = './Vendor/MSFT/VPNv2'
$namespaceName = "root\cimv2\mdm\dmmap"
$className = "MDM_VPNv2_01"

$session = New-CimSession

try
{
$newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance $className, $namespaceName
$property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ParentID", "$nodeCSPURI", 'String', 'Key')
$newInstance.CimInstanceProperties.Add($property)
$property = [Microsoft.Management.Infrastructure.CimProperty]::Create("InstanceID", "$ProfileNameEscaped", 'String', 'Key')
$newInstance.CimInstanceProperties.Add($property)
$property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ProfileXML", "$ProfileXML", 'String', 'Property')
$newInstance.CimInstanceProperties.Add($property)

$session.CreateInstance($namespaceName, $newInstance)
$Message = "Created $ProfileName profile."
Write-Host "$Message"
}
catch [Exception]
{
$Message = "Unable to create $ProfileName profile: $_"
Write-Host "$Message"
exit
}
$Message = "Complete."
Write-Host "$Message"
```

## Ressources complémentaires

Voici des ressources supplémentaires pour aider à votre déploiement d’un VPN.

### Ressources de configuration de client VPN

Il s’agit de ressources de configuration de client VPN.

- [Comment les profils VPN créer dans System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles)
- [Configurer les connexions VPN Toujours actif (AlwaysOn) du client Windows10](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [Options de profilVPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options)

### Les \(RAS\) serveur d’accès à distance des ressources de passerelle

Voici des ressources de la passerelle RAS.

- [Configurer RRAS avec un certificat d’authentification ordinateur](https://technet.microsoft.com/library/dd458982.aspx)
- [Les connexions VPN IKEv2 de résolution des problèmes](https://technet.microsoft.com/library/dd941612.aspx)
- [Configurer l’accès à distance basé sur le protocole IKEv2](https://technet.microsoft.com/library/ff687731.aspx)

>[!IMPORTANT]
>Lorsque vous utilisez la fonction Tunnel de périphérique avec une passerelle Microsoft RAS, vous devez configurer le serveur RRAS pour prendre en charge l’authentification par certificat IKEv2 ordinateur en activant la méthode d’authentification **authentification de certificat d’ordinateur autoriser pour IKEv2** comme décrit [ici](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee922682%28v=ws.10%29). Une fois que ce paramètre est activé, il est fortement recommandé que l’applet de commande **Set-VpnAuthProtocol** PowerShell, ainsi que le paramètre facultatif **RootCertificateNameToAccept** , est utilisé pour vous assurer que les connexions RRAS IKEv2 sont uniquement autorisées pour Cette chaîne pour une explicitement définie interne/privée autorité de Certification racine de certificats client VPN. Par ailleurs, le magasin **d’Autorités de Certification racine de confiance** sur le serveur RRAS doit être modifié pour vous assurer qu’il ne contient pas les autorités de certification publique comme nous l’avons expliqué [ici](https://blogs.technet.microsoft.com/rrasblog/2009/06/10/what-type-of-certificate-to-install-on-the-vpn-server/). Méthodes similaires devrez également à prendre en considération pour les autres passerelles VPN.

---
