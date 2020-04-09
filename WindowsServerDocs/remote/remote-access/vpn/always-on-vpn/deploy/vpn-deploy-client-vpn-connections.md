---
title: Configurer les connexions VPN Toujours actif (AlwaysOn) du client Windows 10
description: Au cours de cette étape, vous allez découvrir les options et le schéma ProfileXML et configurer les ordinateurs clients Windows 10 pour qu’ils communiquent avec cette infrastructure avec une connexion VPN.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.date: 05/29/2018
ms.assetid: d165822d-b65c-40a2-b440-af495ad22f42
ms.localizationpriority: medium
ms.author: v-tea
author: Teresa-MOTIV
ms.reviewer: deverette
ms.openlocfilehash: ecaf35f9cfe825d2bb617acbd9554bf1eb0eff1e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860502"
---
# <a name="step-6-configure-windows-10-client-always-on-vpn-connections"></a>Étape 6. Configurer le client Windows 10 Always On les connexions VPN

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Précédent :** Étape 5. Configurer les paramètres DNS et de pare-feu](vpn-deploy-dns-firewall.md)<br>
- [**Ensuite :** Étape 7. Facultatif Accès conditionnel pour la connectivité VPN à l’aide de Azure AD](../../ad-ca-vpn-connectivity-windows10.md)

Au cours de cette étape, vous allez découvrir les options et le schéma ProfileXML et configurer les ordinateurs clients Windows 10 pour qu’ils communiquent avec cette infrastructure avec une connexion VPN.

Vous pouvez configurer le client VPN Always On via PowerShell, le point de terminaison Microsoft Configuration Manager ou Intune. Les trois nécessitent un profil VPN XML pour configurer les paramètres VPN appropriés. L’automatisation de l’inscription PowerShell pour les organisations sans Configuration Manager ou Intune est possible.

>[!NOTE]
>Stratégie de groupe n’inclut pas de modèles d’administration pour configurer le client VPN Always On accès à distance Windows 10.  Toutefois, vous pouvez utiliser des scripts d’ouverture de session.

## <a name="profilexml-overview"></a>Présentation de ProfileXML

ProfileXML est un nœud URI au sein du CSP VPNv2. Plutôt que de configurer chaque nœud CSP VPNv2 individuellement, comme les déclencheurs, les listes d’itinéraires et les protocoles d’authentification, utilisez ce nœud pour configurer un client VPN Windows 10 en regroupant tous les paramètres en tant que bloc XML unique à un seul nœud CSP. Le schéma ProfileXML correspond presque exactement au schéma des nœuds CSP VPNv2, mais certains termes sont légèrement différents.

Vous utilisez ProfileXML dans toutes les méthodes de remise décrites dans ce déploiement, notamment Windows PowerShell, le point de terminaison Microsoft Configuration Manager et Intune. Il existe deux façons de configurer le nœud CSP ProfileXML VPNv2 dans ce déploiement :

