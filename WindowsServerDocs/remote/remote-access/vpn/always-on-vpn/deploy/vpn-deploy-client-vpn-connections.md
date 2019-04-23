---
title: Configurer les connexions VPN Toujours actif (AlwaysOn) du client Windows 10
description: Dans cette étape, vous en savoir plus sur les options de ProfileXML et le schéma et configurez les ordinateurs clients Windows 10 pour communiquer avec cette infrastructure avec une connexion VPN.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 05/29/2018
ms.assetid: d165822d-b65c-40a2-b440-af495ad22f42
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.reviewer: deverette
ms.openlocfilehash: 6b383076686092e20448977bed3766f7d7d1c2b8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865890"
---
# <a name="step-6-configure-windows-10-client-always-on-vpn-connections"></a>Étape 6. Configurer des connexions VPN Always On de client Windows 10

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10


&#171;  [**Précédent :** Étape 5. Configurer les paramètres de pare-feu et de DNS](vpn-deploy-dns-firewall.md)<br>
&#187;[ **Suivant :** Étape 7. (Facultatif) Accès conditionnel pour la connectivité VPN à l’aide d’Azure AD](../../ad-ca-vpn-connectivity-windows10.md)

Dans cette étape, vous en savoir plus sur les options de ProfileXML et le schéma et configurez les ordinateurs clients Windows 10 pour communiquer avec cette infrastructure avec une connexion VPN. 

Vous pouvez configurer le client VPN Always On dans PowerShell, SCCM ou Intune. Les trois nécessitent un profil VPN XML pour configurer les paramètres VPN appropriés. Automatisation PowerShell l’inscription pour les organisations sans SCCM ou Intune est possible.

>[!NOTE]
>Stratégie de groupe n’inclut pas les modèles d’administration pour configurer le client Windows 10 distant accès VPN Always On.  Toutefois, vous pouvez utiliser les scripts d’ouverture de session.

## <a name="profilexml-overview"></a>Vue d’ensemble de ProfileXML

ProfileXML est un nœud de l’URI dans le VPNv2 CSP. Plutôt que de configurer chaque nœud VPNv2 CSP individuellement, tels que des déclencheurs, router des protocoles d’authentification et des listes, utilisez ce nœud pour configurer un client VPN Windows 10 en fournissant tous les paramètres en tant qu’un seul bloc XML à un seul nœud CSP. Le schéma ProfileXML correspond au schéma des nœuds VPNv2 CSP presque identique, mais certains termes sont légèrement différentes.

Vous utilisez ProfileXML dans toutes les méthodes de remise que décrit par ce déploiement, y compris Windows PowerShell, System Center Configuration Manager et Intune. Il existe deux façons de configurer le nœud ProfileXML VPNv2 CSP dans ce déploiement :

