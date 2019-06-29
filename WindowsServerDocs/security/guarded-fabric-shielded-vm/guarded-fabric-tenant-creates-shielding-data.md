---
title: Machines virtuelles protégées pour les locataires - création des données de protection pour définir une machine virtuelle protégée
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 49f4e84d-c1f7-45e5-9143-e7ebbb2ef052
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 01/30/2019
ms.openlocfilehash: d1d269ecdbfd4803c51da4817b62caf01d2091ae
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469614"
---
# <a name="shielded-vms-for-tenants---creating-shielding-data-to-define-a-shielded-vm"></a>Machines virtuelles protégées pour les locataires - création des données de protection pour définir une machine virtuelle protégée

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Un fichier de données de protection (également appelé « fichier de données d’approvisionnement » ou « fichier PDK ») est un fichier chiffré créé par un locataire ou un propriétaire de machine virtuelle pour protéger les informations clés de configuration de la machine virtuelle, telles que le mot de passe de l’administrateur, les certificats RDP et autres certificats liés aux identités, les informations d’identification de jonction de domaine, etc. Cette rubrique fournit des informations sur la création d’un fichier de données de protection. Avant de pouvoir créer le fichier, vous devez obtenir un disque de modèle à partir de votre fournisseur de services, ou créez un disque de modèle comme décrit dans [machines virtuelles protégées pour les locataires - création d’un disque de modèle (facultatif)](guarded-fabric-tenant-creates-template-disk.md).