- **OMA-DM**. L’une des méthodes consiste à utiliser un fournisseur MDM à l’aide de OMA-DM, comme expliqué précédemment dans la section [nœuds CSP VPNv2](../always-on-vpn-technology-overview.md#vpnv2-csp-nodes). À l’aide de cette méthode, vous pouvez facilement insérer le balisage XML de configuration du profil VPN dans le nœud CSP ProfileXML lors de l’utilisation d’Intune.

- **Pont Windows Management Instrumentation (WMI) vers CSP**. La deuxième méthode de configuration du nœud CSP ProfileXML consiste à utiliser le pont WMI-à-CSP (une classe WMI appelée **MDM_VPNv2_01**) qui peut accéder au fournisseur de services de chiffrement VPNv2 et au nœud ProfileXML. Lorsque vous créez une nouvelle instance de cette classe WMI, WMI utilise le CSP pour créer le profil VPN lors de l’utilisation de Windows PowerShell et Configuration Manager.

Bien que ces méthodes de configuration diffèrent, elles nécessitent toutes deux un profil VPN XML correctement mis en forme. Pour utiliser le paramètre CSP ProfileXML VPNv2, vous construisez du code XML à l’aide du schéma ProfileXML pour configurer les balises nécessaires au scénario de déploiement simple. Pour plus d’informations, consultez [PROFILEXML XSD](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-profile-xsd).

Vous trouverez ci-dessous chacun des paramètres requis et la balise ProfileXML correspondante. Vous configurez chaque paramètre dans une balise spécifique au sein du schéma ProfileXML, et tous les paramètres ne se trouvent pas dans le profil natif. Pour obtenir une balise de placement supplémentaire, consultez le schéma ProfileXML.

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

**Type de connexion :** IKEv2 natif

Élément ProfileXML :

```xml
<NativeProtocolType>IKEv2</NativeProtocolType>
```

**Routage :** Tunneling fractionné

Élément ProfileXML :

```xml
<RoutingPolicyType>SplitTunnel</RoutingPolicyType>
```

**Résolution de noms :** Liste d’informations de nom de domaine et suffixe DNS

Éléments ProfileXML :

```xml
<DomainNameInformation>
<DomainName>.corp.contoso.com</DomainName>
<DnsServers>10.10.1.10,10.10.1.50</DnsServers>
</DomainNameInformation>

<DnsSuffix>corp.contoso.com</DnsSuffix>
```

**Déclenchement en :** Always On et détection de réseau approuvé

Éléments ProfileXML :

```xml
<AlwaysOn>true</AlwaysOn>
<TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>
```

**Authentification :** PEAP-TLS avec certificats d’utilisateur protégés par le module de plateforme sécurisée

Éléments ProfileXML :

```xml
<Authentication>
<UserMethod>Eap</UserMethod>
<Eap>
<Configuration>...</Configuration>
</Eap>
</Authentication>
```

Vous pouvez utiliser des balises simples pour configurer des mécanismes d’authentification VPN. Toutefois, les protocoles EAP et PEAP sont plus impliqués. Le moyen le plus simple de créer le balisage XML consiste à configurer un client VPN avec ses paramètres EAP, puis à exporter cette configuration au format XML.

Pour plus d’informations sur les paramètres EAP, consultez [configuration EAP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/eap-configuration).

## <a name="manually-create-a-template-connection-profile"></a>Créer manuellement un profil de connexion de modèle

Dans cette étape, vous utilisez le protocole PEAP (Protected Extensible Authentication Protocol) pour sécuriser la communication entre le client et le serveur. Contrairement à un nom d’utilisateur et un mot de passe simples, cette connexion nécessite une section EAPConfiguration unique dans le profil VPN pour fonctionner.

Au lieu de décrire comment créer le balisage XML de toutes pièces, vous utilisez des paramètres dans Windows pour créer un profil VPN de modèle. Après avoir créé le profil VPN de modèle, vous utilisez Windows PowerShell pour utiliser la partie EAPConfiguration à partir de ce modèle pour créer le ProfileXML final que vous déployez ultérieurement dans le déploiement.

### <a name="record-nps-certificate-settings"></a>Enregistrer les paramètres de certificat NPS

Avant de créer le modèle, prenez note du nom d’hôte ou du nom de domaine complet (FQDN) du serveur NPS à partir du certificat du serveur et du nom de l’autorité de certification qui a émis le certificat.

**Procédures**

1. Sur votre serveur NPS, ouvrez le serveur de stratégie réseau.

2. Dans la console NPS, sous stratégies, cliquez sur **stratégies réseau**.

3. Cliquez avec le bouton droit sur **connexions de réseau privé virtuel (VPN)** , puis cliquez sur **Propriétés**.

4. Cliquez sur l’onglet **contraintes** , puis sur **méthodes d’authentification**.

5. Dans types EAP, cliquez sur **Microsoft : PEAP (Protected EAP)** , puis cliquez sur **modifier**.

6. Enregistrez les valeurs du **certificat émis à** et l' **émetteur**.

    Vous utilisez ces valeurs dans la configuration de modèle VPN à venir. Par exemple, si le nom de domaine complet du serveur est nps01.corp.contoso.com et que le nom d’hôte est NPS01, le nom du certificat est basé sur le nom de domaine complet ou le nom DNS du serveur, par exemple, nps01.corp.contoso.com.

7. Annulez la boîte de dialogue Modifier les propriétés EAP protégées.

8. Annulez la boîte de dialogue Propriétés des connexions de réseau privé virtuel (VPN).

9. Fermez le serveur NPS (Network Policy Server).

>[!NOTE]
>Si vous avez plusieurs serveurs NPS, effectuez les étapes suivantes sur chacun d’eux afin que le profil VPN puisse vérifier s’ils sont utilisés.

### <a name="configure-the-template-vpn-profile-on-a-domain-joined-client-computer"></a>Configurer le profil VPN de modèle sur un ordinateur client joint à un domaine

Maintenant que vous disposez des informations nécessaires, configurez le profil VPN de modèle sur un ordinateur client joint à un domaine. Le type de compte d’utilisateur que vous utilisez (c’est-à-dire utilisateur standard ou administrateur) pour cette partie du processus n’a pas d’importance.

Toutefois, si vous n’avez pas redémarré l’ordinateur depuis la configuration de l’inscription automatique des certificats, faites-le avant de configurer la connexion VPN du modèle pour vous assurer que vous disposez d’un certificat utilisable.

>[!NOTE]
>Il n’existe aucun moyen d’ajouter manuellement des propriétés avancées de VPN, telles que les règles NRPT, les Always On, la détection de réseau approuvé, etc. À l’étape suivante, vous créez une connexion VPN de test pour vérifier la configuration du serveur VPN et vous pouvez établir une connexion VPN au serveur.

**Créer manuellement une seule connexion VPN de test**

1.  Connectez-vous à un ordinateur client joint à un domaine en tant que membre du groupe **utilisateurs VPN** .

2.  Dans le menu Démarrer, tapez **VPN**, puis appuyez sur entrée.

3.  Dans le volet d’informations, cliquez sur **Ajouter une connexion VPN**.

4.  Dans la liste fournisseur VPN, cliquez sur **Windows (intégré)** .

5.  Dans nom de la connexion, tapez **template**.

6.  Dans nom ou adresse du serveur, tapez le nom de domaine complet (FQDN) **externe** de votre serveur VPN (par exemple, **VPN.contoso.com**).

7.  Cliquez sur **Enregistrer**.

8.  Sous paramètres associés, cliquez sur **modifier les options**de l’adaptateur.

9.  Cliquez avec le bouton droit sur **modèle**, puis cliquez sur **Propriétés**.

10. Sous l’onglet **sécurité** , dans **type de VPN**, cliquez sur **IKEv2**.

11. Dans chiffrement des données, cliquez sur **chiffrement de niveau maximal**.

12. Cliquez sur **utiliser le protocole EAP (Extensible Authentication Protocol)** . Ensuite, dans **utiliser le protocole EAP (Extensible Authentication Protocol)** , cliquez sur **Microsoft : PEAP (Protected EAP) (chiffrement activé)** .

13. Cliquez sur **Propriétés** pour ouvrir la boîte de dialogue Propriétés EAP protégées et procédez comme suit :

    a. Dans la zone **se connecter à ces serveurs** , tapez le nom du serveur NPS que vous avez récupéré à partir des paramètres d’authentification du serveur NPS plus haut dans cette section (par exemple, NPS01).

    >[!NOTE]
    >Le nom du serveur que vous tapez doit correspondre au nom figurant dans le certificat. Vous avez récupéré ce nom plus haut dans cette section. Si le nom ne correspond pas, la connexion échoue, indiquant que « la connexion a été empêchée en raison d’une stratégie configurée sur votre serveur RAS/VPN ».

    b.  Sous autorités de certification racines de confiance, sélectionnez l’autorité de certification racine qui a émis le certificat du serveur NPS (par exemple, contoso-CA).

    c.  Dans notifications avant la connexion, cliquez sur **ne pas demander à l’utilisateur d’autoriser de nouveaux serveurs ou des autorités de certification approuvées**.

    d.  Dans sélectionner la méthode d’authentification, cliquez sur **carte à puce ou autre certificat**, puis cliquez sur **configurer**. La boîte de dialogue Propriétés des cartes à puce ou des autres certificats s’ouvre.

    e.  Cliquez sur **utiliser un certificat sur cet ordinateur**.

    f.  Dans la zone se connecter à ces serveurs, entrez le nom du serveur NPS que vous avez récupéré à partir des paramètres d’authentification du serveur NPS au cours des étapes précédentes.

    g.  Sous autorités de certification racines de confiance, sélectionnez l’autorité de certification racine qui a émis le certificat du serveur NPS.

    h.  Activez la case à cocher **ne pas demander à l’utilisateur d’autoriser de nouveaux serveurs ou des autorités de certification approuvées** .

    i.  Cliquez sur **OK** pour fermer la boîte de dialogue Propriétés des cartes à puce ou des autres certificats.

    j.  Cliquez sur **OK** pour fermer la boîte de dialogue Propriétés EAP protégées.

14. Cliquez sur **OK** pour fermer la boîte de dialogue Propriétés du modèle.

15. Fermez la fenêtre Connexions réseau.

16. Dans paramètres, testez le VPN en cliquant sur **modèle**, puis en cliquant sur **se connecter**.

>[!IMPORTANT]
>Assurez-vous que la connexion VPN du modèle à votre serveur VPN est réussie. Cela garantit que les paramètres EAP sont corrects avant de les utiliser dans l’exemple suivant. Vous devez vous connecter au moins une fois avant de continuer ; dans le cas contraire, le profil ne contient pas toutes les informations nécessaires pour se connecter au VPN.

## <a name="create-the-profilexml-configuration-files"></a>Créer les fichiers de configuration ProfileXML

Avant de terminer cette section, assurez-vous que vous avez créé et testé la connexion VPN de modèle décrite dans la section [Création manuelle d’un profil de connexion de modèle](#manually-create-a-template-connection-profile) . Le test de la connexion VPN est nécessaire pour s’assurer que le profil contient toutes les informations requises pour se connecter au VPN.

Le script Windows PowerShell de la liste 1 crée deux fichiers sur le bureau, tous deux contenant des étiquettes **EAPConfiguration** basées sur le profil de connexion de modèle que vous avez créé précédemment :

- **VPN_Profile. Xml.** Ce fichier contient le balisage XML requis pour configurer le nœud ProfileXML dans le fournisseur de services de chiffrement VPNv2. Utilisez ce fichier avec des services MDM compatibles OMA-DM, tels qu’Intune.

- **VPN_Profile. ps1.** Ce fichier est un script Windows PowerShell que vous pouvez exécuter sur les ordinateurs clients pour configurer le nœud ProfileXML dans le fournisseur de services de chiffrement VPNv2. Vous pouvez également configurer le fournisseur de services de chiffrement en déployant ce script via Configuration Manager. Vous ne pouvez pas exécuter ce script dans une session de Bureau à distance, y compris une session améliorée Hyper-V.

>[!IMPORTANT]
>Les exemples de commandes ci-dessous nécessitent Windows 10 Build 1607 ou version ultérieure.

**Créer VPN_Profile. xml et VPN_Proflie. ps1**

1. Connectez-vous à l’ordinateur client joint au domaine contenant le profil VPN de modèle avec le même compte d’utilisateur que celui dans lequel la section [crée manuellement un profil de connexion de modèle](#manually-create-a-template-connection-profile) décrit.

2. Collez la liste 1 dans l’environnement d’écriture de scripts intégré de Windows PowerShell (ISE) et personnalisez les paramètres décrits dans les commentaires. Il s’agit des $Template, $ProfileName, $Servers, $DnsSuffix, $DomainName, $TrustedNetwork et $DNSServers. Une description complète de chaque paramètre se trouve dans les commentaires.

3. Exécutez le script pour générer **VPN_Profile. xml** et **VPN_Profile. ps1** sur le bureau.

#### <a name="listing-1-understanding-makeprofileps1"></a>Liste 1. Fonctionnement de MakeProfile. ps1

Cette section explique l’exemple de code que vous pouvez utiliser pour comprendre comment créer un profil VPN, en particulier pour configurer ProfileXML dans le fournisseur de services de chiffrement VPNv2.

Après avoir assemblé un script à partir de cet exemple de code et exécuté le script, le script génère deux fichiers : VPN_Profile. xml et VPN_Profile. ps1. Utilisez VPN_Profile. xml pour configurer ProfileXML dans les services de gestion des appareils mobiles compatibles OMA-DM, tels que Microsoft Intune.

Utilisez le script **VPN_Profile. ps1** dans Windows PowerShell ou Microsoft Endpoint Configuration Manager pour configurer ProfileXML sur le bureau Windows 10.

>[!NOTE]
>Pour afficher l’exemple de script complet, consultez la section [script complet MakeProfile. ps1](#makeprofileps1-full-script).

##### <a name="parameters"></a>Paramètres

Configurez les paramètres suivants :

**$Template**. Nom du modèle à partir duquel récupérer la configuration EAP.

**$ProfileName**. Identificateur alphanumérique unique pour le profil. Le nom du profil ne doit pas inclure une barre oblique (/). Si le nom du profil comporte un espace ou un autre caractère non alphanumérique, il doit être correctement placé dans une séquence d’échappement en fonction de la norme d’encodage URL.

**$Servers**. Adresse IP publique ou routable ou nom DNS pour la passerelle VPN. Il peut pointer vers l’adresse IP externe d’une passerelle ou d’une adresse IP virtuelle pour une batterie de serveurs. Exemples, 208.147.66.130 ou vpn.contoso.com.

**$DNSSuffix**. Spécifie un ou plusieurs espaces de noms DNS séparés par des virgules. La première dans la liste est également utilisée comme suffixe DNS spécifique à la connexion principale pour l’interface VPN. La liste entière est également ajoutée à SuffixSearchList.

**$DomainName**. Utilisé pour indiquer l’espace de noms auquel la stratégie s’applique. Quand une requête de nom est émise, le client DNS compare le nom de la requête à tous les espaces de noms sous DomainNameInformationList pour trouver une correspondance. Ce paramètre peut avoir l’un des types suivants :

- FQDN-nom de domaine complet
- Suffixe : suffixe de domaine qui sera ajouté à la requête ShortName pour la résolution DNS. Pour spécifier un suffixe, ajoutez un point (.) au suffixe DNS.

**$DNSServers**. Liste des adresses IP de serveur DNS séparées par des virgules à utiliser pour l’espace de noms.

**$TrustedNetwork**. Chaîne séparée par des virgules permettant d’identifier le réseau approuvé. Le VPN ne se connecte pas automatiquement lorsque l’utilisateur se trouve sur son réseau sans fil d’entreprise, où les ressources protégées sont directement accessibles à l’appareil.

Voici des exemples de valeurs pour les paramètres utilisés dans les commandes ci-dessous. Veillez à modifier ces valeurs pour votre environnement.

```xml
$TemplateName = 'Template'
$ProfileName = 'Contoso AlwaysOn VPN'
$Servers = 'vpn.contoso.com'
$DnsSuffix = 'corp.contoso.com'
$DomainName = '.corp.contoso.com'
$DNSServers = '10.10.0.2,10.10.0.3'
$TrustedNetwork = 'corp.contoso.com'
```

### <a name="prepare-and-create-the-profile-xml"></a>Préparer et créer le profil XML

Les exemples de commandes suivants obtiennent des paramètres EAP à partir du profil de modèle :

```xml
$Connection = Get-VpnConnection -Name $TemplateName
if(!$Connection)
{
$Message = "Unable to get $TemplateName connection profile: $_"
Write-Host "$Message"
exit
}
$EAPSettings= $Connection.EapConfigXmlStream.InnerXml
```

### <a name="create-the-profile-xml"></a>Créer le profil XML

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

```xml
$ProfileXML = @("
<VPNProfile>
  <DnsSuffix>$DnsSuffix</DnsSuffix>
  <NativeProfile>
<Servers>$Servers</Servers>
<NativeProtocolType>IKEv2</NativeProtocolType>
<Authentication>
  <UserMethod>Eap</UserMethod>
  <Eap>
    <Configuration>
     $EAPSettings
    </Configuration>
  </Eap>
</Authentication>
<RoutingPolicyType>SplitTunnel</RoutingPolicyType>
  </NativeProfile>
  <AlwaysOn>true</AlwaysOn>
  <RememberCredentials>true</RememberCredentials>
  <TrustedNetworkDetection>$TrustedNetwork</TrustedNetworkDetection>
  <DomainNameInformation>
<DomainName>$DomainName</DomainName>
<DnsServers>$DNSServers</DnsServers>
</DomainNameInformation>
</VPNProfile>
")
```

### <a name="output-vpn_profilexml-for-intune"></a>Sortie de VPN_Profile. xml pour Intune

Vous pouvez utiliser l’exemple de commande suivant pour enregistrer le fichier XML de profil :

```xml
$ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')
```

### <a name="output-vpn_profileps1-for-the-desktop-and-configuration-manager"></a>Sortie de VPN_Profile. ps1 pour le bureau et Configuration Manager

L’exemple de code suivant configure une connexion VPN IKEv2 AlwaysOn à l’aide du nœud ProfileXML dans le CSP VPNv2.

Vous pouvez utiliser ce script sur le bureau Windows 10 ou dans Configuration Manager.

### <a name="define-key-vpn-profile-parameters"></a>Définir les paramètres de profil VPN de clé

```xml
$Script = '$ProfileName = ''' + $ProfileName + ''''
$ProfileNameEscaped = $ProfileName -replace ' ', '%20'
```

### <a name="escape-special-characters-in-the-profile"></a>Caractères spéciaux d’échappement dans le profil

```xml
$ProfileXML = $ProfileXML -replace '<', '&lt;'
$ProfileXML = $ProfileXML -replace '>', '&gt;'
$ProfileXML = $ProfileXML -replace '"', '&quot;'
```

### <a name="define-wmi-to-csp-bridge-properties"></a>Définir les propriétés de pont WMI-à-CSP

```xml
$nodeCSPURI = "./Vendor/MSFT/VPNv2"
$namespaceName = "root\cimv2\mdm\dmmap"
$className = "MDM_VPNv2_01"
```

### <a name="determine-user-sid-for-vpn-profile"></a>Déterminer le SID de l’utilisateur pour le profil VPN :

```xml
try
{
$username = Gwmi -Class Win32_ComputerSystem | select username
$objuser = New-Object System.Security.Principal.NTAccount($username.username)
$sid = $objuser.Translate([System.Security.Principal.SecurityIdentifier])
$SidValue = $sid.Value
$Message = "User SID is $SidValue."
Write-Host "$Message"
}
catch [Exception]
{
$Message = "Unable to get user SID. User may be logged on over Remote Desktop: $_"
Write-Host "$Message"
exit
}
```

### <a name="define-wmi-session"></a>Définir la session WMI :

```xml
$session = New-CimSession
$options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
$options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Type", "PolicyPlatform_UserContext", $false)
$options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Id", "$SidValue", $false)
```

### <a name="detect-and-delete-previous-vpn-profile"></a>Détecter et supprimer le profil VPN précédent :

```xml
try
{
  $deleteInstances = $session.EnumerateInstances($namespaceName, $className, $options)
  foreach ($deleteInstance in $deleteInstances)
  {
    $InstanceId = $deleteInstance.InstanceID
    if ("$InstanceId" -eq "$ProfileNameEscaped")
    {
        $session.DeleteInstance($namespaceName, $deleteInstance, $options)
        $Message = "Removed $ProfileName profile $InstanceId"
        Write-Host "$Message"
    } else {
        $Message = "Ignoring existing VPN profile $InstanceId"
        Write-Host "$Message"
    }
  }
}
catch [Exception]
{
  $Message = "Unable to remove existing outdated instance(s) of $ProfileName profile: $_"
  Write-Host "$Message"
  exit
}
```

### <a name="create-the-vpn-profile"></a>Créez le profil VPN :

```xml
try
{
  $newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance $className, $namespaceName
  $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ParentID", "$nodeCSPURI", "String", "Key")
  $newInstance.CimInstanceProperties.Add($property)
  $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("InstanceID", "$ProfileNameEscaped", "String", "Key")
  $newInstance.CimInstanceProperties.Add($property)
  $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ProfileXML", "$ProfileXML", "String", "Property")
  $newInstance.CimInstanceProperties.Add($property)
  $session.CreateInstance($namespaceName, $newInstance, $options)
  $Message = "Created $ProfileName profile."


  Write-Host "$Message"
}
catch [Exception]
{
$Message = "Unable to create $ProfileName profile: $_"
Write-Host "$Message"
exit
}

$Message = "Script Complete"
Write-Host "$Message"
```

### <a name="save-the-profile-xml-file"></a>Enregistrer le fichier XML du profil

```xml
$Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')

$Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
Write-Host "$Message"
```

## <a name="makeprofileps1-full-script"></a>Script complet MakeProfile. ps1

La plupart des exemples utilisent l’applet de commande Windows PowerShell Set-WmiInstance pour insérer des ProfileXML dans une nouvelle instance de la classe MDM_VPNv2_01 WMI. 

Toutefois, cela ne fonctionne pas dans Configuration Manager, car vous ne pouvez pas exécuter le package dans le contexte de l’utilisateur final. Par conséquent, ce script utilise la Common Information Model pour créer une session WMI dans le contexte de l’utilisateur, puis crée une nouvelle instance de la classe WMI MDM_VPNv2_01 dans cette session. Cette classe WMI utilise le pont WMI-à-CSP pour configurer le CSP VPNv2. Par conséquent, en ajoutant l’instance de classe, vous configurez le fournisseur de services de chiffrement. 

>[!IMPORTANT]
>Le pont WMI-à-CSP requiert des droits d’administrateur local, par conception. Pour déployer des profils VPN par utilisateur, vous devez utiliser Configuration Manager ou MDM.

>[!NOTE]
>Le script VPN_Profile. ps1 utilise le SID de l’utilisateur actuel pour identifier le contexte de l’utilisateur. Étant donné qu’aucun SID n’est disponible dans une session Bureau à distance, le script ne fonctionne pas dans une session Bureau à distance. De même, il ne fonctionne pas dans une session améliorée Hyper-V. Si vous testez un accès à distance Always On VPN dans des ordinateurs virtuels, désactivez la session étendue sur vos machines virtuelles clientes avant d’exécuter ce script.

L’exemple de script suivant contient tous les exemples de code des sections précédentes. Veillez à modifier les valeurs d’exemple en fonction des valeurs appropriées pour votre environnement.
    
   ```makeProfile.ps1
    $TemplateName = 'Template'
    $ProfileName = 'Contoso AlwaysOn VPN'
    $Servers = 'vpn.contoso.com'
    $DnsSuffix = 'corp.contoso.com'
    $DomainName = '.corp.contoso.com'
    $DNSServers = '10.10.0.2,10.10.0.3'
    $TrustedNetwork = 'corp.contoso.com'

    
    $Connection = Get-VpnConnection -Name $TemplateName
    if(!$Connection)
    {
    $Message = "Unable to get $TemplateName connection profile: $_"
    Write-Host "$Message"
    exit
    }
    $EAPSettings= $Connection.EapConfigXmlStream.InnerXml
    
    $ProfileXML = @("
    <VPNProfile>
      <DnsSuffix>$DnsSuffix</DnsSuffix>
      <NativeProfile>
    <Servers>$Servers</Servers>
    <NativeProtocolType>IKEv2</NativeProtocolType>
    <Authentication>
      <UserMethod>Eap</UserMethod>
      <Eap>
       <Configuration>
        $EAPSettings
       </Configuration>
      </Eap>
    </Authentication>
    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>
      </NativeProfile>
    <AlwaysOn>true</AlwaysOn>
    <RememberCredentials>true</RememberCredentials>
    <TrustedNetworkDetection>$TrustedNetwork</TrustedNetworkDetection>
      <DomainNameInformation>
    <DomainName>$DomainName</DomainName>
    <DnsServers>$DNSServers</DnsServers>
    </DomainNameInformation>
    </VPNProfile>
    ")
    
    $ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')
    
     $Script = @("
      `$ProfileName = '$ProfileName'
      `$ProfileNameEscaped = `$ProfileName -replace ' ', '%20'
 
      `$ProfileXML = '$ProfileXML'
 
      `$ProfileXML = `$ProfileXML -replace '<', '&lt;'
      `$ProfileXML = `$ProfileXML -replace '>', '&gt;'
      `$ProfileXML = `$ProfileXML -replace '`"', '&quot;'
 
      `$nodeCSPURI = `"./Vendor/MSFT/VPNv2`"
      `$namespaceName = `"root\cimv2\mdm\dmmap`"
      `$className = `"MDM_VPNv2_01`"
 
      try
      {
      `$username = Gwmi -Class Win32_ComputerSystem | select username
      `$objuser = New-Object System.Security.Principal.NTAccount(`$username.username)
      `$sid = `$objuser.Translate([System.Security.Principal.SecurityIdentifier])
      `$SidValue = `$sid.Value
      `$Message = `"User SID is `$SidValue.`"
      Write-Host `"`$Message`"
      }
      catch [Exception]
      {
      `$Message = `"Unable to get user SID. User may be logged on over Remote Desktop: `$_`"
      Write-Host `"`$Message`"
      exit
      }
 
      `$session = New-CimSession
      `$options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
      `$options.SetCustomOption(`"PolicyPlatformContext_PrincipalContext_Type`", `"PolicyPlatform_UserContext`", `$false)
      `$options.SetCustomOption(`"PolicyPlatformContext_PrincipalContext_Id`", `"`$SidValue`", `$false)
 
      try
      {
     `$deleteInstances = `$session.EnumerateInstances(`$namespaceName, `$className, `$options)
     foreach (`$deleteInstance in `$deleteInstances)
     {
         `$InstanceId = `$deleteInstance.InstanceID
         if (`"`$InstanceId`" -eq `"`$ProfileNameEscaped`")
         {
             `$session.DeleteInstance(`$namespaceName, `$deleteInstance, `$options)
             `$Message = `"Removed `$ProfileName profile `$InstanceId`"
             Write-Host `"`$Message`"
         } else {
             `$Message = `"Ignoring existing VPN profile `$InstanceId`"
             Write-Host `"`$Message`"
         }
     }
      }
      catch [Exception]
      {
     `$Message = `"Unable to remove existing outdated instance(s) of `$ProfileName profile: `$_`"
     Write-Host `"`$Message`"
     exit
      }
 
      try
      {
     `$newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance `$className, `$namespaceName
     `$property = [Microsoft.Management.Infrastructure.CimProperty]::Create(`"ParentID`", `"`$nodeCSPURI`", `"String`", `"Key`")
     `$newInstance.CimInstanceProperties.Add(`$property)
     `$property = [Microsoft.Management.Infrastructure.CimProperty]::Create(`"InstanceID`", `"`$ProfileNameEscaped`", `"String`",      `"Key`")
     `$newInstance.CimInstanceProperties.Add(`$property)
     `$property = [Microsoft.Management.Infrastructure.CimProperty]::Create(`"ProfileXML`", `"`$ProfileXML`", `"String`", `"Property`")
     `$newInstance.CimInstanceProperties.Add(`$property)
     `$session.CreateInstance(`$namespaceName, `$newInstance, `$options)
     `$Message = `"Created `$ProfileName profile.`"

     Write-Host `"`$Message`"
      }
      catch [Exception]
      {
     `$Message = `"Unable to create `$ProfileName profile: `$_`"
     Write-Host `"`$Message`"
     exit
      }
 
      `$Message = `"Script Complete`"
      Write-Host `"`$Message`"
      ")
 
      $Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')
    
    $Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
    Write-Host "$Message"
   ```

## <a name="configure-the-vpn-client-by-using-windows-powershell"></a>Configurer le client VPN à l’aide de Windows PowerShell

Pour configurer le fournisseur de services de chiffrement VPNv2 sur un ordinateur client Windows 10, exécutez le script Windows PowerShell VPN_Profile. ps1 que vous avez créé dans la section [créer le profil XML](#create-the-profile-xml) . Ouvrez Windows PowerShell en tant qu’administrateur. dans le cas contraire, vous recevrez un message d’erreur indiquant que _l’accès_a été refusé.

Après avoir exécuté VPN_Profile. ps1 pour configurer le profil VPN, vous pouvez vérifier à tout moment qu’il a réussi en exécutant la commande suivante dans la Windows PowerShell ISE :

```powershell
Get-WmiObject -Namespace root\cimv2\mdm\dmmap -Class MDM_VPNv2_01
```

**Résultats obtenus à partir de l’applet de commande « obtenir-WmiObject »**

```powershell
__GENUS : 2
__CLASS : MDM_VPNv2_01
__SUPERCLASS:
__DYNASTY   : MDM_VPNv2_01
__RELPATH   : MDM_VPNv2_01.InstanceID="Contoso%20AlwaysOn%20VPN",ParentID
  ="./Vendor/MSFT/VPNv2"
__PROPERTY_COUNT: 10
__DERIVATION: {}
__SERVER: WIN01
__NAMESPACE : root\cimv2\mdm\dmmap
__PATH  : \\WIN01\root\cimv2\mdm\dmmap:MDM_VPNv2_01.InstanceID="Conto
  so%20AlwaysOn%20VPN",ParentID="./Vendor/MSFT/VPNv2"
AlwaysOn: True
ByPassForLocal  :
DnsSuffix   : corp.contoso.com
EdpModeId   :
InstanceID  : Contoso%20AlwaysOn%20VPN
LockDown:
ParentID: ./Vendor/MSFT/VPNv2
ProfileXML  : <VPNProfile><RememberCredentials>true</RememberCredentials>
  <AlwaysOn>true</AlwaysOn><DnsSuffix>corp.contoso.com</DnsSu
  ffix><TrustedNetworkDetection>corp.contoso.com</TrustedNetw
  orkDetection><NativeProfile><Servers>vpn.contoso.com;vpn.co
  ntoso.com</Servers><RoutingPolicyType>SplitTunnel</RoutingP
  olicyType><NativeProtocolType>Ikev2</NativeProtocolType><Au
  thentication><UserMethod>Eap</UserMethod><MachineMethod>Eap
  </MachineMethod><Eap><Configuration><EapHostConfig xmlns="h
  ttp://www.microsoft.com/provisioning/EapHostConfig"><EapMet
  hod><Type xmlns="https://www.microsoft.com/provisioning/EapC
  ommon">25</Type><VendorId xmlns="https://www.microsoft.com/p
  rovisioning/EapCommon">0</VendorId><VendorType xmlns="http:
  //www.microsoft.com/provisioning/EapCommon">0</VendorType><
  AuthorId xmlns="https://www.microsoft.com/provisioning/EapCo
  mmon">0</AuthorId></EapMethod><Config xmlns="https://www.mic
  rosoft.com/provisioning/EapHostConfig"><Eap xmlns="https://w
  ww.microsoft.com/provisioning/BaseEapConnectionPropertiesV1
  "><Type>25</Type><EapType xmlns="https://www.microsoft.com/p
  rovisioning/MsPeapConnectionPropertiesV1"><ServerValidation
  ><DisableUserPromptForServerValidation>true</DisableUserPro
  mptForServerValidation><ServerNames>NPS</ServerNames><Trust
  edRootCA>3f 07 88 e8 ac 00 32 e4 06 3f 30 f8 db 74 25 e1
  2e 5b 84 d1 </TrustedRootCA></ServerValidation><FastReconne
  ct>true</FastReconnect><InnerEapOptional>false</InnerEapOpt
  ional><Eap xmlns="https://www.microsoft.com/provisioning/Bas
  eEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="
  https://www.microsoft.com/provisioning/EapTlsConnectionPrope
  rtiesV1"><CredentialsSource><CertificateStore><SimpleCertSe
  lection>true</SimpleCertSelection></CertificateStore></Cred
  entialsSource><ServerValidation><DisableUserPromptForServer
  Validation>true</DisableUserPromptForServerValidation><Serv
  erNames>NPS</ServerNames><TrustedRootCA>3f 07 88 e8 ac 00
  32 e4 06 3f 30 f8 db 74 25 e1 2e 5b 84 d1 </TrustedRootCA><
  /ServerValidation><DifferentUsername>false</DifferentUserna
  me><PerformServerValidation xmlns="https://www.microsoft.com
  /provisioning/EapTlsConnectionPropertiesV2">true</PerformSe
  rverValidation><AcceptServerName xmlns="https://www.microsof
  t.com/provisioning/EapTlsConnectionPropertiesV2">true</Acce
  ptServerName></EapType></Eap><EnableQuarantineChecks>false<
  /EnableQuarantineChecks><RequireCryptoBinding>false</Requir
  eCryptoBinding><PeapExtensions><PerformServerValidation xml
  ns="https://www.microsoft.com/provisioning/MsPeapConnectionP
  ropertiesV2">true</PerformServerValidation><AcceptServerNam
  e xmlns="https://www.microsoft.com/provisioning/MsPeapConnec
  tionPropertiesV2">true</AcceptServerName></PeapExtensions><
  /EapType></Eap></Config></EapHostConfig></Configuration></E
  ap></Authentication></NativeProfile><DomainNameInformation>
  <DomainName>corp.contoso.com</DomainName><DnsServers>10.10.
      0.2,10.10.0.3</DnsServers><AutoTrigger>true</AutoTrigger></
  DomainNameInformation></VPNProfile>
RememberCredentials : True
TrustedNetworkDetection : corp.contoso.com
PSComputerName  : WIN01
```

La configuration de ProfileXML doit être correcte dans la structure, l’orthographe, la configuration et parfois lettre. Si vous voyez une autre structure dans la liste 1, le balisage ProfileXML contient probablement une erreur.

Si vous devez résoudre le balisage, il est plus facile de le placer dans un éditeur XML que de le résoudre dans le Windows PowerShell ISE. Dans les deux cas, démarrez avec la version la plus simple du profil, puis rajoutez les composants un par un jusqu’à ce que le problème se reproduise.

## <a name="configure-the-vpn-client-by-using-configuration-manager"></a>Configurer le client VPN à l’aide de Configuration Manager

Dans Configuration Manager, vous pouvez déployer des profils VPN à l’aide du nœud CSP ProfileXML, comme vous l’avez fait dans Windows PowerShell. Ici, vous utilisez le script Windows PowerShell VPN_Profile. ps1 que vous avez créé dans la section [créer les fichiers de configuration ProfileXML](#create-the-profilexml-configuration-files).

Pour utiliser Configuration Manager pour déployer un profil VPN Always On accès à distance sur des ordinateurs clients Windows 10, vous devez commencer par créer un groupe d’ordinateurs ou d’utilisateurs pour lesquels vous déployez le profil. Dans ce scénario, créez un groupe d’utilisateurs pour déployer le script de configuration.

### <a name="create-a-user-group"></a>Créer un groupe d'utilisateurs

1.  Dans la console Configuration Manager, ouvrez ressources et conformité\\regroupements d’utilisateurs.

2.  Dans le ruban d' **hébergement** , dans le groupe **créer** , cliquez sur créer un **regroupement d’utilisateurs**.

3.  Sur la page général, procédez comme suit :

    a. Dans **nom**, tapez **utilisateurs VPN**.

    b. Cliquez sur **Parcourir**, cliquez sur **tous les utilisateurs** , puis cliquez sur **OK**.

    c. Cliquez sur **Suivant**.

4.  Sur la page règles d’adhésion, procédez comme suit :

    a.  Dans **règles d’adhésion**, cliquez sur **Ajouter une règle**, puis sur **règle directe**. Dans cet exemple, vous ajoutez des utilisateurs individuels au regroupement d’utilisateurs. Toutefois, vous pouvez utiliser une règle de requête pour ajouter dynamiquement des utilisateurs à cette collection dans le cas d’un déploiement à grande échelle.

    b.  Dans la page **Bienvenue**, cliquez sur **Suivant**.

    c.  Dans la page Rechercher des ressources, dans **valeur**, tapez le nom de l’utilisateur que vous souhaitez ajouter. Le nom de la ressource comprend le domaine de l’utilisateur. Pour inclure des résultats en fonction d’une correspondance partielle, insérez le caractère **%** à chaque extrémité de votre critère de recherche. Par exemple, pour rechercher tous les utilisateurs contenant la chaîne « Lori », tapez **% Lori%** . Cliquez sur **Suivant**.

    d.  Dans la page Sélectionner les ressources, sélectionnez les utilisateurs que vous souhaitez ajouter au groupe, puis cliquez sur **suivant**.

    e.  Sur la page Résumé, cliquez sur **suivant**.

    f.  Dans la page dernière étape, cliquez sur **Fermer**.

6.  De retour sur la page règles d’adhésion de l’Assistant Création d’un regroupement d’utilisateurs, cliquez sur **suivant**.

7.  Sur la page Résumé, cliquez sur **suivant**.

8.  Dans la page dernière étape, cliquez sur **Fermer**.

Après avoir créé le groupe d’utilisateurs pour recevoir le profil VPN, vous pouvez créer un package et un programme pour déployer le script de configuration Windows PowerShell que vous avez créé dans la section [créer les fichiers de configuration ProfileXML](#create-the-profilexml-configuration-files).

### <a name="create-a-package-containing-the-profilexml-configuration-script"></a>Créer un package contenant le script de configuration ProfileXML

1.  Hébergez le script VPN_Profile. ps1 sur un partage réseau auquel le compte d’ordinateur du serveur de site peut accéder.

2.  Dans la console Configuration Manager, ouvrez **bibliothèque de logiciels\\packages de\\de gestion des applications**.

3.  Dans le ruban **Accueil** , dans le groupe **créer** , cliquez sur **créer un package** pour démarrer l’Assistant Création d’un package et d’un programme.

4.  Sur la page package, procédez comme suit :

    a. Dans **nom**, tapez **Windows 10 Always on le profil VPN**.

    b. Activez la case à cocher **ce package contient des fichiers sources** , puis cliquez sur **Parcourir**.

    c. Dans la boîte de dialogue définir le dossier source, cliquez sur **Parcourir**, sélectionnez le partage de fichiers contenant VPN_Profile. ps1, puis cliquez sur **OK**.
        Veillez à sélectionner un chemin d’accès réseau, et non un chemin d’accès local. En d’autres termes, le chemin d’accès doit ressembler à *\\fileserver\\vpnscript*, et non *c :\\vpnscript*.

1.  Cliquez sur **Suivant**.

2.  Sur la page type de programme, cliquez sur **suivant**.

3.  Sur la page programme standard, procédez comme suit :

    a.  Dans **nom**, tapez **script de profil VPN**.

    b.  Dans **ligne de commande**, tapez **PowerShell. exe-executionpolicy Bypass-file « VPN_Profile. ps1 »** .

    c.  En **mode exécution**, cliquez sur **exécuter avec les droits d’administration**.

    d.  Cliquez sur **Suivant**.

4.  Sur la page spécifications, procédez comme suit :

    a.  Sélectionnez **ce programme peut s’exécuter uniquement sur des plateformes spécifiées**.

    b.  Activez les cases à cocher **tous les Windows 10 (32 bits)** et **toutes les fenêtres windows 10 (64 bits)** .

    c.  Dans **espace disque estimé**, tapez **1**.

    d.  Dans **durée maximale d’exécution allouée (en minutes)** , tapez **15**.

    e.  Cliquez sur **Suivant**.

5.  Sur la page Résumé, cliquez sur **suivant**.

6.  Dans la page dernière étape, cliquez sur **Fermer**.

Après avoir créé le package et le programme, vous devez le déployer dans le groupe **utilisateurs VPN** .

### <a name="deploy-the-profilexml-configuration-script"></a>Déployer le script de configuration ProfileXML

1.  Dans la console Configuration Manager, ouvrez bibliothèque de logiciels\\packages de\\de gestion des applications.

2.  Dans **packages**, cliquez sur **Windows 10 Always on le profil VPN**.

3.  Dans l’onglet **programmes** , en bas du volet d’informations, cliquez avec le bouton droit sur **script de profil VPN**, cliquez sur **Propriétés**, puis procédez comme suit :

    a.  Dans l’onglet **avancé** , dans **lorsque ce programme est attribué à un ordinateur**, cliquez **une fois pour chaque utilisateur qui ouvre une session**.

    b.  Cliquez sur **OK**.

4.  Cliquez avec le bouton droit sur **script de profil VPN** , puis cliquez sur **déployer** pour démarrer l’Assistant déploiement logiciel.

5.  Sur la page général, procédez comme suit :

    a.  À côté de la **collection**, cliquez sur **Parcourir**.

    b.  Dans la liste **types de collections** (en haut à gauche), cliquez sur **regroupements d’utilisateurs**.

    c.  Cliquez sur **utilisateurs VPN**, puis sur **OK**.

    d.  Cliquez sur **Suivant**.

6.  Sur la page contenu, procédez comme suit :

    a.  Cliquez sur **Ajouter**, puis sur **point de distribution**.

    b.  Dans **points de distribution disponibles**, sélectionnez les points de distribution vers lesquels vous souhaitez distribuer le script de configuration ProfileXML, puis cliquez sur **OK**.

    c.  Cliquez sur **Suivant**.

7.  Sur la page Paramètres de déploiement, cliquez sur **suivant**.

8.  Sur la page planification, procédez comme suit :

    a.  Cliquez sur **nouveau** pour ouvrir la boîte de dialogue calendrier d’attribution.

    b.  Cliquez sur **affecter immédiatement après cet événement**, puis cliquez sur **OK**.

    c.  Cliquez sur **Suivant**.

9.  Sur la page expérience utilisateur, procédez comme suit :

    1.  Activez la case à cocher **installation de logiciel** .

    2.  Cliquez sur **Résumé**.

10. Sur la page Résumé, cliquez sur **suivant**.

11. Dans la page dernière étape, cliquez sur **Fermer**.

Une fois le script de configuration ProfileXML déployé, connectez-vous à un ordinateur client Windows 10 avec le compte d’utilisateur que vous avez sélectionné lors de la création du regroupement d’utilisateurs. Vérifiez la configuration du client VPN.

>[!NOTE]
>Le script VPN_Profile. ps1 ne fonctionne pas dans une session de Bureau à distance. De même, il ne fonctionne pas dans une session améliorée Hyper-V. Si vous testez un accès à distance Always On VPN dans des machines virtuelles, désactivez la session étendue sur vos machines virtuelles clientes avant de continuer.

### <a name="verify-the-configuration-of-the-vpn-client"></a>Vérifier la configuration du client VPN

1.  Dans le panneau de configuration, sous **System\\Security**, cliquez sur **Configuration Manager**. 

2.  Dans la boîte de dialogue Propriétés du Configuration Manager, sous l’onglet **actions** , procédez comme suit :

    a.  Cliquez sur **récupération de stratégie ordinateur & cycle d’évaluation**, cliquez sur **Exécuter maintenant**, puis sur **OK**.

    b.  Cliquez sur **récupération de stratégie utilisateur & cycle d’évaluation**, cliquez sur **Exécuter maintenant**, puis sur **OK**.

    c.  Cliquez sur **OK**.

3.  Fermez le panneau de configuration.

Le nouveau profil VPN doit bientôt s’afficher.

## <a name="configure-the-vpn-client-by-using-intune"></a>Configurer le client VPN à l’aide d’Intune

Pour utiliser Intune pour déployer l’accès à distance Windows 10 Always On les profils VPN, vous pouvez configurer le nœud CSP ProfileXML à l’aide du profil VPN que vous avez créé dans la section [créer les fichiers de configuration ProfileXML](#create-the-profilexml-configuration-files), ou vous pouvez utiliser l’exemple de base XML EAP fourni ci-dessous.

>[!NOTE]
>Intune utilise désormais des groupes de Azure AD. Si Azure AD Connect synchronisé le groupe d’utilisateurs VPN de l’emplacement local vers Azure AD, et que les utilisateurs sont affectés au groupe d’utilisateurs VPN, vous êtes prêt à continuer.

Créez la stratégie de configuration de périphérique VPN pour configurer les ordinateurs clients Windows 10 pour tous les utilisateurs ajoutés au groupe. Étant donné que le modèle Intune fournit des paramètres VPN, copiez uniquement les \<EapHostConfig > \</EapHostConfig > du fichier VPN_ProfileXML.

### <a name="create-the-always-on-vpn-configuration-policy"></a>Créer la stratégie de configuration Always On VPN

1.    Connectez-vous au [portail Azure](https://portal.azure.com/).

2.    Accédez à **Intune** > **configuration** de l’appareil > **profils**.

3.    Cliquez sur **créer un profil** pour démarrer l’Assistant Création d’un profil.

4.    Entrez un **nom** pour le profil VPN et (éventuellement) une description.

1.   Sous **plateforme**, sélectionnez **Windows 10 ou version ultérieure**, puis choisissez **VPN** dans la liste déroulante type de profil.

     >[!TIP]
     >Si vous créez un profileXML VPN personnalisé, consultez [appliquer profileXML à l’aide d’Intune](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-profile-options#apply-profilexml-using-intune) pour obtenir des instructions.

2. Sous l’onglet **VPN de base** , vérifiez ou définissez les paramètres suivants :

    - **Nom de la connexion :** Entrez le nom de la connexion VPN telle qu’elle apparaît sur l’ordinateur client sous l’onglet **VPN** sous **paramètres**, par exemple _contoso AutoVPN_.  
    
    - **Serveurs :** Ajoutez un ou plusieurs serveurs VPN en cliquant sur **Ajouter.**
    
    - **Description** et **adresse IP ou nom de domaine complet :** entrez la description et l’adresse IP ou le nom de domaine complet du serveur VPN. Ces valeurs doivent être alignées avec le nom du sujet dans le certificat d’authentification du serveur VPN. 
    
    - **Serveur par défaut :** S’il s’agit du serveur VPN par défaut, affectez la valeur **true**. Cela permet à ce serveur d’être le serveur par défaut que les appareils utilisent pour établir la connexion. 
    
    - **Type de connexion :** Défini sur **IKEv2**.  
    
    - **Always On :** Défini sur **activer** pour se connecter automatiquement au VPN à la connexion et rester connecté jusqu’à ce que l’utilisateur se déconnecte manuellement.
    
    - **Mémoriser les informations d’identification à chaque ouverture de session**: valeur booléenne (true ou false) pour la mise en cache des informations d’identification. Si la valeur est true, les informations d’identification sont mises en cache chaque fois que cela est possible.

3.  Copiez la chaîne XML suivante dans un éditeur de texte :
 
    [!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]
    
    
    ```XML
    <EapHostConfig xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><CredentialsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>false</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</AcceptServerName></EapType></Eap><EnableQuarantineChecks>false</EnableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</AcceptServerName></PeapExtensions></EapType></Eap></Config></EapHostConfig>
    ```

4.  Remplacez la **\<TrustedRootCA > 5a 89 Fe CB 5b 49 a7 0b 1a 52 63 b7 35 EE 1 1C c2 68 comme étant 4b <\/TrustedRootCA >** dans l’exemple avec l’empreinte numérique de certificat de votre autorité de certification racine locale aux deux emplacements.
  
    >[!Important]
    >N’utilisez pas l’exemple d’empreinte numérique dans la section \<TrustedRootCA >\</TrustedRootCA > ci-dessous.  TrustedRootCA doit être l’empreinte numérique du certificat de l’autorité de certification racine locale qui a émis le certificat d’authentification serveur pour les serveurs RRAS et NPS. **Il ne doit pas s’agir du certificat racine du Cloud, ni de l’empreinte numérique du certificat d’autorité de certification émettrice intermédiaire**.

5.  Remplacez la **\<servernames > NPS. contoso. com\</ServerNames >** dans l’exemple de code XML par le nom de domaine complet du serveur NPS joint au domaine dans lequel l’authentification a lieu. 

6.  Copiez la chaîne XML révisée et collez-la dans la zone **XML EAP** sous l’onglet VPN de base, puis cliquez sur **OK**.
    Une stratégie de configuration d’appareil Always On VPN utilisant EAP est créée dans Intune.


### <a name="sync-the-always-on-vpn-configuration-policy-with-intune"></a>Synchroniser la stratégie de configuration Always On VPN avec Intune

Pour tester la stratégie de configuration, connectez-vous à un ordinateur client Windows 10 en tant qu’utilisateur que vous avez ajouté au groupe **Always on les utilisateurs VPN** , puis synchronisez avec Intune.

1.  Dans le menu Démarrer, cliquez sur **paramètres**.

2.  Dans paramètres, cliquez sur **comptes**, puis sur **accès professionnel ou scolaire**.

3.  Cliquez sur le profil MDM, puis sur **info**.

4.  Cliquez sur **synchroniser** pour forcer une évaluation et une récupération de la stratégie Intune.

5.  Fermez paramètres. Après la synchronisation, vous voyez le profil VPN disponible sur l’ordinateur.

## <a name="next-steps"></a>Étapes suivantes :

Vous avez terminé le déploiement Always On VPN.  Pour les autres fonctionnalités que vous pouvez configurer, consultez le tableau ci-dessous :

|Si vous souhaitez...  |Alors consultez...  |
|---------|---------|
|Configurer l’accès conditionnel pour le VPN    |[Étape 7. Facultatif Configurer l’accès conditionnel pour la connectivité VPN à l’aide de Azure AD](../../ad-ca-vpn-connectivity-windows10.md): dans cette étape, vous pouvez ajuster la façon dont les utilisateurs VPN autorisés accèdent à vos ressources à l’aide de l' [accès conditionnel Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Avec Azure AD accès conditionnel pour la connectivité de réseau privé virtuel (VPN), vous pouvez protéger les connexions VPN. L’accès conditionnel est un moteur d’évaluation basé sur les stratégies qui vous permet de créer des règles d’accès pour n’importe quelle application connectée Azure Active Directory (Azure AD).         |
|En savoir plus sur les fonctionnalités VPN avancées  |[Fonctionnalités VPN avancées](always-on-vpn-adv-options.md#advanced-vpn-features): cette page fournit des conseils sur l’activation des filtres de trafic VPN, sur la configuration des connexions VPN automatiques à l’aide de déclencheurs d’application et sur la configuration de NPS pour autoriser uniquement les connexions VPN à partir de clients utilisant des certificats émis par Azure ad.        |
