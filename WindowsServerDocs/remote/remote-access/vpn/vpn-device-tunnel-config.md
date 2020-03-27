---
title: Configurer le tunnel des périphériques VPN dans Windows 10
description: Découvrez comment créer un tunnel de périphérique VPN dans Windows 10.
ms.prod: windows-server
ms.date: 11/05/2018
ms.technology: networking-ras
ms.topic: article
ms.assetid: 158b7a62-2c52-448b-9467-c00d5018f65b
ms.author: lizross
author: eross-msft
ms.localizationpriority: medium
ms.openlocfilehash: ebf7a18c462909fa7b07b7c52b6e8a8083d0ab9b
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308070"
---
# <a name="configure-vpn-device-tunnels-in-windows-10"></a>Configurer des tunnels de périphériques VPN dans Windows 10

>S’applique à : Windows 10 version 1709

Always On VPN vous donne la possibilité de créer un profil VPN dédié pour un appareil ou un ordinateur. Always On les connexions VPN incluent deux types de tunnel : 

- Le _tunnel d’appareil_ se connecte aux serveurs VPN spécifiés avant que les utilisateurs se connectent à l’appareil. Les scénarios de connectivité avant connexion et la gestion des appareils utilisent le tunnel de l’appareil.

- Le _tunnel utilisateur_ se connecte uniquement lorsqu’un utilisateur se connecte à l’appareil. Le tunnel utilisateur permet aux utilisateurs d’accéder aux ressources de l’organisation via des serveurs VPN.

Contrairement au _tunnel utilisateur_, qui se connecte uniquement après qu’un utilisateur s’est connecté à l’appareil ou à l’ordinateur, le _tunnel d’appareil_ permet au VPN d’établir une connexion avant que l’utilisateur ouvre une session. Le tunnel d' _appareil_ et le _tunnel utilisateur_ fonctionnent indépendamment avec leurs profils VPN, peuvent être connectés en même temps et peuvent utiliser différentes méthodes d’authentification et d’autres paramètres de configuration VPN, le cas échéant. Le tunnel utilisateur prend en charge SSTP et IKEv2, et le tunnel d’appareil prend en charge IKEv2 uniquement sans prise en charge du secours SSTP.

Le tunnel utilisateur est pris en charge sur les appareils joints à un domaine, qui ne sont pas joints à un domaine (groupe de travail) ou Azure AD, pour permettre à la fois les scénarios entreprise et BYOD. Elle est disponible dans toutes les éditions de Windows et les fonctionnalités de la plateforme sont disponibles pour les tiers par le biais de la prise en charge du plug-in VPN UWP.

Le tunnel d’appareil ne peut être configuré que sur des appareils joints à un domaine exécutant Windows 10 entreprise ou Education version 1709 ou ultérieure. Il n’existe aucune prise en charge pour le contrôle tiers du tunnel de l’appareil.


## <a name="device-tunnel-requirements-and-features"></a>Spécifications et fonctionnalités du tunnel de périphérique
Vous devez activer l’authentification par certificat d’ordinateur pour les connexions VPN et définir une autorité de certification racine pour l’authentification des connexions VPN entrantes. 

```PowerShell
$VPNRootCertAuthority = “Common Name of trusted root certification authority”
$RootCACert = (Get-ChildItem -Path cert:LocalMachine\root | Where-Object {$_.Subject -Like “*$VPNRootCertAuthority*” })
Set-VpnAuthProtocol -UserAuthProtocolAccepted Certificate, EAP -RootCertificateNameToAccept $RootCACert -PassThru
```

![Fonctionnalités et configuration requise du tunnel d’appareil](../../media/device-tunnel-feature-and-requirements.png)

## <a name="vpn-device-tunnel-configuration"></a>Configuration du tunnel du périphérique VPN

L’exemple de code XML ci-dessous fournit de bons conseils pour les scénarios où seules les extractions lancées par le client sont requises sur le tunnel de l’appareil.  Les filtres de trafic sont exploités pour limiter le tunnel de l’appareil au trafic de gestion uniquement.  Cette configuration fonctionne bien pour les scénarios de mise à jour Windows Update, standard stratégie de groupe (GP) et Microsoft Endpoint Configuration Manager, ainsi que pour la connectivité VPN pour la première ouverture de session sans informations d’identification mises en cache ou pour les scénarios de réinitialisation de mot de passe. 

