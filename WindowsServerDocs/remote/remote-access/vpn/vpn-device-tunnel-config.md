---
title: Configurer le tunnel d’appareil VPN dans Windows 10
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864550"
---
# <a name="configure-vpn-device-tunnels-in-windows-10"></a>Configurer des tunnels de périphérique VPN dans Windows 10

>S'applique à : Windows 10 version 1709

VPN Always On vous donne la possibilité de créer un profil VPN dédié pour le périphérique ou ordinateur. Les connexions VPN Always On incluent deux types de tunnels : 

- _Tunnel de l’appareil_ se connecte à des serveurs VPN spécifiés avant de connecter les utilisateurs à l’appareil. Scénarios de connectivité de préconnexion et à des fins de gestion de périphérique utilisent le tunnel de l’appareil.

- _Tunnel de l’utilisateur_ se connecte uniquement après un utilisateur se connecte à l’appareil. Tunnel de l’utilisateur permet aux utilisateurs d’accéder aux ressources de l’organisation via les serveurs VPN.

Contrairement aux _tunnel de l’utilisateur_, qui se connecte uniquement après un utilisateur se connecte à l’appareil ou l’ordinateur, _tunnel de l’appareil_ permet la connexion VPN établir la connectivité avant de l’utilisateur se connecte. Les deux _tunnel de l’appareil_ et _tunnel de l’utilisateur_ fonctionnent indépendamment avec leurs profils VPN, peuvent être connectés en même temps et pouvez utiliser différentes méthodes d’authentification et d’autres paramètres de configuration de VPN comme il convient. Tunnel de l’utilisateur prend en charge SSTP et IKEv2 et tunnel de l’appareil prend en charge IKEv2 uniquement avec aucune prise en charge de secours SSTP.

Tunnel de l’utilisateur est prise en charge sur joints au domaine, joint à un domaine (groupe de travail), ou appareils joint à un AD Azure pour autoriser enterprise et les scénarios BYOD. Il est disponible dans toutes les éditions de Windows et les fonctionnalités de plateforme sont disponibles à des tiers par le biais de prise en charge de plug-in UWP VPN.

Tunnel de l’appareil peut uniquement être configurée sur les appareils joints au domaine exécutant Windows 10 entreprise ou l’éducation version 1709 ou ultérieure. Il n’existe aucune prise en charge pour le contrôle tiers du tunnel de périphérique.


## <a name="device-tunnel-requirements-and-features"></a>Fonctionnalités et exigences de Tunnel de l’appareil
Vous devez activer l’authentification de certificat de machine pour les connexions VPN et définir une autorité de certification racine pour l’authentification des connexions VPN entrantes. 

```PowerShell
$VPNRootCertAuthority = “Common Name of trusted root certification authority”
$RootCACert = (Get-ChildItem -Path cert:LocalMachine\root | Where-Object {$_.Subject -Like “*$VPNRootCertAuthority*” })
Set-VpnAuthProtocol -UserAuthProtocolAccepted Certificate, EAP -RootCertificateNameToAccept $RootCACert -PassThru
```

![Exigences et les fonctionnalités de Tunnel d’appareil](../../media/device-tunnel-feature-and-requirements.png)

## <a name="vpn-device-tunnel-configuration"></a>Configuration de Tunnel du périphérique VPN

Le profil de l’exemple XML ci-dessous fournit de bons conseils pour les scénarios où le client uniquement lancée bénéficiant d’extractions sont requises via le tunnel de l’appareil.  Filtres de trafic sont optimisés pour restreindre le tunnel de l’appareil pour le trafic de gestion uniquement.  Cette configuration fonctionne bien pour mettre à jour de Windows, stratégie de groupe standard (GP) et scénarios de mise à jour de System Center Configuration Manager (SCCM), mais aussi connectivité VPN pour la première ouverture de session sans informations d’identification mises en cache, ou les scénarios de réinitialisation de mot de passe. 

Pour les cas de push occasionnés par le serveur, telles que la gestion à distance de Windows (WinRM), GPUpdate distant et scénarios de mise à jour à distance SCCM : vous devez autoriser le trafic entrant sur le tunnel de l’appareil, afin de filtres de trafic ne peut pas être utilisés.  Si dans le profil d’appareil tunnel vous activez des filtres de trafic, le Tunnel appareil refuse le trafic entrant.  Cette limitation sera supprimée dans les versions futures.