- **OMA-DM**. Une consiste à utiliser un fournisseur de gestion des appareils mobiles à l’aide d’OMA-DM, comme expliqué précédemment dans la section [les nœuds VPNv2 CSP](../always-on-vpn-technology-overview.md#vpnv2-csp-nodes). À l’aide de cette méthode, vous pouvez facilement insérer le balisage XML de configuration de profil VPN dans le nœud ProfileXML CSP lors de l’utilisation d’Intune.

- **Windows Management Instrumentation (WMI) - to - pont CSP**. La deuxième méthode de configuration du nœud ProfileXML CSP consiste à utiliser le pont WMI-CSP, une classe WMI appelée **MDM_VPNv2_01**— ayant accès à la VPNv2 CSP et le nœud ProfileXML. Lorsque vous créez une nouvelle instance de cette classe WMI, WMI utilise le CSP pour créer le profil VPN lors de l’utilisation de Windows PowerShell et System Center Configuration Manager.

Bien que ces méthodes de configuration diffèrent, les deux nécessitent un profil VPN XML correctement mis en forme. Pour utiliser le paramètre ProfileXML VPNv2 CSP, vous construisez un document XML en utilisant le schéma de ProfileXML pour configurer les balises nécessaires pour le scénario de déploiement simple. Pour plus d’informations, consultez [ProfileXML XSD](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-profile-xsd).

Vous trouverez ci-après chacun des paramètres requis et sa balise ProfileXML correspondante. Vous configurez chaque paramètre dans une balise spécifique au sein du schéma ProfileXML, et certaines d'entre elles sont trouvent sous le profil natif. Pour la sélection élective de balise supplémentaire, consultez le schéma ProfileXML.

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

**Type de connexion :** IKEv2 natif

Élément de ProfileXML :

    <NativeProtocolType>IKEv2</NativeProtocolType>

**Routage :** Le tunneling fractionné

Élément de ProfileXML :

    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>

**Résolution de noms :** Liste d’informations de nom de domaine et le suffixe DNS

Éléments de ProfileXML :

    <DomainNameInformation>
    <DomainName>.corp.contoso.com</DomainName>
    <DnsServers>10.10.1.10,10.10.1.50</DnsServers>
    </DomainNameInformation>
    
    <DnsSuffix>corp.contoso.com</DnsSuffix>


**Déclenchement :** Détection de réseau Always On et approuvé

Éléments de ProfileXML :

    <AlwaysOn>true</AlwaysOn>
    <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>


**Authentification :** PEAP-TLS avec des certificats utilisateur protégé par le module de plateforme sécurisée

Éléments de ProfileXML :

    <Authentication>
    <UserMethod>Eap</UserMethod>
    <Eap>
    <Configuration>...</Configuration>
    </Eap>
    </Authentication>

Vous pouvez utiliser des balises simples à configurer certains mécanismes d’authentification VPN. Toutefois, PEAP et EAP sont plus complexe. Le moyen le plus simple de créer le balisage XML consiste à configurer un client VPN avec ses paramètres EAP, puis exporter cette configuration au format XML. 

Pour plus d’informations sur les paramètres du protocole EAP, consultez [configuration EAP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/eap-configuration). 

## <a name="bkmk_profile"></a>Créer manuellement un profil de connexion de modèle

Dans cette étape, vous utilisez Protected Extensible Authentication Protocol \(PEAP\) pour sécuriser la communication entre le client et le serveur. Contrairement à un simple nom d’utilisateur et un mot de passe, cette connexion requiert une section EAPConfiguration unique dans le profil VPN fonctionne. 

Au lieu de décrire comment créer le balisage XML à partir de zéro, vous utilisez des paramètres dans Windows pour créer un modèle de profil VPN. Après avoir créé le modèle de profil VPN, vous utilisez Windows PowerShell pour consommer la partie EAPConfiguration à partir de ce modèle pour créer le ProfileXML final que vous déployez ultérieurement dans le déploiement.

### <a name="record-nps-certificate-settings"></a>Enregistrer les paramètres de certificat de serveur NPS

Avant de créer le modèle, prenez note du nom d’hôte ou le nom de domaine complet (FQDN) du serveur NPS à partir du certificat du serveur et le nom de l’autorité de certification qui a émis le certificat.

**Procédure :**

1.  Sur votre serveur NPS, ouvrez le serveur NPS.

2.  Dans la console NPS, sous stratégies, cliquez sur **stratégies réseau**.

3.  Avec le bouton droit **réseau privé virtuel \(VPN\) connexions**, puis cliquez sur **propriétés**.

4.  Cliquez sur le **contraintes** onglet, puis cliquez sur **méthodes d’authentification**.

5.  Dans les Types EAP, cliquez sur **Microsoft : PEAP (Protected EAP)**, puis cliquez sur **modifier**.

6.  Enregistrez les valeurs pour **certificat délivré à** et **émetteur**.<p>Vous utilisez ces valeurs dans la configuration du modèle VPN à venir. Par exemple, si le nom de domaine complet du serveur est nps01.corp.contoso.com et le nom d’hôte est NPS01, le nom du certificat repose sur le nom DNS ou nom de domaine complet du serveur - par exemple, nps01.corp.contoso.com.

7.  Annuler la boîte de dialogue Modifier les propriétés EAP protégées.

8.  Annuler la boîte de dialogue Propriétés de connexions de réseau privé virtuel (VPN, Virtual Private Network).

9.  Fermez le serveur de stratégie réseau.

>[!NOTE]
>Si vous avez plusieurs serveurs NPS, effectuez ces étapes sur chacun d’eux afin que le profil VPN peut vérifier chacun d’eux doivent leur être utilisés.

### <a name="configure-the-template-vpn-profile-on-a-domain-joined-client-computer"></a>Configurer le modèle de profil VPN sur un domaine\-ordinateur joint à un client

Maintenant que vous avez les informations nécessaires de configurer le modèle de profil VPN sur un ordinateur client joint au domaine. Le type de compte d’utilisateur que vous utilisez \(, autrement dit, utilisateur standard ou administrateur\) pour cette partie du processus n’a pas d’importance. 

Toutefois, si vous n’avez pas redémarré l’ordinateur depuis la configuration de l’inscription automatique de certificat, le faire avant de configurer le modèle de connexion VPN pour vous assurer un certificat utilisable inscrit sur celui-ci.

>[!NOTE]
>Il n’existe aucun moyen pour ajouter manuellement les propriétés avancées VPN, telles que les règles NRPT, Always On, approuvé de détection de réseau, etc. Dans l’étape suivante, vous créez une connexion VPN de test pour vérifier la configuration du serveur VPN et que vous pouvez établir une connexion VPN sur le serveur.

**Créer manuellement un connexion VPN de test unique**

1.  Connectez-vous à un ordinateur client joint au domaine en tant que membre de la **utilisateurs VPN** groupe.

2.  Dans le menu Démarrer, tapez **VPN**, puis appuyez sur ENTRÉE.

3.  Dans le volet détails, cliquez sur **ajouter une connexion VPN**.

4.  Dans la liste des fournisseurs de VPN, cliquez sur **Windows (intégré)**.

5.  Nom de connexion, tapez **modèle**.

6.  Dans nom du serveur ou l’adresse, tapez le **externe** nom de domaine complet de votre serveur VPN \(, par exemple, **vpn.contoso.com**\).

7.  Cliquez sur **Enregistrer**.

8.  Sous les paramètres associés, cliquez sur **modifier les options de l’adaptateur**.

9.  Avec le bouton droit **modèle**, puis cliquez sur **propriétés**.

10. Sur le **sécurité** sous l’onglet **Type de VPN**, cliquez sur **IKEv2**.

11. Dans le chiffrement des données, cliquez sur **niveau de chiffrement maximal**.

12. Cliquez sur **utilisez protocole EAP (Extensible Authentication)**; ensuite, dans **utilisez protocole EAP (Extensible Authentication)**, cliquez sur **Microsoft : PEAP (Protected EAP) (cryptage activé)**.

13. Cliquez sur **propriétés** pour ouvrir la boîte de dialogue Propriétés EAP protégées, procédez comme suit :

    a. Dans le **se connecter à ces serveurs** , tapez le nom du serveur NPS que vous avez récupérées à partir des paramètres de l’authentification de serveur NPS plus haut dans cette section (par exemple, NPS01).

    >[!NOTE]
    >Le nom de serveur que vous tapez doit correspondre au nom du certificat. Vous récupéré ce nom plus haut dans cette section. Si le nom ne correspond pas, la connexion s’échoue, indiquant que « la connexion a été empêchée en raison d’une stratégie configurée sur votre serveur RAS/VPN. »

    b.  Sous autorités de Certification racine de confiance, sélectionnez l’autorité de certification qui a émis le certificat du serveur NPS (par exemple, contoso-CA) racine.

    c.  Notifications avant de vous connecter, cliquez sur **ne pas demander à l’utilisateur d’autoriser de nouveaux serveurs ou des autorités de certification**.

    d.  Dans Sélectionner la méthode d’authentification, cliquez sur **carte à puce ou autre certificat**, puis cliquez sur **configurer**. La carte à puce ou autre boîte de dialogue Propriétés du certificat s’ouvre.

    e.  Cliquez sur **utiliser un certificat sur cet ordinateur**.

    f.  Dans la boîte connexion à ces serveurs, entrez le nom du serveur NPS que vous avez récupérées sur les paramètres d’authentification de serveur NPS dans les étapes précédentes.

    g.  Sous autorités de Certification racine de confiance, sélectionnez la racine Autorité de certification qui a émis le certificat du serveur NPS.

    h.  Sélectionnez le **ne pas inviter l’utilisateur à autoriser de nouveaux serveurs ou des autorités de certification approuvées** case à cocher.

    i.  Cliquez sur **OK** pour fermer la carte à puce ou autre boîte de dialogue Propriétés du certificat.

    j.  Cliquez sur **OK** pour fermer la boîte de dialogue Propriétés EAP protégées.

10. Cliquez sur **OK** pour fermer la boîte de dialogue Propriétés du modèle.

11. Fermez la fenêtre Connexions réseau.

12. Dans Paramètres, testez la connexion VPN en cliquant sur **modèle**, puis en cliquant sur **Connect**.

>[!IMPORTANT]
>Assurez-vous que le modèle de connexion VPN à votre serveur VPN est réussi. Cela garantit que les paramètres EAP sont corrects avant de les utiliser dans l’exemple suivant. Vous devez vous connecter au moins une fois avant de continuer ; Autrement, le profil ne contient pas de toutes les informations nécessaires pour se connecter au VPN.

## <a name="bkmk_ProfileXML"></a>Créer les fichiers de configuration ProfileXML

Avant la fin de cette section, assurez-vous que vous avez créé et testé le modèle de connexion VPN qui la section [créer manuellement un profil de connexion de modèle](#bkmk_profile) décrit. Test de la connexion VPN est nécessaire pour vous assurer que le profil contient toutes les informations requises pour se connecter au VPN.

Le script Windows PowerShell dans le Listing 1 crée deux fichiers sur le bureau, et contiennent des **EAPConfiguration** balises basé sur le profil de connexion de modèle vous avez créé précédemment :

-   **VPN_Profile.xml.** Ce fichier contient le balisage XML requis pour configurer le nœud ProfileXML dans le VPNv2 CSP. Utilisez ce fichier avec les services de gestion des appareils mobiles OMA-DM compatibles, tels que Intune.

-   **VPN_Profile.ps1.** Ce fichier est un script Windows PowerShell que vous pouvez exécuter sur les ordinateurs clients pour configurer le nœud ProfileXML dans le VPNv2 CSP. Vous pouvez également configurer le CSP en déployant ce script par le biais de System Center Configuration Manager. Vous ne pouvez pas exécuter ce script dans une session Bureau à distance, y compris une session de Hyper-V est amélioré.

>[!IMPORTANT]
>Les commandes de l’exemple ci-dessous requièrent Windows 10 Build 1607 ou ultérieure.

**Créer VPN_Profile.xml et VPN_Proflie.ps1**

1. Connectez-vous à l’ordinateur client joint à un domaine qui contient le modèle de profil VPN avec le même utilisateur qui compte la section « Créer manuellement un profil de connexion de modèle » décrites.

2. Collez la liste 1 dans l’environnement de script intégré de Windows PowerShell \(ISE\)et personnaliser les paramètres décrits dans les commentaires. Il s’agit de $Template $ProfileName, $Servers, $DnsSuffix, $DomainName, $TrustedNetwork et $DNSServers. Une description complète de chaque paramètre est dans les commentaires.

3.  Exécutez le script pour générer **VPN_Profile.xml** et **VPN_Profile.ps1** sur le bureau.

#### <a name="listing-1-understanding-makeprofileps1"></a>Listing 1. Présentation MakeProfile.ps1

Cette section explique l’exemple de code que vous pouvez utiliser pour mieux comprendre comment créer un profil VPN, en particulier pour la configuration ProfileXML dans le VPNv2 CSP.

Une fois que vous assemblez un script à partir de cet exemple de code et exécuter le script, le script génère deux fichiers : VPN_Profile.XML et VPN_Profile.ps1. Utilisez VPN_Profile.xml pour configurer ProfileXML dans OMA-DM conforme MDM services, tels que Microsoft Intune.

Utilisez le **VPN_Profile.ps1** script dans Windows PowerShell ou System Center Configuration Manager pour configurer ProfileXML sur le bureau Windows 10.

>[!NOTE]
>Pour afficher le script de l’exemple complet, consultez la section [Script complet MakeProfile.ps1](#bkmk_fullscript).

#### <a name="parameters"></a>Paramètres

Configurer les paramètres suivants :

**$Template**. Le nom du modèle à partir duquel récupérer la configuration EAP.

**$ProfileName**. Identificateur alphanumérique unique pour le profil. Le nom du profil ne doit pas inclure une barre oblique (/). Si le nom du profil possède un espace ou autres caractères non alphanumériques, il doit être correctement échappé selon la norme de codage d’URL.

**$Servers**. Adresse IP publique ou routable ou nom DNS pour la passerelle VPN. Il peut pointer vers l’externe IP d’une passerelle ou une adresse IP virtuelle pour une batterie de serveurs. Exemples, 208.147.66.130 ou vpn.contoso.com.

**$DnsSuffix**. Spécifie qu'une ou plusieurs virgules séparées par des suffixes DNS. Le premier dans la liste est également utilisé comme suffixe DNS spécifique à la connexion principal pour l’Interface VPN. L’intégralité de la liste est également ajouté dans le SuffixSearchList.

**$DomainName**. Utilisé pour indiquer l’espace de noms auquel s’applique la stratégie. Lorsqu’une requête de nom est émise, le client DNS compare le nom de la requête à l’ensemble des espaces de noms sous DomainNameInformationList pour rechercher une correspondance. Ce paramètre peut être un des types suivants :

- Nom de domaine complet - nom de domaine complet
- Suffixe - il s’agit d’un suffixe de domaine qui sera ajouté à la requête de nom court pour la résolution DNS. Pour spécifier un suffixe, ajoutez au début d’une période \(.) pour le suffixe DNS.

**$DNSServers**. Liste d’IP du serveur DNS séparés par des virgules d’adresses à utiliser pour l’espace de noms.

**$TrustedNetwork**. Chaîne séparée par des virgules pour identifier le réseau approuvé. VPN ne connecte pas automatiquement lorsque l’utilisateur est sur leur réseau sans fil d’entreprise où les ressources protégées sont directement accessibles à l’appareil.

Voici des exemples de valeurs pour les paramètres utilisés dans les commandes ci-dessous. Veillez à modifier ces valeurs pour votre environnement.

    $TemplateName = 'Template'
    $ProfileName = 'Contoso AlwaysOn VPN'
    $Servers = 'vpn.contoso.com'
    $DnsSuffix = 'corp.contoso.com'
    $DomainName = '.corp.contoso.com'
    $DNSServers = '10.10.0.2,10.10.0.3'
    $TrustedNetwork = 'corp.contoso.com'


### <a name="prepare-and-create-the-profile-xml"></a>Préparer et créer le profil XML

L’exemple de commande suivant obtenir les paramètres EAP à partir du profil de modèle :


    $Connection = Get-VpnConnection -Name $TemplateName
    if(!$Connection)
    {
    $Message = "Unable to get $TemplateName connection profile: $_"
    Write-Host "$Message"
    exit
    }
    $EAPSettings= $Connection.EapConfigXmlStream.InnerXml


### <a name="create-the-profile-xml"></a>Créer le profil XML

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

    $ProfileXML =
    '<VPNProfile>
      <DnsSuffix>' + $DnsSuffix + '</DnsSuffix>
      <NativeProfile>
    <Servers>' + $Servers + '</Servers>
    <NativeProtocolType>IKEv2</NativeProtocolType>
    <Authentication>
      <UserMethod>Eap</UserMethod>
      <Eap>
       <Configuration>
     '+ $EAPSettings + '
       </Configuration>
      </Eap>
    </Authentication>
    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>
      </NativeProfile>
     <AlwaysOn>true</AlwaysOn>
     <RememberCredentials>true</RememberCredentials>
     <TrustedNetworkDetection>' + $TrustedNetwork + '</TrustedNetworkDetection>
      <DomainNameInformation>
    <DomainName>' + $DomainName + '</DomainName>
    <DnsServers>' + $DNSServers + '</DnsServers>
    </DomainNameInformation>
    </VPNProfile>'


### <a name="output-vpnprofilexml-for-intune"></a>VPN_Profile.xml de sortie pour Intune

Vous pouvez utiliser la commande suivante pour enregistrer le fichier XML de profil :

    $ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')

### <a name="output-vpnprofileps1-for-the-desktop-and-system-center-configuration-manager"></a>VPN_Profile.ps1 de sortie pour le bureau et du System Center Configuration Manager

L’exemple de code suivant configure une connexion de VPN IKEv2 AlwaysOn à l’aide du nœud ProfileXML dans le VPNv2 CSP.

Vous pouvez utiliser ce script sur le bureau Windows 10 ou dans System Center Configuration Manager.

### <a name="define-key-vpn-profile-parameters"></a>Définir les paramètres de profil VPN clés

    $Script = '$ProfileName = ''' + $ProfileName + '''
    $ProfileNameEscaped = $ProfileName -replace ' ', '%20'

## <a name="define-vpn-profilexml"></a>Définir ProfileXML VPN

    $ProfileXML = ''' + $ProfileXML + '''
    
### <a name="escape-special-characters-in-the-profile"></a>Caractères spéciaux d’échappement dans le profil

    $ProfileXML = $ProfileXML -replace '<', '&lt;'
    $ProfileXML = $ProfileXML -replace '>', '&gt;'
    $ProfileXML = $ProfileXML -replace '"', '&quot;'
    
### <a name="define-wmi-to-csp-bridge-properties"></a>Définir les propriétés de pont de WMI-CSP

    $nodeCSPURI = "./Vendor/MSFT/VPNv2"
    $namespaceName = "root\cimv2\mdm\dmmap"
    $className = "MDM_VPNv2_01"

### <a name="determine-user-sid-for-vpn-profile"></a>Déterminer le SID de l’utilisateur pour le profil VPN :

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
    

### <a name="define-wmi-session"></a>Définir la session WMI :

    $session = New-CimSession
    $options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Type", "PolicyPlatform_UserContext", $false)
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Id", "$SidValue", $false)


### <a name="detect-and-delete-previous-vpn-profile"></a>Détecter et supprimer le profil VPN précédent :

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
    

### <a name="create-the-vpn-profile"></a>Créez le profil VPN :

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
    Write-Host "$Message"'

### <a name="save-the-profile-xml-file"></a>Enregistrez le fichier XML de profil

    $Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')
    
    $Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
    Write-Host "$Message"

## <a name="bkmk_fullscript"></a>Script complet MakeProfile.ps1

La plupart des exemples utilisent l’applet de commande Set-WmiInstance Windows PowerShell pour insérer ProfileXML dans une nouvelle instance de la classe MDM_VPNv2_01 WMI. 

Toutefois, cela ne fonctionne pas dans System Center Configuration Manager, car vous ne pouvez pas exécuter le package dans le contexte de l’utilisateur final. Par conséquent, ce script utilise le modèle CIM pour créer une session WMI dans le contexte de l’utilisateur, et il crée ensuite une nouvelle instance de la classe WMI de MDM_VPNv2_01 dans cette session. Cette classe WMI utilise le pont WMI-CSP pour configurer le VPNv2 CSP. Par conséquent, en ajoutant l’instance de classe, vous configurez le CSP. 

>[!IMPORTANT]
>Pont de WMI-CSP requiert des droits d’administrateur local, par sa conception. Pour déployer par les profils utilisateur VPN vous devez être à l’aide de SCCM ou des appareils mobiles.

>[!NOTE]
>Le script VPN_Profile.ps1 utilise des SID de l’utilisateur actuel pour identifier le contexte de l’utilisateur. Aucun SID étant disponible dans une session Bureau à distance, le script ne fonctionne pas dans une session Bureau à distance. De même, il ne fonctionne pas dans une session de Hyper-V est amélioré. Si vous testez un accès à distance toujours sur VPN dans les machines virtuelles, désactivez la session étendu sur vos machines virtuelles du client avant d’exécuter ce script.

L’exemple de script suivant inclut tous les exemples de code dans les sections précédentes. Veillez à modifier les exemples de valeurs pour les valeurs appropriées pour votre environnement.
    
   ```XML
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
    
    $ProfileXML =
    '<VPNProfile>
      <DnsSuffix>' + $DnsSuffix + '</DnsSuffix>
      <NativeProfile>
    <Servers>' + $Servers + '</Servers>
    <NativeProtocolType>IKEv2</NativeProtocolType>
    <Authentication>
      <UserMethod>Eap</UserMethod>
      <Eap>
       <Configuration>
     '+ $EAPSettings + '
       </Configuration>
      </Eap>
    </Authentication>
    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>
      </NativeProfile>
    <AlwaysOn>true</AlwaysOn>
    <RememberCredentials>true</RememberCredentials>
    <TrustedNetworkDetection>' + $TrustedNetwork + '</TrustedNetworkDetection>
      <DomainNameInformation>
    <DomainName>' + $DomainName + '</DomainName>
    <DnsServers>' + $DNSServers + '</DnsServers>
    </DomainNameInformation>
    </VPNProfile>'
    
    $ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')
    
    $Script = '$ProfileName = ''' + $ProfileName + '''
    $ProfileNameEscaped = $ProfileName -replace ' ', '%20'
    
    $ProfileXML = ''' + $ProfileXML + '''
    
    $ProfileXML = $ProfileXML -replace '<', '&lt;'
    $ProfileXML = $ProfileXML -replace '>', '&gt;'
    $ProfileXML = $ProfileXML -replace '"', '&quot;'
    
    $nodeCSPURI = "./Vendor/MSFT/VPNv2"
    $namespaceName = "root\cimv2\mdm\dmmap"
    $className = "MDM_VPNv2_01"
    
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
    
    $session = New-CimSession
    $options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Type", "PolicyPlatform_UserContext", $false)
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Id", "$SidValue", $false)
    
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
    Write-Host "$Message"'
    
    $Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')
    
    $Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
    Write-Host "$Message"
   ```

## <a name="configure-the-vpn-client-by-using-windows-powershell"></a>Configurer le client VPN à l’aide de Windows PowerShell

Pour configurer le VPNv2 CSP sur un ordinateur client de Windows 10, exécutez le script VPN_Profile.ps1 Windows PowerShell que vous avez créé dans le [créer le profil XML](#create-the-profile-xml) section. Ouvrez Windows PowerShell en tant qu’administrateur ; Sinon, vous recevrez une erreur indiquant que, _accès refusé_.

Après avoir exécuté VPN_Profile.ps1 pour configurer le profil VPN, vous pouvez vérifier à tout moment si elle a réussi en exécutant la commande suivante dans Windows PowerShell ISE :

    Get-WmiObject -Namespace root\cimv2\mdm\dmmap -Class MDM_VPNv2_01

**Résultats réussis à partir de l’applet de commande Get-WmiObject**


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

La configuration ProfileXML doit être correcte dans la structure, orthographe, la configuration et parfois casse des lettres. Si vous voyez quelque chose de différent dans la structure de liste 1, le balisage ProfileXML probablement contient une erreur.

Si vous avez besoin résoudre les problèmes de la balise, il est plus facile à placer dans un éditeur XML qu’à résoudre dans Windows PowerShell ISE. Dans les deux cas, démarrer avec la version la plus simple du profil et ajouter des composants de revenir à la fois jusqu'à ce que le problème se produit à nouveau.

## <a name="vpn-deploy-client-sccm"></a>Configurer le client VPN à l’aide de System Center Configuration Manager

Dans System Center Configuration Manager, vous pouvez déployer des profils VPN à l’aide du nœud ProfileXML CSP, comme vous l’avez fait dans Windows PowerShell. Ici, vous utilisez le script VPN_Profile.ps1 Windows PowerShell que vous avez créé dans la section [créer les fichiers de configuration ProfileXML](#bkmk_ProfileXML).

Pour utiliser System Center Configuration Manager pour déployer un profil d’accès à distance toujours sur VPN sur les ordinateurs clients Windows 10, vous devez démarrer en créant un groupe d’ordinateurs ou d’utilisateurs pour lesquels vous déployez le profil. Dans ce scénario, créez un groupe d’utilisateurs pour déployer le script de configuration.

### <a name="create-a-user-group"></a>Créer un groupe d’utilisateurs

1.  Dans la console Configuration Manager, ouvrez les ressources et conformité\\regroupements d’utilisateurs.

2.  Sur le **accueil** du ruban, dans le **créer** de groupe, cliquez sur **créer une Collection utilisateur**.

3.  Dans la page Général, procédez comme suit :

    a. Dans **nom**, type **utilisateurs VPN**.

    b. Cliquez sur **Parcourir**, cliquez sur **tous les utilisateurs** et cliquez sur **OK**.

    c. Cliquez sur **Suivant**.

4.  Dans la page règles d’adhésion, procédez comme suit :

    a.  Dans **règles d’adhésion**, cliquez sur **ajouter une règle**, puis cliquez sur **règle directe**. Dans cet exemple, vous ajoutez des utilisateurs individuels à la collection de l’utilisateur. Toutefois, vous pouvez utiliser une règle de requête pour ajouter des utilisateurs à cette collection dynamiquement pour un déploiement à grande échelle.

    b.  Dans la page **Bienvenue**, cliquez sur **Suivant**.

    c.  Sur la page Rechercher des ressources, dans **valeur**, tapez le nom de l’utilisateur que vous souhaitez ajouter. Le nom de ressource inclut le domaine de l’utilisateur. Pour inclure les résultats en fonction d’une correspondance partielle, insérez le **%** caractère aux deux extrémités de votre critère de recherche. Par exemple, pour rechercher tous les utilisateurs qui contient la chaîne « lori », tapez **% lori %**. Cliquez sur **Suivant**.

    d.  Dans la page Sélectionner les ressources, sélectionnez les utilisateurs que vous souhaitez ajouter au groupe, puis cliquez sur **suivant**.

    e.  Dans la page Résumé, cliquez sur **suivant**.

    f.  Dans la page de fin, cliquez sur **fermer**.

6.  Dans la page règles d’adhésion de l’Assistant Création d’un regroupement d’utilisateurs, cliquez sur **suivant**.

7.  Dans la page Résumé, cliquez sur **suivant**.

8.  Dans la page de fin, cliquez sur **fermer**.

Après avoir créé le groupe d’utilisateurs pour recevoir le profil VPN, vous pouvez créer un package et un programme pour déployer le script de configuration de Windows PowerShell que vous avez créé dans la section [créer les fichiers de configuration ProfileXML](#bkmk_ProfileXML).

### <a name="create-a-package-containing-the-profilexml-configuration-script"></a>Créer un package contenant le script de configuration ProfileXML

1.  Héberger le script VPN_Profile.ps1 sur un partage réseau accessible par le compte ordinateur de serveur de site.

2.  Dans la console Configuration Manager, ouvrez **bibliothèque de logiciels\\gestion des applications\\Packages**.

3.  Sur le **accueil** du ruban, dans le **créer** de groupe, cliquez sur **création d’un Package** pour démarrer l’Assistant de programme et de création d’un Package.

4.  Dans la page de Package, procédez comme suit :

    a. Dans **nom**, type **Windows 10 toujours sur profil VPN**.

    b. Sélectionnez le **ce package contient des fichiers sources** case à cocher, puis cliquez sur **Parcourir**.

    c. Dans la boîte de dialogue Définir le dossier Source, cliquez sur **Parcourir**, sélectionnez le partage de fichiers contenant VPN_Profile.ps1, puis cliquez sur **OK**.<p>Veillez à que sélectionner un chemin d’accès réseau, pas un chemin d’accès local. En d’autres termes, le chemin d’accès doit être quelque chose comme  *\\fileserver\\vpnscript*, et non *c:\\vpnscript*.

1.  Cliquez sur **Suivant**.

2.  Dans la page Type de programme, cliquez sur **suivant**.

3.  Dans la page programme Standard, procédez comme suit :

    a.  Dans **nom**, type **Script de profil VPN**.

    b.  Dans **ligne de commande**, type **PowerShell.exe - ExecutionPolicy Bypass - fichier « VPN_Profile.ps1 »**.

    c.  Dans **mode d’exécution**, cliquez sur **exécuter avec les droits d’administration**.

    d.  Cliquez sur **Suivant**.

4.  Dans la page Configuration requise, procédez comme suit :

    a.  Sélectionnez **ce programme peut s’exécuter uniquement sur des plateformes spécifiées**.

    b.  Sélectionnez le **tout Windows 10 (32 bits)** et **tout Windows 10 (64 bits)** cases à cocher.

    c.  Dans **espace disque estimé**, type **1**.

    d.  Dans **Maximum durée d’exécution allouée (en minutes)**, type **15**.

    e.  Cliquez sur **Suivant**.

5.  Dans la page Résumé, cliquez sur **suivant**.

6.  Dans la page de fin, cliquez sur **fermer**.

Avec le package et le programme créé, vous devez la déployer à la **utilisateurs VPN** groupe.

### <a name="deploy-the-profilexml-configuration-script"></a>Déployer le script de configuration ProfileXML

1.  Dans la console Configuration Manager, ouvrez la bibliothèque de logiciels\\gestion des applications\\Packages.

2.  Dans **Packages**, cliquez sur **Windows 10 toujours sur profil VPN**.

3.  Sur le **programmes** avec le bouton droit de l’onglet, en bas du volet d’informations, **Script de profil VPN**, cliquez sur **propriétés**et procédez comme suit :

    a.  Sur le **avancé** sous l’onglet **lorsque ce programme est attribué à un ordinateur**, cliquez sur **une fois pour chaque utilisateur qui ouvre une session sur**.

    b.  Cliquez sur **OK**.

4.  Avec le bouton droit **Script de profil VPN** et cliquez sur **déployer** pour démarrer l’Assistant déploiement logiciel.

5.  Dans la page Général, procédez comme suit :

    a.  En regard de **Collection**, cliquez sur **Parcourir**.

    b.  Dans le **Types de collections** liste (en haut à gauche), cliquez sur **regroupements d’utilisateurs**.

    c.  Cliquez sur **utilisateurs VPN**, puis cliquez sur **OK**.

    d.  Cliquez sur **Suivant**.

6.  Sur la page de contenu, procédez comme suit :

    a.  Cliquez sur **ajouter**, puis cliquez sur **Point de Distribution**.

    b.  Dans **points de distribution disponibles**, sélectionnez les points de distribution à laquelle vous souhaitez distribuer le script de configuration ProfileXML, puis cliquez sur **OK**.

    c.  Cliquez sur **Suivant**.

7.  Dans la page Paramètres de déploiement, cliquez sur **suivant**.

8.  Sur la page de planification, procédez comme suit :

    a.  Cliquez sur **New** pour ouvrir la boîte de dialogue calendrier d’attribution.

    b.  Cliquez sur **attribuer immédiatement après cet événement**, puis cliquez sur **OK**.

    c.  Cliquez sur **Suivant**.

9.  Dans la page de l’expérience utilisateur, procédez comme suit :

    1.  Sélectionnez le **Installation du logiciel** case à cocher.

    2.  Cliquez sur **Résumé**.

10. Dans la page Résumé, cliquez sur **suivant**.

11. Dans la page de fin, cliquez sur **fermer**.

Avec le script de configuration ProfileXML déployé, vous connecter à un ordinateur client de Windows 10 avec le compte d’utilisateur que vous avez sélectionné lorsque vous avez généré le regroupement d’utilisateurs. Vérifiez la configuration du client VPN.

>[!NOTE]
>Le script VPN_Profile.ps1 ne fonctionne pas dans une session Bureau à distance. De même, il ne fonctionne pas dans une session de Hyper-V est amélioré. Si vous testez un accès à distance toujours sur VPN dans les machines virtuelles, désactivez la session étendu sur vos machines virtuelles du client avant de continuer.

### <a name="verify-the-configuration-of-the-vpn-client"></a>Vérifier la configuration du client VPN

1.  Dans le panneau de configuration, sous **système\\sécurité**, cliquez sur **Configuration Manager**. 

2.  Dans la boîte de dialogue Propriétés de Configuration Manager, sur le **Actions** onglet, procédez comme suit :

    a.  Cliquez sur **récupération de stratégie ordinateur et Cycle d’évaluation**, cliquez sur **exécuter maintenant**, puis cliquez sur **OK**.

    b.  Cliquez sur **récupération de stratégie utilisateur et Cycle d’évaluation**, cliquez sur **exécuter maintenant**, puis cliquez sur **OK**.

    c.  Cliquez sur **OK**.

3.  Fermez le panneau de configuration.

Vous devriez voir le nouveau profil VPN sous peu.

## <a name="configure-the-vpn-client-by-using-intune"></a>Configurer le client VPN à l’aide d’Intune

Pour utiliser Intune pour déployer Windows 10 distant accès profils VPN Always On, vous pouvez configurer le nœud ProfileXML CSP en utilisant le profil VPN que vous avez créé dans la section [créer les fichiers de configuration ProfileXML](#bkmk_ProfileXML), ou vous pouvez utiliser le protocole EAP base Exemple XML fourni ci-dessous.

>[!NOTE]
>Intune utilise désormais les groupes Azure AD. Si Azure AD Connect synchronisés avec le groupe d’utilisateurs VPN en local à Azure AD et les utilisateurs sont affectés au groupe utilisateurs de VPN, vous êtes prêt à continuer.

Créez la stratégie de configuration de périphérique VPN pour configurer les ordinateurs clients Windows 10 pour tous les utilisateurs ajoutés au groupe. Dans la mesure où le modèle de Intune fournit des paramètres VPN, vous devez uniquement copier les \<EapHostConfig > \</EapHostConfig > partie du fichier VPN_ProfileXML. 


### <a name="create-the-always-on-vpn-configuration-policy"></a>Créer la stratégie de configuration VPN Always On

1.  Connectez-vous à la [Azure portal](https://portal.azure.com/).

2.  Accédez à **Intune** > **Configuration de l’appareil** > **profils**.

3.  Cliquez sur **créer un profil** pour démarrer l’Assistant créer un profil.

4.  Entrez un **nom** pour le profil VPN et (éventuellement) une description.

5.   Sous **plateforme**, sélectionnez **Windows 10 ou version ultérieure**, puis choisissez **VPN** dans la profil type liste déroulante.

     >[!TIP]
     >Si vous créez un profileXML VPN personnalisé, consultez [ProfileXML s’appliquent à l’aide d’Intune](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-profile-options#apply-profilexml-using-intune) pour obtenir des instructions.

6. Sous le **VPN de Base** onglet, vérifiez ou définissez les paramètres suivants :

    - **Nom de la connexion :** Entrez le nom de la connexion VPN tel qu’il apparaît sur l’ordinateur client dans le **VPN** onglet sous **paramètres**, par exemple, _Contoso AutoVPN_.  
    
    - **serveurs :** Ajoutez un ou plusieurs serveurs VPN en cliquant sur **ajouter.**
    
    - **Description** et **adresse IP ou nom de domaine complet :** Entrez une description et adresse IP ou nom de domaine complet du serveur VPN. Ces valeurs doivent être alignées avec le nom du sujet dans le certificat d’authentification du serveur VPN. 
    
    - **Serveur par défaut :** Si c’est le serveur VPN par défaut, la valeur est **True**. Cette opération active ce serveur comme serveur par défaut que les appareils utilisent pour établir la connexion. 
    
    - **Type de connexion :** La valeur **IKEv2**.  
    
    - **Always On :** La valeur **activer** pour vous connecter au VPN automatiquement lors de la connexion et de rester connectés jusqu'à ce que l’utilisateur se déconnecte manuellement.
    
    - **N’oubliez pas d’informations d’identification à chaque ouverture de session**:  Valeur booléenne (true ou false) pour la mise en cache des informations d’identification. Si défini sur true, informations d’identification est mises en cache autant que possible.

7.  Copiez la chaîne XML suivante dans un éditeur de texte :<p>
 
    [!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]
    <p>
    
    ```XML
    <EapHostConfig xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><CredentialsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>false</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</AcceptServerName></EapType></Eap><EnableQuarantineChecks>false</EnableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</AcceptServerName></PeapExtensions></EapType></Eap></Config></EapHostConfig>
    ```

8.  Remplacez le  **\<TrustedRootCA > 5 a 89 fe cb 5 b 49 a7 0 b 1 a 52 63 b7 35 ee d7 1C c2 68 être 4 b <\/TrustedRootCA >** dans l’exemple avec l’empreinte numérique du certificat de votre autorité de certification racine en local dans les deux emplacements.
  
    >[!Important]
    >N’utilisez pas l’empreinte numérique d’exemple dans le \<TrustedRootCA >\</TrustedRootCA > section ci-dessous.  Le TrustedRootCA doit être l’empreinte numérique du certificat de l’autorité de certification racine en local qui a émis le certificat d’authentification serveur pour les serveurs RRAS et NPS. **Cela ne doit pas être le certificat racine de cloud, ni l’empreinte de certificat d’autorité de certification émettrice intermédiaire**.

10. Remplacez le  **\<ServerNames > NPS.contoso.com\</ServerNames >** dans l’exemple de code XML avec le nom de domaine complet du NPS joints au domaine dans lequel l’authentification a lieu. 

11. Copiez la chaîne XML révisée et collez-y le **Xml EAP** sous l’onglet VPN de Base, puis cliquez sur **OK**.<p>Une stratégie toujours sur Configuration de périphérique VPN à l’aide du protocole EAP est créée dans Intune.


### <a name="sync-the-always-on-vpn-configuration-policy-with-intune"></a>Synchronisation de la stratégie de configuration VPN Always On avec Intune

Pour tester la stratégie de configuration, connectez-vous à un ordinateur client de Windows 10 en tant que l’utilisateur que vous avez ajouté à la **toujours sur les utilisateurs VPN** groupe, puis synchronisation avec Intune.

1.  Dans le menu Démarrer, cliquez sur **paramètres**.

2.  Dans Paramètres, cliquez sur **comptes**, puis cliquez sur **accès scolaire ou Professionnel**.

3.  Cliquez sur le profil de gestion des appareils mobiles, puis cliquez sur **Info**.

4.  Cliquez sur **synchronisation** pour forcer une évaluation de la stratégie Intune et la récupération.

5.  Fermer les paramètres. Après la synchronisation, vous voyez le profil VPN disponible sur l’ordinateur.

## <a name="next-step"></a>Étape suivante
Vous avez terminé le déploiement VPN Always On.  D’autres fonctionnalités que vous pouvez configurer, consultez le tableau ci-dessous :

|Si vous souhaitez...  |Alors consultez...  |
|---------|---------|
|Configurer l’accès conditionnel pour VPN    |[Étape 7. (Facultatif) Configurer l’accès conditionnel pour la connectivité VPN à l’aide d’Azure AD](../../ad-ca-vpn-connectivity-windows10.md): Dans cette étape, vous pouvez affiner un accès VPN aux utilisateurs comment autorisé à vos ressources en utilisant [accès conditionnel Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Accès conditionnel Azure AD pour la connectivité de réseau privé virtuel (VPN), vous pouvez protéger les connexions VPN. L’accès conditionnel est un moteur d’évaluation basé sur les stratégies qui vous permet de créer des règles d’accès pour n’importe quelle application connectée Azure Active Directory (Azure AD).         |
|En savoir plus sur les fonctionnalités VPN avancées  |[Fonctionnalités VPN avancées](always-on-vpn-adv-options.md#advanced-vpn-features): Cette page fournit des conseils sur l’activation de filtres de trafic VPN, comment configurer des connexions VPN automatique à l’aide de déclencheurs d’application et comment configurer NPS pour autoriser uniquement les connexions VPN à partir de clients à l’aide de certificats émis par Azure AD.        |


---