Pour les cas de Push initiés par le serveur, comme Windows Remote Management (WinRM), Remote GPUpdate et les scénarios de mise à jour des Configuration Manager distants : vous devez autoriser le trafic entrant sur le tunnel de l’appareil, de sorte que les filtres de trafic ne peuvent pas être utilisés.  Si, dans le profil de tunnel d’appareil, vous activez les filtres de trafic, le tunnel d’appareil rejette le trafic entrant.  Cette limitation sera supprimée dans les versions ultérieures.


### <a name="sample-vpn-profilexml"></a>Exemple de profileXML VPN

Voici l’exemple de profileXML VPN.

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

En fonction des besoins de chaque scénario de déploiement particulier, une autre fonctionnalité VPN qui peut être configurée avec le tunnel d’appareil est la [détection de réseau approuvé](https://social.technet.microsoft.com/wiki/contents/articles/38546.new-features-for-vpn-in-windows-10-and-windows-server-2016.aspx#Trusted_Network_Detection).

```
 <!-- inside/outside detection -->
  <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>
```

## <a name="deployment-and-testing"></a>Déploiement et test

Vous pouvez configurer des tunnels d’appareil à l’aide d’un script Windows PowerShell et du pont Windows Management Instrumentation (WMI). Le tunnel d’appareil VPN Always On doit être configuré dans le contexte du compte **système local** . Pour ce faire, il est nécessaire d’utiliser [PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec), l’un des [PsTools](https://docs.microsoft.com/sysinternals/downloads/pstools) inclus dans la suite d’utilitaires [Sysinternals](https://docs.microsoft.com/sysinternals/) .

Pour obtenir des instructions sur le déploiement d’un `(.\Device)` par appareil ou d’un profil de `(.\User)` par utilisateur, consultez [utilisation de scripts PowerShell avec le fournisseur de pont WMI](https://docs.microsoft.com/windows/client-management/mdm/using-powershell-scripting-with-the-wmi-bridge-provider).

Exécutez la commande Windows PowerShell suivante pour vérifier que vous avez correctement déployé un profil d’appareil :

  ```powershell
  Get-VpnConnection -AllUserConnection
  ```

La sortie affiche une liste de l’appareil\-les profils VPN étendus qui sont déployés sur l’appareil.

### <a name="example-windows-powershell-script"></a>Exemple de script Windows PowerShell

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

## <a name="additional-resources"></a>Ressources supplémentaires

Vous trouverez ci-dessous des ressources supplémentaires pour faciliter le déploiement de votre VPN.

### <a name="vpn-client-configuration-resources"></a>Ressources de configuration du client VPN

Les ressources de configuration de client VPN sont les suivantes.

- [Comment créer des profils VPN dans Configuration Manager](https://docs.microsoft.com/configmgr/protect/deploy-use/create-vpn-profiles)
- [Configurer le client Windows 10 Always On les connexions VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [Options de profil VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options)

### <a name="remote-access-server-gateway-resources"></a>Ressources de la passerelle du serveur d’accès à distance

Les ressources de passerelle du serveur d’accès à distance (RAS) sont les suivantes.

- [Configurer RRAS avec un certificat d’authentification d’ordinateur](https://technet.microsoft.com/library/dd458982.aspx)
- [Résolution des problèmes de connexions VPN IKEv2](https://technet.microsoft.com/library/dd941612.aspx)
- [Configurer l’accès à distance basé sur IKEv2](https://technet.microsoft.com/library/ff687731.aspx)

>[!IMPORTANT]
>Lorsque vous utilisez le tunnel d’appareil avec une passerelle RAS Microsoft, vous devez configurer le serveur RRAS pour prendre en charge l’authentification par certificat d’ordinateur IKEv2 en activant la méthode autoriser l’authentification par **certificat d’ordinateur pour** l’authentification IKEv2, comme décrit [ici](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee922682%28v=ws.10%29). Une fois ce paramètre activé, il est fortement recommandé que l’applet de commande PowerShell **Set-VpnAuthProtocol** , ainsi que le paramètre facultatif **RootCertificateNameToAccept** , permettent de s’assurer que les connexions IKEv2 RRAS sont uniquement autorisées pour les certificats clients VPN qui se lient à une autorité de certification racine interne/privée définie explicitement. Vous pouvez également modifier le magasin **autorités de certification racines de confiance** sur le serveur RRAS pour vous assurer qu’il ne contient pas d’autorités de certification publiques, comme indiqué [ici](https://blogs.technet.microsoft.com/rrasblog/2009/06/10/what-type-of-certificate-to-install-on-the-vpn-server/). Vous devrez peut-être également prendre en compte les méthodes similaires pour les autres passerelles VPN.

