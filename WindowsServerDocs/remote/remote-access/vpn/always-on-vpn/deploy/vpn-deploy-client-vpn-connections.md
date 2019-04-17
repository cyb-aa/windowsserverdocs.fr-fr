---
title: Configurer les connexions VPN Toujours actif (AlwaysOn) du client Windows10
description: Dans cette étape, vous en savoir plus sur le schéma et les options de ProfileXML et configurez les ordinateurs clients de Windows 10 pour communiquer avec cette infrastructure avec une connexion VPN.
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
ms.sourcegitcommit: 4147e092d77d9458696e6686bccefe3788c90508
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/28/2019
ms.locfileid: "9031313"
---
# Étape6. Configurer les connexions de Windows 10 client toujours actif (AlwaysOn)

>S’applique à: Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10


& #171;  [ **Précédente:** étape 5. Configurer les paramètres de pare-feu et de DNS](vpn-deploy-dns-firewall.md)<br>
& #187; [ **Suivant:** étape 7. (Facultatif) Accès conditionnel pour une connexion VPN à l’aide d’Azure AD](../../ad-ca-vpn-connectivity-windows10.md)

Dans cette étape, vous en savoir plus sur le schéma et les options de ProfileXML et configurez les ordinateurs clients de Windows 10 pour communiquer avec cette infrastructure avec une connexion VPN. 

Vous pouvez configurer le client toujours actif (AlwaysOn) par le biais de PowerShell, SCCM ou Intune. Les trois nécessitent un profil VPN XML pour configurer les paramètres VPN appropriés. Automatisation de PowerShell l’inscription pour les organisations sans SCCM ou Intune est possible.

>[!NOTE]
>La stratégie de groupe n’inclut pas les modèles d’administration pour configurer le client Windows 10 distant accès toujours actif (AlwaysOn).  Toutefois, vous pouvez utiliser les scripts d’ouverture de session.

## Vue d’ensemble de ProfileXML

ProfileXML est un nœud d’URI dans le CSP VPNv2. Au lieu de la configuration de chaque nœud CSP VPNv2 individuellement, tels que des déclencheurs, router les listes et les protocoles d’authentification, ce nœud permet de configurer un client VPN Windows 10 en fournissant tous les paramètres comme un seul bloc XML à un seul nœud CSP. Le schéma ProfileXML correspond au schéma des nœuds CSP VPNv2 quasiment identique, mais certains termes sont légèrement différents.

Vous utilisez ProfileXML dans toutes les méthodes de la distribution que décrit ce déploiement, y compris Windows PowerShell, System Center Configuration Manager et Intune. Il existe deux façons de configurer le nœud ProfileXML VPNv2 CSP dans ce déploiement:

- **OMA-DM**. Une façon consiste à utiliser un fournisseur GPM à l’aide du protocole OMA-DM, comme indiqué précédemment dans la section [nœud CSP VPNv2](../always-on-vpn-technology-overview.md#vpnv2-csp-nodes). À l’aide de cette méthode, vous pouvez facilement insérer le balisage XML de configuration de profil VPN dans le nœud ProfileXML CSP lorsque vous utilisez Intune.

- **Windows Management Instrumentation (WMI) - to - pont CSP**. La deuxième méthode de configuration du nœud ProfileXML CSP consiste à utiliser le pont WMI vers CSP, une classe WMI appelée **MDM_VPNv2_01**, qui peuvent accéder au fournisseur CSP VPNv2 et le nœud ProfileXML. Lorsque vous créez une nouvelle instance de cette classe WMI, WMI utilise le fournisseur CSP pour créer le profil VPN lors de l’utilisation de Windows PowerShell et System Center Configuration Manager.

Même si ces méthodes de configuration diffèrent, requièrent un profil VPN XML correctement formaté. Pour utiliser le paramètre CSP VPNv2 de ProfileXML, vous construire le fichier XML à l’aide du schéma ProfileXML pour configurer les balises nécessaires pour le scénario de déploiement simple. Pour plus d’informations, voir [XSD ProfileXML](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-profile-xsd).

Vous trouvez ci-dessous chacun des paramètres requis et sa balise ProfileXML correspondante. Configuration de chaque paramètre dans une balise spécifique au sein du schéma de ProfileXML, et pas toutes sont trouvent sous le profil natif. Pour le placement de la balise supplémentaires, consultez le schéma ProfileXML.

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

**Type de connexion:** IKEv2 native

Élément ProfileXML:

    <NativeProtocolType>IKEv2</NativeProtocolType>

**Routage:** Tunneling fractionné

Élément ProfileXML:

    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>

**Résolution de nom:** Suffixe DNS et la liste d’informations de nom de domaine

Éléments de ProfileXML:

    <DomainNameInformation>
    <DomainName>.corp.contoso.com</DomainName>
    <DnsServers>10.10.1.10,10.10.1.50</DnsServers>
    </DomainNameInformation>
    
    <DnsSuffix>corp.contoso.com</DnsSuffix>


**Déclenchement:** Détection de réseaux toujours activé et de confiance

Éléments de ProfileXML:

    <AlwaysOn>true</AlwaysOn>
    <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>


**Authentification:** PEAP-TLS avec des certificats utilisateur protégé par le module de plateforme sécurisée

Éléments de ProfileXML:

    <Authentication>
    <UserMethod>Eap</UserMethod>
    <Eap>
    <Configuration>...</Configuration>
    </Eap>
    </Authentication>

Vous pouvez utiliser des balises simples pour configurer certains mécanismes d’authentification VPN. Toutefois, le protocole PEAP et EAP sont plus complexes. Le moyen le plus simple pour créer le balisage XML consiste à configurer un client VPN avec ses paramètres EAP, puis à exporter cette configuration au format XML. 

Pour plus d’informations sur les paramètres EAP, voir [configuration EAP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/eap-configuration). 

## <a name="bkmk_profile"></a>Créer manuellement un profil de connexion de modèle

Dans cette étape, vous utilisez Protected Extensible Authentication Protocol \(PEAP\) pour sécuriser la communication entre le client et le serveur. Contrairement à un simple nom d’utilisateur et mot de passe, cette connexion requiert une section EAPConfiguration unique dans le profil VPN à fonctionner. 

Au lieu d’expliquer comment créer le balisage XML à partir de zéro, vous utilisez des paramètres dans Windows pour créer un modèle de profil VPN. Après avoir créé le modèle de profil VPN, vous utilisez Windows PowerShell pour consommer la portion de EAPConfiguration à partir de ce modèle pour créer le ProfileXML finale que vous déployez plus loin dans le déploiement.

### Enregistrez les paramètres de certificat de serveur NPS

Avant de créer le modèle, prenez note du nom d’hôte ou le nom de domaine complet (FQDN) du serveur NPS dans le certificat du serveur et le nom de l’autorité de certification qui a émis le certificat.

**Procédure:**

1.  Sur votre serveur NPS, ouvrez Network Policy Server.

2.  Dans la console NPS, sous stratégies, cliquez sur **Les stratégies de réseau**.

3.  **Les connexions réseau privé virtuel \(VPN\)** d’avec le bouton droit, puis cliquez sur **Propriétés**.

4.  Cliquez sur l’onglet **contraintes** , puis cliquez sur les **Méthodes d’authentification**.

5.  Dans les Types de protocoles EAP, cliquez sur **Microsoft: PEAP (Protected EAP)**, puis cliquez sur **Modifier**.

6.  Enregistrez les valeurs pour **le certificat émis vers** et de **l’émetteur**.<p>Vous utilisez ces valeurs dans la configuration du modèle VPN à venir. Par exemple, si le nom de domaine complet du serveur est nps01.corp.contoso.com et le nom d’hôte est NPS01, le nom du certificat repose sur le nom DNS ou le nom de domaine complet du serveur: par exemple, nps01.corp.contoso.com.

7.  Annuler la boîte de dialogue Modifier les propriétés EAP protégé.

8.  Annuler la boîte de dialogue des propriétés des connexions réseau privé virtuel (VPN).

9.  Fermez le serveur NPS.

>[!NOTE]
>Si vous avez plusieurs serveurs NPS, effectuez ces étapes sur chacun d’eux afin que le profil VPN puisse vérifier chacune d’elles doivent ils être utilisés.

### Configurer le modèle de profil VPN sur un ordinateur client joint à un Domaine\\Nom

Maintenant que vous avez les informations nécessaires à configurer le modèle de profil VPN sur un ordinateur client joint au domaine. Le type de compte utilisateur que vous utilisez \ (autrement dit, un utilisateur standard ou administrator\) pour cette partie du processus n’a pas d’importance. 

Toutefois, si vous n’avez pas redémarré l’ordinateur depuis la configuration de l’inscription automatique de certificat, le faire avant de configurer le modèle de connexion VPN que vous disposez d’un certificat utilisable inscrit sur ce dernier.

>[!NOTE]
>Il n’existe aucun moyen pour ajouter manuellement les propriétés avancées de VPN, par exemple, les règles de noms, toujours active, approuvé la détection du réseau, etc.. Dans l’étape suivante, vous créez une connexion VPN de test pour vérifier la configuration du serveur VPN et que vous pouvez établir une connexion VPN sur le serveur.

**Créer manuellement un seul et même test connexion VPN**

1.  Connectez-vous à un ordinateur client joint au domaine en tant que membre du groupe **d’Utilisateurs VPN** .

2.  Dans le menu Démarrer, tapez **VPN**et appuyez sur ENTRÉE.

3.  Dans le volet d’informations, cliquez sur **Ajouter une connexion VPN**.

4.  Dans la liste de fournisseurs de VPN, cliquez sur **Windows (intégré)**.

5.  Nom de connexion, tapez le **modèle**.

6.  Nom du serveur ou l’adresse, tapez l' **externe** nom de domaine complet de votre serveur VPN \ (par exemple,**vpn.contoso.com**\).

7.  Cliquez sur **Enregistrer**.

8.  Sous paramètres associés, cliquez sur **Modifier les options de carte**.

9.  Cliquez sur **modèle**, puis cliquez sur **Propriétés**.

10. Dans l’onglet **sécurité** , dans le **Type de réseau VPN**, cliquez sur **IKEv2**.

11. Dans le chiffrement des données, cliquez sur le **niveau de chiffrement maximal**.

12. Cliquez sur **utilisation Extensible Authentication Protocol (EAP)**; Ensuite, dans **Une utilisation protocole EAP (Extensible Authentication)**, cliquez sur **Microsoft: PEAP (Protected EAP) (cryptage activé)**.

13. Cliquez sur **Propriétés** pour ouvrir la boîte de dialogue Propriétés EAP protégées, puis procédez comme suit:

    a. Dans la zone de **se connecter à ces serveurs** , tapez le nom du serveur NPS que vous avez récupérées à partir des paramètres d’authentification serveur NPS plus haut dans cette section (par exemple, NPS01).

    >[!NOTE]
    >Vous tapez le nom du serveur doit correspondre au nom dans le certificat. Vous récupéré ce nom plus haut dans cette section. Si le nom ne correspond pas, la connexion échoue, indiquant que «la connexion n’a pas pu en raison d’une stratégie configurée sur votre serveur RAS/VPN.»

    b.  Sous autorités de Certification racine de confiance, sélectionnez la racine Autorité de certification qui a émis le certificat du serveur NPS (par exemple, contoso-CA).

    c.  Dans les Notifications avant de vous connecter, cliquez sur **ne demandez pas l’utilisateur d’autoriser de nouveaux serveurs ou des autorités de certification approuvées**.

    d.  Dans Sélectionner la méthode d’authentification, cliquez sur la **carte à puce ou autre certificat**, puis cliquez sur **configurer**. La carte à puce ou autre boîte de dialogue des propriétés du certificat s’ouvre.

    e.  Cliquez sur **utiliser un certificat sur cet ordinateur**.

    f.  Dans la zone connexion à ces serveurs, entrez le nom du serveur NPS récupérée à partir des paramètres de l’authentification du serveur NPS dans les étapes précédentes.

    g.  Sous autorités de Certification racine de confiance, sélectionnez l’autorité de certification qui a émis le certificat du serveur NPS racine.

    h.  Sélectionnez le **n’invite l’utilisateur à autoriser de nouveaux serveurs ou des autorités de certification approuvées** case à cocher.

    i.  Cliquez sur **OK** pour fermer la carte à puce ou une autre boîte de dialogue des propriétés du certificat.

    j.  Cliquez sur **OK** pour fermer la boîte de dialogue Propriétés EAP protégées.

10. Cliquez sur **OK** pour fermer la boîte de dialogue des propriétés du modèle.

11. Fermez la fenêtre de connexions réseau.

12. Dans les paramètres, testez la connexion VPN en cliquant sur **le modèle**, cliquez sur **se connecter**.

>[!IMPORTANT]
>Assurez-vous que le modèle de connexion VPN à votre serveur VPN est réussi. Cela permet de s’assurer que les paramètres EAP sont corrects avant de les utiliser dans l’exemple suivant. Vous devez vous connecter au moins une fois avant de poursuivre; dans le cas contraire, le profil ne contient toutes les informations nécessaires pour se connecter au VPN.

## <a name="bkmk_ProfileXML"></a>Créer les fichiers de configuration ProfileXML

Avant la fin de cette section, assurez-vous que vous avez créé et testé le modèle de connexion VPN qui décrit la section [créer manuellement un profil de connexion du modèle](#bkmk_profile) . Test de la connexion VPN est nécessaire pour vous assurer que le profil contient toutes les informations nécessaires pour se connecter au VPN.

Le script Windows PowerShell dans le Listing 1 crée deux fichiers sur le bureau, et contiennent des balises **EAPConfiguration** basés sur le profil de connexion modèle que vous avez créé précédemment:

-   **VPN_Profile.Xml.** Ce fichier contient le balisage XML requis pour configurer le nœud ProfileXML dans le fournisseur CSP VPNv2. Utilisez ce fichier avec les services GPM OMA-DM compatibles, par exemple Intune.

-   **VPN_Profile.ps1.** Ce fichier est un script Windows PowerShell que vous pouvez exécuter sur les ordinateurs clients pour configurer le nœud ProfileXML dans le fournisseur CSP VPNv2. Vous pouvez également configurer le fournisseur CSP en déployant ce script par le biais de System Center Configuration Manager. Vous ne pouvez pas exécuter ce script dans une session Bureau à distance, y compris une session amélioré Hyper-V.

>[!IMPORTANT]
>Les commandes de l’exemple ci-dessous nécessitent Windows 10 version 1607 ou ultérieure.

**Créer des VPN_Profile.xml et VPN_Proflie.ps1**

1. Connectez-vous à l’ordinateur client joint au domaine contenant le modèle de profil VPN avec l’utilisateur même compte du fait que la section «Créer manuellement un profil de connexion de modèle» décrites.

2. Coller liste 1 dans l’environnement Windows PowerShell de script intégré \(ISE\) et personnaliser les paramètres décrits dans les commentaires. Il s’agit de $Template, $ProfileName, $Servers, $DnsSuffix, $DomainName, $TrustedNetwork et $DNSServers. Une description complète de chaque paramètre se trouve dans les commentaires.

3.  Exécutez le script pour générer **VPN_Profile.xml** et **VPN_Profile.ps1** sur le bureau.

#### Le listing 1. Compréhension MakeProfile.ps1

Cette section décrit l’exemple de code que vous pouvez utiliser pour mieux comprendre comment créer un profil VPN, spécifiquement pour la configuration ProfileXML dans le fournisseur CSP VPNv2.

Une fois que vous assemblez un script à partir de cet exemple de code et exécutez le script, le script génère deux fichiers: VPN_Profile.xml et VPN_Profile.ps1. Utiliser VPN_Profile.xml pour configurer ProfileXML dans OMA-DM conforme GPM services, tels que Microsoft Intune.

Utilisez le script **VPN_Profile.ps1** dans Windows PowerShell ou System Center Configuration Manager pour configurer ProfileXML sur le bureau Windows 10.

>[!NOTE]
>Pour afficher le script de l’exemple complet, consultez la section [Script complet MakeProfile.ps1](#bkmk_fullscript).

#### Parameters

Configurer les paramètres suivants:

**$Template**. Le nom du modèle à partir duquel extraire la configuration EAP.

**$ProfileName**. Identificateur alphanumérique unique pour le profil. Le nom du profil ne doit pas inclure une barre oblique (/). Si le nom du profil dispose d’un espace ou un autre caractère non alphanumérique, il doit être échappé correctement en fonction de la norme de codage URL.

**$Servers**. Adresse IP publique ou routable ou le nom DNS pour la passerelle VPN. Elle peut pointer vers externe IP d’une passerelle ou une adresse IP virtuelle d’une batterie de serveurs. Obtenir des exemples, 208.147.66.130 ou vpn.contoso.com.

**$DnsSuffix**. Spécifie que séparées par des virgules d’un ou plusieurs suffixes DNS. Le premier dans la liste est également utilisé en tant que le suffixe DNS spécifique à la connexion principal pour l’Interface VPN. La liste entière est également ajoutée dans la liste SuffixSearchList.

**$DomainName**. Utilisé pour indiquer l’espace de noms auquel s’applique la stratégie. Lorsqu’une requête de nom est émise, le client DNS compare le nom de la requête à l’ensemble des espaces de noms sous DomainNameInformationList pour trouver une correspondance. Ce paramètre peut être un des types suivants:

- Nom de domaine complet - nom de domaine complet
- Suffixe - un suffixe de domaine qui sera ajouté à la requête shortname pour la résolution DNS. Pour spécifier un suffixe, faites précéder une période \ (.) pour le suffixe DNS.

**$DNSServers**. Liste des IP du serveur DNS séparées par des virgules adresses à utiliser pour l’espace de noms.

**$TrustedNetwork**. Chaîne séparée par des virgules pour identifier le réseau approuvé. VPN ne connecte pas automatiquement lorsque l’utilisateur est sur leur réseau sans fil d’entreprise dans lequel les ressources protégées sont directement accessibles à l’appareil.

Voici des exemples de valeurs pour les paramètres utilisés dans les commandes ci-dessous. Veillez à modifier ces valeurs pour votre environnement.

    $TemplateName = 'Template'
    $ProfileName = 'Contoso AlwaysOn VPN'
    $Servers = 'vpn.contoso.com'
    $DnsSuffix = 'corp.contoso.com'
    $DomainName = '.corp.contoso.com'
    $DNSServers = '10.10.0.2,10.10.0.3'
    $TrustedNetwork = 'corp.contoso.com'


### Préparer et créer le fichier XML de profil

Les exemples de commandes suivants accéder à paramètres EAP dans le profil de modèle:


    $Connection = Get-VpnConnection -Name $TemplateName
    if(!$Connection)
    {
    $Message = "Unable to get $TemplateName connection profile: $_"
    Write-Host "$Message"
    exit
    }
    $EAPSettings= $Connection.EapConfigXmlStream.InnerXml


### Créer le fichier XML de profil

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


### Sortie VPN_Profile.xml pour Intune

Vous pouvez utiliser la commande suivante pour enregistrer le fichier XML de profil:

    $ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')

### VPN_Profile.ps1 de sortie pour le bureau et System Center Configuration Manager

L’exemple de code suivant configure une connexion de VPN IKEv2 AlwaysOn à l’aide du nœud ProfileXML dans le fournisseur CSP VPNv2.

Vous pouvez utiliser ce script sur le bureau Windows 10 ou dans System Center Configuration Manager.

### Définir les paramètres de profil VPN clés

    $Script = '$ProfileName = ''' + $ProfileName + '''
    $ProfileNameEscaped = $ProfileName -replace ' ', '%20'

## Définir ProfileXML VPN

    $ProfileXML = ''' + $ProfileXML + '''
    
### Échapper les caractères spéciaux dans le profil

    $ProfileXML = $ProfileXML -replace '<', '&lt;'
    $ProfileXML = $ProfileXML -replace '>', '&gt;'
    $ProfileXML = $ProfileXML -replace '"', '&quot;'
    
### Définir les propriétés de pont WMI vers CSP

    $nodeCSPURI = "./Vendor/MSFT/VPNv2"
    $namespaceName = "root\cimv2\mdm\dmmap"
    $className = "MDM_VPNv2_01"

### Déterminez le SID de l’utilisateur pour le profil VPN:

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
    

### Définissez la session WMI:

    $session = New-CimSession
    $options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Type", "PolicyPlatform_UserContext", $false)
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Id", "$SidValue", $false)


### Détecter et supprimer le profil VPN précédent:

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
    

### Créer le profil VPN:

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

### Enregistrez le fichier XML de profil

    $Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')
    
    $Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
    Write-Host "$Message"

## <a name="bkmk_fullscript"></a>Script complet MakeProfile.ps1

La plupart des exemples utiliser l’applet de commande Set-WmiInstance Windows PowerShell pour insérer ProfileXML dans une nouvelle instance de la classe WMI MDM_VPNv2_01. 

Toutefois, cela ne fonctionne pas dans System Center Configuration Manager, car vous ne pouvez pas exécuter le package dans le contexte de l’utilisateur final. Par conséquent, ce script utilise le modèle d’informations courantes pour créer une session WMI dans le contexte de l’utilisateur, et il crée ensuite une nouvelle instance de la classe WMI MDM_VPNv2_01 dans cette session. Cette classe WMI utilise le pont WMI vers CSP pour configurer le fournisseur CSP VPNv2. Par conséquent, en ajoutant l’instance de classe, vous configurez le fournisseur CSP. 

>[!IMPORTANT]
>Pont WMI vers CSP requiert des droits d’administrateur local, par sa conception. Pour déployer par les profils utilisateur VPN est recommandé d’utiliser SCCM ou GPM.

>[!NOTE]
>Le script VPN_Profile.ps1 utilise SID de l’utilisateur actuel pour identifier le contexte de l’utilisateur. Dans la mesure où aucune SID n’est disponible dans une session Bureau à distance, le script ne fonctionne pas dans une session Bureau à distance. De même, il ne fonctionne pas dans une session amélioré Hyper-V. Si vous testez un accès à distance toujours sur VPN sur des ordinateurs virtuels, désactiver session étendu sur votre client d’ordinateurs virtuels avant d’exécuter ce script.

L’exemple de script suivant inclut tous les exemples de code dans les sections précédentes. Assurez-vous que vous remplacez les valeurs de l’exemple par les valeurs qui conviennent à votre environnement.
    
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

## Configurer le client VPN à l’aide de Windows PowerShell

Pour configurer le fournisseur CSP VPNv2 sur un ordinateur client Windows 10, exécutez le script VPN_Profile.ps1 Windows PowerShell que vous avez créé dans la section [créer le fichier XML de profil](#create-the-profile-xml) . Ouvrez Windows PowerShell en tant qu’administrateur; dans le cas contraire, vous recevrez un message d’erreur indiquant, _accès refusé_.

Après avoir exécuté VPN_Profile.ps1 pour configurer le profil VPN, vous pouvez vérifier à tout moment si elle a réussi en exécutant la commande suivante dans Windows PowerShell ISE:

    Get-WmiObject -Namespace root\cimv2\mdm\dmmap -Class MDM_VPNv2_01

**Réussite résultant de l’applet de commande Get-WmiObject**


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

La configuration de ProfileXML doit être correcte dans la structure, l’orthographe, la configuration et parfois les majuscules. Si vous voyez quelque chose de différent dans la structure de liste 1, le balisage ProfileXML probablement contient une erreur.

Si vous avez besoin résoudre les problèmes de balisage, il est plus facile de le placer dans un éditeur XML qu’à résoudre dans Windows PowerShell ISE. Dans les deux cas, démarrer avec la version la plus simple du profil et ajouter des composants de revenir à la fois jusqu'à ce que le problème se produit à nouveau.

## <a name="vpn-deploy-client-sccm"></a>Configurer le client VPN à l’aide de System Center Configuration Manager

Dans System Center Configuration Manager, vous pouvez déployer des profils VPN à l’aide du nœud ProfileXML CSP, tout comme vous l’avez fait dans Windows PowerShell. Ici, vous utilisez le script VPN_Profile.ps1 Windows PowerShell que vous avez créé dans la section [créer les fichiers de configuration ProfileXML](#bkmk_ProfileXML).

Pour utiliser System Center Configuration Manager pour déployer un profil d’accès à distance toujours sur VPN sur les ordinateurs clients Windows 10, vous devez démarrer en créant un groupe d’ordinateurs ou des utilisateurs auxquels vous déployez le profil. Dans ce scénario, créez un groupe d’utilisateurs pour déployer le script de configuration.

### Créer un groupe d’utilisateurs

1.  Dans la console Configuration Manager, ouvrez actifs et Compliance\\User Collections.

2.  Sur le ruban **accueil** , dans le groupe de **créer** , cliquez sur **Créer un regroupement d’utilisateur**.

3.  Sur la page Général, procédez comme suit:

    a. Dans la zone **nom**, tapez **Les utilisateurs VPN**.

    b. Cliquez sur **Parcourir**, cliquez sur **Tous les utilisateurs** et cliquez sur **OK**.

    c. Cliquez sur **Suivant**.

4.  Sur la page de règles d’adhésion, procédez comme suit:

    a.  Dans les **règles d’adhésion**, cliquez sur **Ajouter une règle**, puis cliquez sur **Règle directe**. Dans cet exemple, que vous ajoutez des utilisateurs individuels à la collection de l’utilisateur. Toutefois, vous pouvez utiliser une règle de requête pour ajouter des utilisateurs à cette collection dynamiquement pour un déploiement à grande échelle.

    b.  Dans la page **Bienvenue** , cliquez sur **suivant**.

    c.  Sur la page Rechercher des ressources, dans la **valeur**, tapez le nom de l’utilisateur que vous voulez ajouter. Le nom de ressource inclut le domaine de l’utilisateur. Pour inclure des résultats basés sur une correspondance partielle, insérez le **%** caractère aux extrémités de vos critères de recherche. Par exemple, pour rechercher tous les utilisateurs contenant la chaîne «lori», tapez **% lori %**. Cliquez sur **Suivant**.

    d.  Sur la page Sélectionner les ressources, sélectionnez les utilisateurs auxquels que vous souhaitez ajouter au groupe, puis cliquez sur **suivant**.

    e.  Dans la page Résumé, cliquez sur **suivant**.

    f.  Sur la page Achèvement, cliquez sur **Fermer**.

6.  Retour sur la page de règles d’adhésion de l’Assistant créer un regroupement utilisateur, cliquez sur **suivant**.

7.  Dans la page Résumé, cliquez sur **suivant**.

8.  Sur la page Achèvement, cliquez sur **Fermer**.

Après avoir créé le groupe d’utilisateurs pour recevoir le profil VPN, vous pouvez créer un package et un programme pour déployer le script de configuration de Windows PowerShell que vous avez créé dans la section [créer les fichiers de configuration ProfileXML](#bkmk_ProfileXML).

### Créer un package contenant le script de configuration ProfileXML

1.  Héberger le script VPN_Profile.ps1 sur un partage réseau auquel le compte d’ordinateur server site peut accéder.

2.  Dans la console Configuration Manager, ouvrez le **Logiciel Library\\Application applications\\packages**.

3.  Sur le ruban **accueil** , dans le groupe de **créer** , cliquez sur **Créer un Package** pour démarrer l’Assistant de programme et de créer un Package.

4.  Sur la page Package, procédez comme suit:

    a. Dans la zone **nom**, tapez **Windows 10 toujours sur profil VPN**.

    b. Activez la case à cocher **ce package contient des fichiers sources** , puis cliquez sur **Parcourir**.

    c. Dans la boîte de dialogue dossier de définir la Source, cliquez sur **Parcourir**, sélectionnez le partage de fichiers contenant VPN_Profile.ps1, puis cliquez sur **OK**.<p>Assurez-vous que vous sélectionnez un chemin d’accès réseau, pas un chemin d’accès local. En d’autres termes, le chemin d’accès doit être quelque chose comme *\\fileserver\\vpnscript*, pas *c:\\vpnscript*.

1.  Cliquez sur **Suivant**.

2.  Sur la page Type de programme, cliquez sur **suivant**.

3.  Sur la page programme Standard, procédez comme suit:

    a.  Dans la zone **nom**, tapez le **Script de profil VPN**.

    b.  Dans la **ligne de commande**, tapez **PowerShell.exe - ExecutionPolicy contourner - fichier «VPN_Profile.ps1»**.

    c.  Dans le **mode d’exécution**, cliquez sur **exécuter avec des droits d’administration**.

    d.  Cliquez sur **Suivant**.

4.  Sur la page Configuration requise, procédez comme suit:

    a.  Sélectionnez **ce programme peut s’exécuter uniquement sur les plateformes spécifiés**.

    b.  Activez les cases à cocher **tous les Windows 10 (32 bits)** et **tous les Windows 10 (64 bits)** .

    c.  Dans l' **espace disque estimé**, tapez **1**.

    d.  Dans **maximale autorisée moment de l’exécution (minutes)**, tapez **15**.

    e.  Cliquez sur **Suivant**.

5.  Dans la page Résumé, cliquez sur **suivant**.

6.  Sur la page Achèvement, cliquez sur **Fermer**.

Avec le package et un programme créé, vous devez déployer vers le groupe **d’Utilisateurs VPN** .

### Déployer le script de configuration ProfileXML

1.  Dans la console Configuration Manager, ouvrez le logiciel Library\\Application applications\\packages.

2.  Dans les **Packages**, cliquez sur **Windows 10 toujours sur profil VPN**.

3.  Sous l’onglet **programmes** , en bas du volet d’informations, avec le bouton droit de **Script de profil VPN**, cliquez sur **Propriétés**et procédez comme suit:

    a.  Dans l’onglet **Avancé** , **lorsque ce programme est attribué à un ordinateur**, cliquez sur **une fois pour chaque utilisateur qui ouvre une session**.

    b.  Cliquez sur **OK**.

4.  Avec le bouton droit de **Script de profil VPN** et cliquez sur **déployer** pour démarrer l’Assistant déploiement logiciel.

5.  Sur la page Général, procédez comme suit:

    a.  En regard de la **Collection**, cliquez sur **Parcourir**.

    b.  Dans la liste des **Types de Collection** (coin supérieur gauche), cliquez sur **Collections d’utilisateurs**.

    c.  **Les utilisateurs VPN**, puis cliquez sur **OK**.

    d.  Cliquez sur **Suivant**.

6.  Sur la page de contenu, procédez comme suit:

    a.  Cliquez sur **Ajouter**, puis cliquez sur **Point de Distribution**.

    b.  Dans les **points de distribution disponibles**, sélectionnez les points de distribution auquel vous souhaitez distribuer le script de configuration de ProfileXML, puis cliquez sur **OK**.

    c.  Cliquez sur **Suivant**.

7.  Sur la page de paramètres de déploiement, cliquez sur **suivant**.

8.  Sur la page de planification, procédez comme suit:

    a.  Cliquez sur **Nouveau** pour ouvrir la boîte de dialogue calendrier d’attribution.

    b.  **Attribuer immédiatement après cet événement**, puis cliquez sur **OK**.

    c.  Cliquez sur **Suivant**.

9.  Sur la page expérience utilisateur, procédez comme suit:

    1.  Sélectionnez la case à cocher **Installation du logiciel** .

    2.  Cliquez sur **le résumé**.

10. Dans la page Résumé, cliquez sur **suivant**.

11. Sur la page Achèvement, cliquez sur **Fermer**.

Avec le script de configuration ProfileXML déployé, connectez-vous à un ordinateur de client Windows 10 avec le compte d’utilisateur que vous avez sélectionné lorsque vous avez créé la collection de l’utilisateur. Vérifier la configuration du client VPN.

>[!NOTE]
>Le script VPN_Profile.ps1 ne fonctionne pas dans une session Bureau à distance. De même, il ne fonctionne pas dans une session amélioré Hyper-V. Si vous testez un accès à distance toujours sur VPN sur des ordinateurs virtuels, désactiver session étendu sur votre client d’ordinateurs virtuels avant de continuer.

### Vérifier la configuration du client VPN

1.  Dans le panneau de configuration, sous **System\\Security**, cliquez sur **Le Gestionnaire de Configuration**. 

2.  Dans la boîte de dialogue Gestionnaire de Configuration de propriétés, sous l’onglet **Actions** , procédez comme suit:

    a.  Cliquez sur **& de récupération de stratégie ordinateur et Cycle d’évaluation**, cliquez sur **Exécuter maintenant**, puis cliquez sur **OK**.

    b.  Cliquez sur **& de récupération de stratégie utilisateur et Cycle d’évaluation**, cliquez sur **Exécuter maintenant**, puis cliquez sur **OK**.

    c.  Cliquez sur **OK**.

3.  Fermez le panneau de configuration.

Vous devriez voir le nouveau profil VPN peu de temps.

## Configurer le client VPN à l’aide de Intune

Pour utiliser Intune pour déployer des profils VPN Windows 10 distant accès toujours sur, vous pouvez configurer le nœud ProfileXML CSP en utilisant le profil VPN que vous avez créé dans la section [créer les fichiers de configuration ProfileXML](#bkmk_ProfileXML), ou vous pouvez utiliser la base exemple EAP XML fourni ci-dessous.

>[!NOTE]
>Intune utilise désormais des groupes Azure AD. Si les utilisateurs sont affectés au groupe d’utilisateurs VPN et Azure AD Connect synchronisées avec le groupe d’utilisateurs VPN local à Azure AD, vous êtes prêt à continuer.

Créez la stratégie de configuration d’appareil VPN pour configurer les ordinateurs clients Windows 10 pour tous les utilisateurs ajoutés au groupe. Dans la mesure où le modèle de Intune fournit des paramètres VPN, copiez uniquement la partie \<EapHostConfig> \</EapHostConfig> du fichier VPN_ProfileXML. 


### Créer la stratégie de configuration toujours actif (AlwaysOn)

1.  Connectez-vous au [portail Azure](https://portal.azure.com/).

2.  Accédez à **Intune** > **Configuration du périphérique** > **profils**.

3.  Cliquez sur **Créer un profil** pour démarrer l’Assistant créer un profil.

4.  Entrez un **nom** pour le profil VPN et une description (facultative).

5.   Sous de la **plateforme**, sélectionnez **Windows 10 ou version ultérieure**et choisissez **VPN** dans le profil type liste déroulante s’affiche.

     >[!TIP]
     >Si vous créez un profileXML VPN personnalisé, voir [ProfileXML s’appliquent à l’aide d’Intune](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-profile-options#apply-profilexml-using-intune) pour les instructions.

6. Sous l’onglet **VPN de Base** , vérifiez ou définissez les paramètres suivants:

    - **Nom de la connexion:** Entrez le nom de la connexion VPN tel qu’il apparaît sur l’ordinateur client dans l’onglet **VPN** sous **paramètres**, par exemple, _Contoso AutoVPN_.  
    
    - **Serveurs:** Ajoutez un ou plusieurs serveurs VPN en cliquant sur **Ajouter.**
    
    - **Description** et **adresse IP ou le nom de domaine complet:** Entrez la description et l’adresse IP ou un nom de domaine complet du serveur VPN. Ces valeurs doivent être alignée avec le nom du sujet dans le certificat d’authentification du serveur VPN. 
    
    - **Serveur par défaut:** Si c’est le serveur VPN par défaut, la valeur **True**. Cette opération permet de ce serveur en tant que le serveur par défaut qui utilisent des appareils pour établir la connexion. 
    
    - **Type de connexion:** Définissez sur **IKEv2**.  
    
    - **Toujours activé:** Définie pour **l’Activer** pour vous connecter au réseau VPN automatiquement lors de la connexion et rester connecté jusqu'à ce que l’utilisateur se déconnecte manuellement.
    
    - **Mémoriser les informations d’identification à chaque ouverture de session**: valeur booléenne (true ou false) pour la mise en cache des informations d’identification. Si définie sur true, informations d’identification est mises en cache autant que possible.

7.  Copiez la chaîne de code XML suivante dans un éditeur de texte:<p>
 
    [!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]
    <p>
    
    ```XML
    <EapHostConfig xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><CredentialsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>false</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</AcceptServerName></EapType></Eap><EnableQuarantineChecks>false</EnableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</AcceptServerName></PeapExtensions></EapType></Eap></Config></EapHostConfig>
    ```

8.  Remplacez le **1 \<TrustedRootCA>5a 89 fe cb 5 b 49 a7 0 b a 52 63 b7 35 ee d7 1C c2 68 être 4b<\/TrustedRootCA>** dans l’exemple avec l’empreinte de certificat de votre autorité de certification racine locaux aux deux emplacements.
  
    >[!Important]
    >N’utilisez pas l’empreinte numérique exemple dans la section \<TrustedRootCA>\</TrustedRootCA> ci-dessous.  Le TrustedRootCA doit être l’empreinte de certificat de l’autorité de certification racine locaux qui a émis le certificat d’authentification du serveur pour les serveurs RRAS et NPS. **Cela ne doit pas être le certificat racine du cloud, ni l’empreinte de certificat d’autorité de certification émettrice intermédiaire**.

10. Remplacez le **\<ServerNames>NPS.contoso.com\</ServerNames>** dans l’exemple XML par le nom de domaine complet du serveur NPS joints au domaine où l’authentification a lieu. 

11. Copiez la chaîne XML révisée et coller dans la zone **EAP Xml** sous l’onglet VPN de Base et cliquez sur **OK**.<p>Une stratégie toujours sur appareil Configuration VPN à l’aide du protocole EAP est créée dans Intune.


### Synchronisation de la stratégie de configuration toujours actif (AlwaysOn) avec Intune

Pour tester la stratégie de configuration, se connecter à un ordinateur de client Windows 10 en tant que l’utilisateur que vous avez ajouté au groupe **Toujours sur les utilisateurs VPN** et puis synchroniser avec Intune.

1.  Dans le menu Démarrer, cliquez sur **les paramètres**.

2.  Dans les paramètres, cliquez sur **les comptes**, puis cliquez sur **accès Professionnel ou scolaire**.

3.  Cliquez sur le profil de GPM, puis cliquez sur les **informations**.

4.  Cliquez sur **la synchronisation** pour forcer une évaluation de la stratégie Intune et l’extraction.

5.  Fermer les paramètres. Après la synchronisation, vous voyez le profil VPN disponible sur l’ordinateur.

## Étape suivante
Vous avez terminé le déploiement toujours actif (AlwaysOn).  Pour d’autres fonctionnalités, que vous pouvez configurer, consultez le tableau ci-dessous:

|Si vous souhaitez...  |Alors consultez...  |
|---------|---------|
|Configurer l’accès conditionnel pour le VPN    |[Étape 7. (Facultatif) Configurer l’accès conditionnel pour une connexion VPN à l’aide d’Azure AD](../../ad-ca-vpn-connectivity-windows10.md): dans cette étape, vous pouvez affiner un accès VPN aux utilisateurs comment autorisé à vos ressources à l’aide de [l’accès conditionnel Azure Active Directory (AD Azure)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Avec l’accès conditionnel à Azure AD pour la connectivité réseau privé virtuel (VPN), vous pouvez protéger les connexions VPN. L’accès conditionnel est un moteur d’évaluation basé sur les stratégies qui vous permet de créer des règles d’accès pour n’importe quelle application connectée Azure Active Directory (Azure AD).         |
|En savoir plus sur les fonctionnalités avancées de VPN  |[Fonctionnalités avancées de VPN](always-on-vpn-adv-options.md#advanced-vpn-features): cette page fournit des conseils sur l’activation des filtres de trafic VPN, la configuration de connexions VPN automatiques à l’aide de déclencheurs d’application et comment configurer un serveur NPS pour autoriser uniquement les connexions VPN à partir de clients utilisant des certificats émis par Azure AD.        |


---