### <a name="sample-vpn-profilexml"></a>Exemple VPN profileXML

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

Selon les besoins de chaque scénario de déploiement spécifique, une autre fonctionnalité VPN peut être configurée avec le tunnel de l’appareil est [détection de réseau approuvé](https://social.technet.microsoft.com/wiki/contents/articles/38546.new-features-for-vpn-in-windows-10-and-windows-server-2016.aspx#Trusted_Network_Detection).

```
 <!-- inside/outside detection --> 
  <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection> 
```

## <a name="deployment-and-testing"></a>Déploiement et le test

Vous pouvez configurer des tunnels de périphérique en utilisant un script Windows PowerShell et à l’aide de l’Instrumentation de gestion Windows \(WMI\) pont. Le tunnel de périphérique VPN Always On doit être configuré dans le contexte de la **système LOCAL** compte. Pour ce faire, il sera nécessaire d’utiliser [PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec), l’un de le [PsTools](https://docs.microsoft.com/sysinternals/downloads/pstools) inclus dans le [Sysinternals](https://docs.microsoft.com/sysinternals/) suite d’utilitaires.

Pour obtenir des instructions sur la façon de déployer une par appareil `(.\Device)` et un par utilisateur `(.\User)` Profiler, consultez [PowerShell à l’aide de scripts avec le fournisseur de pont WMI](https://docs.microsoft.com/windows/client-management/mdm/using-powershell-scripting-with-the-wmi-bridge-provider). 

Exécutez la commande Windows PowerShell suivante pour vérifier que vous avez déployé un profil d’appareil :

    `Get-VpnConnection -AllUserConnection`

La sortie affiche une liste de l’appareil\-larges profils VPN déployés sur l’appareil.


### <a name="example-windows-powershell-script"></a>Exemple de Script Windows PowerShell

Vous pouvez utiliser le script Windows PowerShell suivant pour vous aider à créer votre propre script pour la création du profil.

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

## <a name="additional-resources"></a>Ressources complémentaires

Voici des ressources supplémentaires pour aider à votre déploiement de VPN.

### <a name="vpn-client-configuration-resources"></a>Ressources de configuration de client VPN

Il s’agit de ressources de configuration de client VPN.

- [Comment créer des profils VPN dans System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles)
- [Configurer les clients Windows 10 toujours sur les connexions VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [Options de profil VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options)

### <a name="remote-access-server-ras-gateway-resources"></a>Serveur d’accès à distance \(RAS\) ressources de passerelle

Voici les ressources de passerelle RAS.

- [Configurer RRAS avec un certificat d’authentification d’ordinateur](https://technet.microsoft.com/library/dd458982.aspx)
- [Résolution des problèmes de connexions VPN IKEv2](https://technet.microsoft.com/library/dd941612.aspx)
- [Configurer l’accès à distance basé sur le protocole IKEv2](https://technet.microsoft.com/library/ff687731.aspx)

>[!IMPORTANT]
>Lorsque vous utilisez le Tunnel de l’appareil avec une passerelle RAS de Microsoft, vous devez configurer le serveur RRAS pour prendre en charge l’authentification de certificat IKEv2 machine en activant le **autoriser l’authentification de certificat ordinateur pour IKEv2** méthode d’authentification comme décrit [ici](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee922682%28v=ws.10%29). Une fois que ce paramètre est activé, il est fortement recommandé que le **Set-VpnAuthProtocol** applet de commande PowerShell avec le **RootCertificateNameToAccept** paramètre facultatif, est utilisé pour vous assurer que Connexions RRAS IKEv2 sont uniquement autorisées pour les certificats de client VPN qui sont liés à une défini explicitement interne/privée autorité de Certification racine. Vous pouvez également le **Trusted Root Certification Authorities** store sur le serveur RRAS doit être modifié pour garantir qu’elle ne contient pas les autorités de certification publique, comme indiqué [ici](https://blogs.technet.microsoft.com/rrasblog/2009/06/10/what-type-of-certificate-to-install-on-the-vpn-server/). Méthodes similaires serez peut-être amené à prendre en compte pour les autres passerelles VPN.

---