Pour obtenir la liste et un diagramme du contenu d’un fichier de données de protection, consultez [ce qui est des données de protection et pourquoi est-il nécessaire ?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

> [!IMPORTANT]
> Les étapes décrites dans cette section doivent être effectuées sur un ordinateur client exécutant Windows Server 2016. Cet ordinateur ne doit pas faire partie d’une structure protégée (autrement dit, ne doivent pas être configurés pour utiliser un cluster SGH).

Pour vous préparer à créer un fichier de données de protection, procédez comme suit :

- [Obtenir un certificat pour la connexion Bureau à distance](#obtain-a-certificate-for-remote-desktop-connection)
- [Créer un fichier de réponses](#create-an-answer-file)
- [Obtenir le fichier de catalogue de signatures de volume](#get-the-volume-signature-catalog-file)
- [Sélectionnez les infrastructures de confiance](#select-trusted-fabrics)

Vous pouvez ensuite créer le fichier de données de protection :

- [Créer un fichier de données de protection et ajoutez les gardiens](#create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard)


## <a name="obtain-a-certificate-for-remote-desktop-connection"></a>Obtenir un certificat pour la connexion Bureau à distance

Dans la mesure où les clients peuvent uniquement se connecter à leurs machines virtuelles protégées, à l’aide de la connexion Bureau à distance ou autres outils de gestion à distance, il est important de s’assurer que les clients peuvent vérifier auquel il se connecte au point de terminaison droit (autrement dit, il n’est pas un « l’homme au milieu » interception de la connexion).

Pour vérifier que vous vous connectez au serveur voulu un moyen consiste à installer et configurer un certificat pour les Services Bureau à distance présenter lorsque vous lancez une connexion. L’ordinateur client se connectant au serveur vérifiera si il approuve le certificat et affiche un avertissement si elle n’est pas le cas. En règle générale, pour garantir que le client qui se connecte approuve le certificat, les certificats RDP sont émis à partir de l’infrastructure à clé publique du locataire. Plus d’informations sur [à l’aide de certificats dans les Services Bureau à distance](https://technet.microsoft.com/library/dn781533.aspx) peut être disponible sur TechNet.

> [!NOTE]
> Lorsque vous sélectionnez un certificat RDP à inclure dans votre fichier de données de protection, veillez à utiliser un certificat générique. Un fichier de données de protection peut être utilisé pour créer un nombre illimité de machines virtuelles. Étant donné que chaque machine virtuelle partagent le même certificat, un certificat générique permet de garantir le certificat valide, quel que soit le nom d’hôte de la machine virtuelle.

Si vous évaluez les machines virtuelles protégées et ne sont pas encore prêt à demander un certificat à partir de votre autorité de certification, vous pouvez créer un certificat auto-signé sur l’ordinateur client en exécutant la commande Windows PowerShell suivante (où *contoso.com*  est le domaine du locataire) :

``` powershell
$rdpCertificate = New-SelfSignedCertificate -DnsName '\*.contoso.com'
$password = ConvertTo-SecureString -AsPlainText 'Password1' -Force
Export-PfxCertificate -Cert $RdpCertificate -FilePath .\rdpCert.pfx -Password $password
```

## <a name="create-an-answer-file"></a>Créer un fichier de réponses

Étant donné que le disque de modèle signés dans VMM est généralisé, clients sont requis pour fournir un fichier de réponses afin de spécialiser leurs machines virtuelles protégées pendant le processus d’approvisionnement. Le fichier de réponses (souvent appelé le fichier d’installation sans assistance) permettre configurer la machine virtuelle pour son rôle prévu : autrement dit, il peut installer des fonctionnalités de Windows, inscrire le certificat RDP créé à l’étape précédente et effectuer d’autres actions personnalisées. Il fournit également des informations requises pour le programme d’installation Windows, y compris la clé de produit et le mot de passe de l’administrateur par défaut.

Pour plus d’informations sur l’obtention et à l’aide de la **New-ShieldingDataAnswerFile** fonction permettant de générer un fichier de réponses (fichier Unattend.xml) pour la création de machines virtuelles protégées, consultez [générer un fichier de réponses à l’aide de la Fonction de la nouvelle-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md). Vous pouvez plus facilement à l’aide de la fonction, pour générer un fichier de réponses qui reflète le choix tels que les éléments suivants :

- La machine virtuelle est destinée à être joints à la fin du processus d’initialisation ?
- Vous utiliserez une licence en volume ou une clé de produit spécifique par machine virtuelle ?
- Si vous utilisez DHCP ou adresse IP statique ?
- Allez-vous utiliser un certificat de protocole RDP (Remote Desktop) qui sera utilisé pour prouver que la machine virtuelle appartient à votre organisation ?
- Vous souhaitez exécuter un script à la fin de l’initialisation
- Vous utilisez un serveur Desired State Configuration (DSC) pour poursuivre la configuration ?

Utilisé dans les fichiers de données de protection des fichiers de réponses seront utilisés sur chaque machine virtuelle créée à l’aide de ce fichier de données de protection. Par conséquent, il se peut que vous devez vous assurer que vous ne codez pas dur toutes les informations spécifiques à la machine virtuelle dans le fichier de réponses. VMM prend en charge certaines chaînes de substitution (voir le tableau ci-dessous) dans le fichier d’installation sans assistance pour gérer les valeurs de spécialisation qui peuvent changer d’une machine virtuelle à machine virtuelle. Vous n’êtes pas obligé de les utiliser ; Toutefois, s’ils sont présents VMM profitera d'entre eux.

Lorsque vous créez un fichier unattend.xml pour les machines virtuelles protégées, gardez à l’esprit les restrictions suivantes :

-   Le fichier d’installation sans assistance doit avoir pour résultat de la machine virtuelle est désactivée une fois qu’il a été configuré. Il s’agit pour que VMM puisse savoir quand il doit signaler au client que la machine virtuelle approvisionné et est prêt à être utilisé. VMM sera automatiquement la machine virtuelle précédent mettre sous tension une fois qu’il détecte qu’il a été désactivé lors de l’approvisionnement.

-   Il est fortement recommandé de configurer un certificat RDP pour vous assurer que vous vous connectez à la machine virtuelle de droite et pas un autre ordinateur configuré pour une attaque man-in-the-middle.

-   Veillez à activer RDP et la règle de pare-feu correspondante pour pouvoir accéder à la machine virtuelle après que qu’il a été configuré. Vous ne pouvez pas utiliser la console VMM pour les machines virtuelles protégées des accès, vous devez RDP pour se connecter à votre machine virtuelle. Si vous préférez gérer vos systèmes de communication à distance de Windows PowerShell, vérifiez que WinRM est activé, trop.

-   Les chaînes de substitution uniquement pris en charge dans les fichiers d’installation sans assistance machine virtuelle protégées sont les suivantes :

| Élément remplaçable | Chaîne de substitution |
|-----------|-----------|
| ComputerName        | @ComputerName@      |
| TimeZone            | @TimeZone@          |
| ProductKey          | @ProductKey@        |
| IPAddr4-1           | @IP4Addr-1@         |
| IPAddr6-1           | @IP6Addr-1@         |
| MACAddr-1           | @MACAddr-1@         |
| Préfixe-1-1          | @Prefix-1-1@        |
| NextHop-1-1         | @NextHop-1-1@       |
| Préfixe-1-2          | @Prefix-1-2@        |
| NextHop-1-2         | @NextHop-1-2@       |

Lorsque vous utilisez des chaînes de substitution, il est important de s’assurer que les chaînes seront renseignées lors de la machine virtuelle les processus d’approvisionnement. Si une chaîne telle que @ProductKey@ n’est pas fourni au moment du déploiement, en laissant le &lt;ProductKey&gt; nœud dans le fichier d’installation sans assistance vide, le processus de spécialisation échouera et vous ne pourrez pas vous connecter à votre machine virtuelle.

En outre, notez que les chaînes de substitution en matière de réseau vers la fin de la table sont utilisées uniquement si vous exploitez des Pools d’adresses IP statiques VMM. Votre fournisseur de services doit être en mesure de vous indiquer si ces chaînes de substitution sont nécessaires. Pour plus d’informations sur les adresses IP statiques dans les modèles VMM, consultez la rubrique suivante dans la documentation de VMM :

- [Instructions pour les pools d’adresses IP](https://technet.microsoft.com/system-center-docs/vmm/plan/plan-network#guidelines-for-ip-address-pools) 
- [Configurer des pools d’adresses IP statiques dans l’infrastructure VMM](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-network-static-address-pools)

Enfin, il est important de noter que le processus de déploiement de machine virtuelle protégé chiffrera uniquement le lecteur du système d’exploitation. Si vous déployez une machine virtuelle protégée avec un ou plusieurs disques de données, il est fortement recommandé que vous ajoutez une commande de d’installation sans assistance ou d’un paramètre de stratégie de groupe dans le domaine du client pour chiffrer automatiquement les lecteurs de données.

## <a name="get-the-volume-signature-catalog-file"></a>Obtenir le fichier de catalogue de signatures de volume

Fichiers de données de protection contiennent également des informations sur les disques de modèle fait confiance à un locataire. Clients d’acquérir les signatures de disque à partir de disques de modèle approuvé sous la forme d’un fichier de catalogue (VSC) de signature de volume. Ces signatures sont ensuite validées quand un nouvel ordinateur virtuel est déployé. Si aucune des signatures dans le fichier de données de protection correspond le disque de modèle tente de déployer avec la machine virtuelle (c'est-à-dire qu’elle a été modifiée ou échanger avec un disque différent et potentiellement malveillant), le processus d’approvisionnement échoue.

> [!IMPORTANT]
> Tandis que la VSC garantit qu’un disque n’a pas été falsifié, il est toujours important pour le client approuve le disque en premier lieu. Si vous êtes le locataire et le disque de modèle est fourni par votre hébergeur, déployer un machine virtuelle à l’aide de ce disque de modèle de test et exécuter vos propres outils (antivirus, des analyseurs de vulnérabilité et ainsi de suite) pour valider le disque sont, en fait, dans un état qui vous faites confiance.

Il existe deux façons d’acquérir la VSC d’un disque de modèle :

-  L’hébergeur (ou le client, si le client a accès à VMM) utilise les applets de commande PowerShell de VMM pour enregistrer la VSC et lui donne au client. Cela peut être effectuée sur n’importe quel ordinateur avec la console VMM installés et configurés pour gérer l’environnement de VMM de l’infrastructure d’hébergement. Les applets de commande PowerShell pour enregistrer la VSC sont :

        $disk = Get-SCVirtualHardDisk -Name "templateDisk.vhdx"
    
        $vsc = Get-SCVolumeSignatureCatalog -VirtualHardDisk $disk
    
        $vsc.WriteToFile(".\templateDisk.vsc")

-  Le client a accès au fichier de disque de modèle. Cela peut être le cas si le client crée un disque de modèle téléchargés vers un fournisseur de services ou si le client peut télécharger le disque de modèle de l’hébergeur. Dans ce cas, sans VMM dans l’image, le locataire exécutez l’applet de commande suivante (installé avec la fonctionnalité Outils de machine virtuelle protégée, qui fait partie des outils d’Administration de serveur distant) :

        Save-VolumeSignatureCatalog -TemplateDiskPath templateDisk.vhdx -VolumeSignatureCatalogPath templateDisk.vsc

## <a name="select-trusted-fabrics"></a>Sélectionnez les infrastructures de confiance

Le dernier composant dans le fichier de données de protection se rapporte au propriétaire et les gardiens d’une machine virtuelle. Gardiens sont utilisés pour désigner les deux le propriétaire d’une machine virtuelle protégée et structures protégées sur lequel il est autorisé à exécuter.

Pour autoriser une infrastructure d’hébergement pour exécuter une machine virtuelle protégée, vous devez obtenir les métadonnées de Service de surveillance d’hôte du fournisseur service d’hébergement. Souvent, le fournisseur de services vous fournira ces métadonnées via leurs outils de gestion. Dans un scénario d’entreprise, vous pouvez accéder directement pour obtenir les métadonnées vous-même.

Vous ou votre fournisseur de services peut obtenir les métadonnées à partir de SGH en effectuant l’une des actions suivantes :

-  Obtenir les métadonnées directement à partir de SGH en exécutant la commande Windows PowerShell suivante, ou en accédant au site Web et en enregistrant le fichier XML qui s’affiche :

        Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml

-  Obtenir les métadonnées à partir de VMM à l’aide des applets de commande PowerShell de VMM :

        $relecloudmetadata = Get-SCGuardianConfiguration

        $relecloudmetadata.InnerXml | Out-File .\RelecloudGuardian.xml -Encoding UTF8

Obtenir la surveillance des fichiers de métadonnées pour chaque infrastructure protégée que vous souhaitez autoriser vos machines virtuelles protégées à exécuter sur avant de continuer.

## <a name="create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard"></a>Créer un fichier de données de protection et ajoutez les gardiens à l’aide de l’Assistant de protection des fichiers de données

Exécutez l’Assistant de protection de fichier de données pour créer un fichier de données (PDK) de protection. Ici, vous allez ajouter le certificat RDP, d’installation sans assistance de fichier, les catalogues de signature de volume, guardian de propriétaire et les métadonnées téléchargé obtenu à l’étape précédente.

1.  Installer **outils d’Administration de serveur distant &gt; Feature Administration Tools &gt; outils de machine virtuelle protégée** sur votre ordinateur à l’aide du Gestionnaire de serveur ou de la commande Windows PowerShell suivante :

        Install-WindowsFeature RSAT-Shielded-VM-Tools

2.  Ouvrir l’Assistant de protection d’un fichier de données à partir de la section Outils d’administration dans votre menu Démarrer ou en exécutant le fichier exécutable suivant **C:\\Windows\\System32\\ShieldingDataFileWizard.exe**.

3.  Sur la première page, utilisez la deuxième zone de sélection de fichier pour choisir un emplacement et un nom pour votre fichier de données de protection. En règle générale, vous devez nommer un fichier de données de protection après la création de l’entité qui possède toutes les machines virtuelles avec des données de cette protection (par exemple, ressources humaines, informatique, Finance) et le rôle de la charge de travail qu’il est en cours d’exécution (par exemple, serveur de fichiers, serveur web ou tout autre élément configuré par le fichier d’installation sans assistance). Laissez la case d’option définie sur **protection des données pour les modèles de blindé**.

    > [!NOTE]
    > Dans l’Assistant de protection d’un fichier de données, vous remarquerez deux options ci-dessous :
    >- **Données de protection pour les modèles de protégés**
    >- **Les données de protection pour les machines virtuelles existantes et les modèles non protégé**<br>
    > La première option est utilisée lors de la création de machines virtuelles protégées à partir des modèles protégés. La deuxième option permet de créer des données de protection qui est utilisable uniquement lorsque les machines virtuelles existantes de conversion ou la création de machines virtuelles protégées à partir de modèles non protégé.

    ![Assistant fichier de données de protection, sélection de fichiers](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-01.png)

       En outre, vous devez choisir si les machines virtuelles créées à l’aide de ce fichier de données de protection sera véritablement protégée ou configuré en mode « chiffrement pris en charge ». Pour plus d’informations sur ces deux options, consultez [quels sont les types de machines virtuelles qui a une structure protégée peut exécuter ?](guarded-fabric-and-shielded-vms.md#what-are-the-types-of-virtual-machines-that-a-guarded-fabric-can-run).

    > [!IMPORTANT]
    > Soyez très attentif à l’étape suivante car il définit le propriétaire de vos machines virtuelles protégées et qui vos machines virtuelles protégées seront autorisées à s’exécuter sur des infrastructures.<br>Possession de **guardian de propriétaire** est requis pour modifier ultérieurement protégées d’une machine virtuelle existante à partir de **blindé** à **chiffrement pris en charge** ou vice versa.
    
4.  Votre objectif de cette étape est double :

    - Créez ou sélectionnez un tuteur propriétaire qui vous représente le propriétaire de la machine virtuelle

    - Importer le gardien que vous avez téléchargé à partir du fournisseur d’hébergement (ou votre propre) Service de surveillance d’hôte à l’étape précédente

    Pour désigner un tuteur propriétaire existant, sélectionnez le gardien approprié dans le menu déroulant. Uniquement les gardiens installés sur votre ordinateur local avec les clés privées intacts seront afficheront dans cette liste. Vous pouvez également créer vos propres guardian propriétaire en sélectionnant **gérer les gardiens Local** dans la partie inférieure droite et en cliquant sur **créer** et de fin de l’Assistant.

    Ensuite, nous importer les métadonnées téléchargée précédemment à l’aide de la **propriétaire et gardiens** page. Sélectionnez **gérer les gardiens Local** à partir de l’angle inférieur droit. Utilisez le **importer** fonctionnalité pour importer le fichier de métadonnées guardian. Cliquez sur **OK** une fois que vous avez importé ou ajouté tous les gardiens nécessaires. Comme meilleure pratique, nom gardiens après l’hébergement service fournisseur enterprise centre de données ou qu’ils représentent. Enfin, sélectionnez tous les gardiens qui représentent les centres de données dans laquelle votre machine virtuelle protégée est autorisé à exécuter. Vous n’avez pas besoin resélectionner le gardien propriétaire. Cliquez sur **suivant** une fois terminé.

    ![Assistant fichier de données, propriétaire et gardiens de protection](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-02.png)

5.  Dans la page des qualificateurs ID de Volume, cliquez sur **ajouter** pour autoriser un disque de modèle signés dans votre fichier de données de protection. Lorsque vous sélectionnez un VSC dans la boîte de dialogue, il s’affiche des informations sur son nom, version et le certificat qui a été utilisé pour la signature. Répétez ce processus pour chaque disque de modèle que vous souhaitez autoriser.

6.  Sur le **spécialisation valeurs** , cliquez sur **Parcourir** pour sélectionner votre fichier unattend.xml qui sera utilisé pour spécialiser vos machines virtuelles.

    Utilisez le **ajouter** bouton du bas pour ajouter des fichiers supplémentaires à PDK qui sont nécessaires au cours du processus de spécialisation. Par exemple, si votre fichier d’installation sans assistance est l’installation d’un certificat RDP sur la machine virtuelle (comme décrit dans [générer un fichier de réponses à l’aide de la fonction New-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md)), vous devez ajouter le fichier RDPCert.pfx référencé dans le fichier d’installation sans assistance. Notez que tous les fichiers que vous spécifiez ici seront automatiquement copié dans C:\\temp\\ sur la machine virtuelle qui est créée. Votre fichier d’installation sans assistance doit attendre les fichiers doivent être dans ce dossier quand vous les référencez par chemin d’accès.

7.  Passez en revue vos sélections dans la page suivante, puis cliquez sur **générer**.

8.  Fermez l’Assistant une fois celle-ci terminée.

## <a name="create-a-shielding-data-file-and-add-guardians-using-powershell"></a>Créer un fichier de données de protection et ajoutez les gardiens à l’aide de PowerShell

Comme alternative à l’Assistant de protection des fichiers de données, vous pouvez exécuter [New-ShieldingDataFile](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/new-shieldingdatafile?view=win10-ps) pour créer un fichier de données de protection.

Tous les fichiers de données de protection doivent être configurés avec le propriétaire et les certificats de guardian pour autoriser vos machines virtuelles protégées à être exécuté sur une infrastructure protégée.
Vous pouvez vérifier si vous avez les gardiens installés localement en exécutant [Get-HgsGuardian](https://docs.microsoft.com/powershell/module/hgsclient/get-hgsguardian?view=win10-ps). Contrairement à gardiens pour votre centre de données généralement gardiens du propriétaire ont une clé privée.

Si vous avez besoin créer un tuteur propriétaire, exécutez la commande suivante :

```powershell
New-HgsGuardian -Name "Owner" -GenerateCertificates
```

Cette commande crée une paire de certificats de signature et chiffrement dans le magasin de certificats de l’ordinateur local sous le dossier « Certificats Local Machine virtuelle protégée ».
Vous devrez les certificats de propriétaire et leurs clés privées correspondantes pour unshield une machine virtuelle, par conséquent, vérifiez ces certificats sont sauvegardés et protégés contre le vol.
Un pirate ayant accès aux certificats propriétaire peut les utiliser pour démarrer votre machine virtuelle protégée ou modifier sa configuration de sécurité.

Si vous avez besoin importer des informations de surveillance à partir d’une structure protégée, où vous souhaitez exécuter votre machine virtuelle (votre centre de données principal, centres de données de sauvegarde, etc.), exécutez la commande suivante pour chaque [fichier de métadonnées récupérées à partir de vos infrastructures protégées ](#select-trusted-fabrics).

```powershell
Import-HgsGuardian -Name 'EAST-US Datacenter' -Path '.\EastUSGuardian.xml'
```

> [!TIP]
> Si vous avez utilisé des certificats auto-signés ou les certificats inscrits avec SGH arrivées à expiration, vous devrez peut-être utiliser le `-AllowUntrustedRoot` et/ou `-AllowExpired` indicateurs avec la commande Import-HgsGuardian pour contourner les contrôles de sécurité.

Vous devrez également [obtenir un catalogue de signatures de volume](#get-the-volume-signature-catalog-file) pour chaque disque de modèle que vous souhaitez utiliser avec ce fichier de données de protection et un [fichier de réponses de données de protection](#create-an-answer-file) pour permettre au système d’exploitation terminer son spécialisation automatiquement des tâches.
Enfin, décidez si votre machine virtuelle doit être entièrement protégées ou simplement compatibles avec vTPM.
Utilisez `-Policy Shielded` pour une machine virtuelle entièrement protégée ou `-Policy EncryptionSupported` pour un vTPM activé la machine virtuelle qui autorise les connexions de console de base et PowerShell Direct.

Une fois que tout est prêt, exécutez la commande suivante pour créer votre fichier de données de protection :

```powershell
$viq = New-VolumeIDQualifier -VolumeSignatureCatalogFilePath 'C:\temp\marketing-ws2016.vsc' -VersionRule Equals
New-ShieldingDataFile -ShieldingDataFilePath "C:\temp\Marketing-LBI.pdk" -Policy EncryptionSupported -Owner 'Owner' -Guardian 'EAST-US Datacenter' -VolumeIDQualifier $viq -AnswerFile 'C:\temp\marketing-ws2016-answerfile.xml'
```

Dans la commande ci-dessus, le gardien nommé « Propriétaire » (obtenu à partir de Get-HgsGuardian) sera en mesure de modifier la configuration de la sécurité de la machine virtuelle à l’avenir, tandis que « Centre de données est des États-Unis » peut exécuter la machine virtuelle, mais pas modifier ses paramètres.
Si vous avez plusieurs guardian, distinct les noms des gardiens par des virgules tel que `'EAST-US Datacenter', 'EMEA Datacenter'`.
Le qualificateur d’ID de volume Spécifie si vous faites confiance uniquement la version exacte (égal à) du disque de modèle ou les versions ultérieures (GreaterThanOrEquals) également.
Le nom du disque et le certificat de signature doivent correspondre exactement pour la comparaison de version pris en considération au moment du déploiement.
Vous pouvez faire confiance à plusieurs disques de modèle en fournissant une liste séparée par des virgules de volume qualificateurs ID pour le `-VolumeIDQualifier` paramètre.
Enfin, si vous avez d’autres fichiers qui doivent accompagner le fichier de réponses avec la machine virtuelle, utilisez le `-OtherFile` paramètre et fournir une liste séparée par des virgules des chemins d’accès de fichier.

Consultez la documentation de l’applet de commande pour [New-ShieldingDataFile](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/New-ShieldingDataFile?view=win10-ps) et [New-VolumeIDQualifier](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/New-VolumeIDQualifier?view=win10-ps) pour en savoir plus sur les autres méthodes permettent de configurer votre fichier de données de protection.

## <a name="see-also"></a>Voir aussi

- [Déployer des machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Structure protégée et machines virtuelles dotées d’une protection maximale](guarded-fabric-and-shielded-vms-top-node.md)
