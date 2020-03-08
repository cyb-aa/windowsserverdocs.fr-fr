---
title: Machines virtuelles protégées pour les locataires-création de données de protection pour définir une machine virtuelle protégée
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 49f4e84d-c1f7-45e5-9143-e7ebbb2ef052
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 09/25/2019
ms.openlocfilehash: 26ff5e27494e2f42a0c8e4d28e2b9820f8d19e6a
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78370753"
---
# <a name="shielded-vms-for-tenants---creating-shielding-data-to-define-a-shielded-vm"></a>Machines virtuelles protégées pour les locataires-création de données de protection pour définir une machine virtuelle protégée

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Un fichier de données de protection (également appelé « fichier de données d’approvisionnement » ou « fichier PDK ») est un fichier chiffré créé par un locataire ou un propriétaire de machine virtuelle pour protéger les informations clés de configuration de la machine virtuelle, telles que le mot de passe de l’administrateur, les certificats RDP et autres certificats liés aux identités, les informations d’identification de jonction de domaine, etc. Cette rubrique fournit des informations sur la création d’un fichier de données de protection. Avant de pouvoir créer le fichier, vous devez obtenir un disque de modèle auprès de votre fournisseur de services d’hébergement ou créer un disque de modèle comme décrit dans [machines virtuelles protégées pour les locataires-création d’un disque de modèle (facultatif)](guarded-fabric-tenant-creates-template-disk.md).

Pour obtenir une liste et un diagramme du contenu d’un fichier de données de protection, consultez [qu’est-ce que les données de protection et pourquoi est-il nécessaire ?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

> [!IMPORTANT]
> Les étapes de cette section doivent être effectuées sur un ordinateur distinct et approuvé en dehors de l’infrastructure protégée. En règle générale, le propriétaire de la machine virtuelle (locataire) crée les données de protection de leurs machines virtuelles, et non les administrateurs de l’infrastructure.

Pour préparer la création d’un fichier de données de protection, procédez comme suit :

- [Obtenir un certificat pour Connexion Bureau à distance](#optional-obtain-a-certificate-for-remote-desktop-connection)
- [Créer un fichier de réponses](#create-an-answer-file)
- [Récupération du fichier catalogue des signatures de volume](#get-the-volume-signature-catalog-file)
- [Sélectionner des infrastructures de confiance](#select-trusted-fabrics)

Vous pouvez ensuite créer le fichier de données de protection :

- [Créer un fichier de données de protection et ajouter des gardiens](#create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard)

## <a name="optional-obtain-a-certificate-for-remote-desktop-connection"></a>Facultatif Obtenir un certificat pour Connexion Bureau à distance

Étant donné que les locataires sont uniquement en mesure de se connecter à leurs machines virtuelles protégées à l’aide d’Connexion Bureau à distance ou d’autres outils de gestion à distance, il est important de s’assurer que les locataires peuvent vérifier qu’ils se connectent au point de terminaison approprié (autrement dit, il n’y a pas de « Man au milieu ») interception de la connexion).

Une façon de vérifier que vous vous connectez au serveur souhaité consiste à installer et à configurer un certificat pour que Services Bureau à distance présente lorsque vous établissez une connexion. L’ordinateur client qui se connecte au serveur vérifie s’il approuve le certificat et affiche un avertissement dans le cas contraire. En règle générale, pour garantir que le client qui se connecte approuve le certificat, les certificats RDP sont émis à partir de l’infrastructure à clé publique du locataire. Vous trouverez plus d’informations sur [l’utilisation des certificats dans services Bureau à distance](https://technet.microsoft.com/library/dn781533.aspx) sur TechNet.

 Pour vous aider à déterminer si vous devez obtenir un certificat RDP personnalisé, prenez en compte les éléments suivants :

- Si vous testez simplement des machines virtuelles protégées dans un environnement Lab, vous **n’avez pas** besoin d’un certificat RDP personnalisé.
- Si votre machine virtuelle est configurée pour joindre un domaine Active Directory, un certificat d’ordinateur est généralement émis automatiquement par l’autorité de certification de votre organisation et utilisé pour identifier l’ordinateur lors des connexions RDP. Vous **n’avez pas** besoin d’un certificat RDP personnalisé.
- Si votre machine virtuelle n’est pas jointe à un domaine, mais que vous souhaitez disposer d’un moyen de vérifier que vous vous connectez à l’ordinateur approprié lorsque vous utilisez Bureau à distance, vous **devez envisager** d’utiliser des certificats RDP personnalisés.

> [!TIP]
> Lorsque vous sélectionnez un certificat RDP à inclure dans votre fichier de données de protection, veillez à utiliser un certificat générique. Un fichier de données de protection peut être utilisé pour créer un nombre illimité de machines virtuelles. Étant donné que chaque machine virtuelle partagera le même certificat, un certificat générique garantit que le certificat sera valide quel que soit le nom d’hôte de la machine virtuelle.

## <a name="create-an-answer-file"></a>Créer un fichier de réponses

Étant donné que le disque de modèle signé dans VMM est généralisé, les locataires sont tenus de fournir un fichier de réponses pour spécialiser leurs machines virtuelles protégées pendant le processus de configuration. Le fichier de réponses (souvent appelé fichier d’installation sans assistance) peut configurer la machine virtuelle pour son rôle prévu, c’est-à-dire qu’elle peut installer les fonctionnalités Windows, inscrire le certificat RDP créé à l’étape précédente et effectuer d’autres actions personnalisées. Il fournit également les informations requises pour l’installation de Windows, y compris le mot de passe de l’administrateur par défaut et la clé de produit.

Pour plus d’informations sur l’obtention et l’utilisation de la fonction **New-ShieldingDataAnswerFile** pour générer un fichier de réponses (fichier Unattend. Xml) pour créer des machines virtuelles protégées, consultez [générer un fichier de réponses à l’aide de la fonction New-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md). À l’aide de la fonction, vous pouvez plus facilement générer un fichier de réponses qui reflète des choix tels que les suivants :

- La machine virtuelle est-elle destinée à être jointe à un domaine à la fin du processus d’initialisation ?
- Utiliserez-vous une licence en volume ou une clé de produit spécifique par machine virtuelle ?
- Utilisez-vous DHCP ou une adresse IP statique ?
- Allez-vous utiliser un certificat de protocole RDP (Remote Desktop Protocol) personnalisé (RDP) qui sera utilisé pour prouver que la machine virtuelle appartient à votre organisation ?
- Voulez-vous exécuter un script à la fin de l’initialisation ?

Les fichiers de réponses utilisés dans les fichiers de données de protection seront utilisés sur chaque machine virtuelle créée à l’aide de ce fichier de données de protection. Par conséquent, vous devez veiller à ne pas coder en dur les informations spécifiques à la machine virtuelle dans le fichier de réponses. VMM prend en charge certaines chaînes de substitution (voir le tableau ci-dessous) dans le fichier d’installation sans assistance pour gérer les valeurs de spécialisation qui peuvent changer d’une machine virtuelle à une machine virtuelle. Vous n’êtes pas obligé d’utiliser ces ; Toutefois, s’ils sont présents, VMM en tire parti.

Lorsque vous créez un fichier Unattend. xml pour les machines virtuelles protégées, gardez à l’esprit les restrictions suivantes :

- Si vous utilisez VMM pour gérer votre centre de fichiers, le fichier d’installation sans assistance doit entraîner la désactivation de la machine virtuelle une fois qu’elle a été configurée. Cela permet à VMM de savoir quand il doit signaler au locataire que la machine virtuelle a été configurée et qu’elle est prête à être utilisée. VMM rétablit automatiquement la machine virtuelle une fois qu’elle a détecté qu’elle a été désactivée lors de la configuration.

- Veillez à activer le protocole RDP et la règle de pare-feu correspondante pour pouvoir accéder à la machine virtuelle une fois qu’elle a été configurée. Vous ne pouvez pas utiliser la console VMM pour accéder aux machines virtuelles protégées. vous aurez donc besoin du protocole RDP pour vous connecter à votre machine virtuelle. Si vous préférez gérer vos systèmes avec la communication à distance Windows PowerShell, assurez-vous également que WinRM est activé.

- Les seules chaînes de substitution prises en charge dans les fichiers d’installation sans assistance de la machine virtuelle protégée sont les suivantes :

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

    Si vous avez plusieurs cartes réseau, vous pouvez ajouter plusieurs chaînes de substitution pour la configuration IP en incrémentant le premier chiffre. Par exemple, pour définir l’adresse IPv4, le sous-réseau et la passerelle pour 2 cartes réseau, vous devez utiliser les chaînes de substitution suivantes :

    | Chaîne de substitution | Exemple de substitution |
    |---------------------|----------------------|
    | @IP4Addr-1@         | 192.168.1.10/24      |
    | @MACAddr-1@         | Ethernet             |
    | @Prefix-1-1@        | 24                   |
    | @NextHop-1-1@       | 192.168.1.254        |
    | @IP4Addr-2@         | 10.0.20.30/24        |
    | @MACAddr-2@         | Ethernet 2           |
    | @Prefix-2-1@        | 24                   |
    | @NextHop-2-1@       | 10.0.20.1            |

Lors de l’utilisation de chaînes de substitution, il est important de s’assurer que les chaînes seront remplies pendant le processus d’approvisionnement de la machine virtuelle. Si une chaîne telle que @ProductKey@ n’est pas fournie au moment du déploiement, le fait de laisser le nœud &lt;ProductKey&gt; dans le fichier d’installation sans assistance n’est pas renseigné, le processus de spécialisation échoue et vous ne pouvez pas vous connecter à votre machine virtuelle.

Notez également que les chaînes de substitution associées à la mise en réseau vers la fin de la table sont utilisées uniquement si vous tirez parti des pools d’adresses IP statiques VMM. Votre fournisseur de services d’hébergement doit être en mesure de vous indiquer si ces chaînes de substitution sont requises. Pour plus d’informations sur les adresses IP statiques dans les modèles VMM, consultez les rubriques suivantes dans la documentation de VMM :

- [Instructions pour les pools d’adresses IP](https://technet.microsoft.com/system-center-docs/vmm/plan/plan-network#guidelines-for-ip-address-pools)
- [Configurer des pools d’adresses IP statiques dans l’infrastructure VMM](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-network-static-address-pools)

Enfin, il est important de noter que le processus de déploiement de la machine virtuelle protégée chiffrera uniquement le lecteur du système d’exploitation. Si vous déployez une machine virtuelle protégée avec un ou plusieurs lecteurs de données, il est fortement recommandé d’ajouter une commande sans assistance ou stratégie de groupe paramètre dans le domaine du locataire pour chiffrer automatiquement les lecteurs de données.

## <a name="get-the-volume-signature-catalog-file"></a>Récupération du fichier catalogue des signatures de volume

Les fichiers de données de protection contiennent également des informations sur les disques de modèle qu’un client approuve. Les locataires acquièrent les signatures de disque à partir de disques de modèles approuvés sous la forme d’un fichier de catalogue de signatures de volume (VSC). Ces signatures sont ensuite validées lors du déploiement d’une nouvelle machine virtuelle. Si aucune des signatures du fichier de données de protection ne correspond au disque de modèle qui tente d’être déployé avec la machine virtuelle (c’est-à-dire qu’elle a été modifiée ou remplacée par un autre disque potentiellement malveillant), le processus d’approvisionnement échouera.

> [!IMPORTANT]
> Alors que le VSC garantit qu’aucun disque n’a été falsifié, il est toujours important que le locataire approuve le disque en premier lieu. Si vous êtes le client et que le disque de modèle est fourni par votre hébergeur, déployez une machine virtuelle de test à l’aide de ce disque de modèle et exécutez vos propres outils (antivirus, scanneurs de vulnérabilité, etc.) pour valider que le disque est, en fait, dans un état de confiance.

Il existe deux façons d’obtenir le VSC d’un disque de modèle :

1. L’hébergeur (ou le locataire, si le locataire a accès à VMM) utilise les applets de commande PowerShell de VMM pour enregistrer le VSC et le transmet au locataire. Cela peut être effectué sur n’importe quel ordinateur sur lequel la console VMM est installée et configurée pour gérer l’environnement VMM de l’infrastructure d’hébergement. Les applets de commande PowerShell pour enregistrer le VSC sont les suivantes :

    ```powershell
    $disk = Get-SCVirtualHardDisk -Name "templateDisk.vhdx"

    $vsc = Get-SCVolumeSignatureCatalog -VirtualHardDisk $disk

    $vsc.WriteToFile(".\templateDisk.vsc")
    ```

2. Le locataire a accès au fichier de modèle de disque. Cela peut être le cas si le locataire crée un disque de modèle à charger vers un fournisseur de services d’hébergement ou si le locataire peut télécharger le disque de modèle de l’hébergeur. Dans ce cas, sans VMM dans l’image, le locataire exécuterait l’applet de commande suivante (installée avec la fonctionnalité outils de machine virtuelle protégée, qui fait partie de Outils d’administration de serveur distant) :

    ```powershell
    Save-VolumeSignatureCatalog -TemplateDiskPath templateDisk.vhdx -VolumeSignatureCatalogPath templateDisk.vsc
    ```

## <a name="select-trusted-fabrics"></a>Sélectionner des infrastructures de confiance

Le dernier composant du fichier de données de protection est associé au propriétaire et aux gardiens d’une machine virtuelle. Les gardiens sont utilisés pour désigner le propriétaire d’une machine virtuelle protégée et les structures protégées sur lesquelles elle est autorisée à s’exécuter.

Pour autoriser une infrastructure d’hébergement à exécuter une machine virtuelle protégée, vous devez obtenir les métadonnées du gardien auprès du service Guardian hôte du fournisseur de services d’hébergement. Souvent, le fournisseur de services d’hébergement vous fournit ces métadonnées via leurs outils de gestion. Dans un scénario d’entreprise, vous pouvez avoir un accès direct pour obtenir les métadonnées vous-même.

Vous ou votre fournisseur de services d’hébergement pouvez obtenir les métadonnées Guardian à partir de SGH en effectuant l’une des actions suivantes :

- Obtenez les métadonnées Guardian directement à partir de SGH en exécutant la commande Windows PowerShell suivante, ou en accédant au site Web et en enregistrant le fichier XML qui s’affiche :

    ```powershell
    Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml
    ```

- Obtenez les métadonnées Guardian à partir de VMM à l’aide des applets de commande PowerShell de VMM :

    ```powershell
    $relecloudmetadata = Get-SCGuardianConfiguration
    $relecloudmetadata.InnerXml | Out-File .\RelecloudGuardian.xml -Encoding UTF8
    ```

Procurez-vous les fichiers de métadonnées Guardian pour chaque infrastructure protégée sur laquelle vous souhaitez autoriser l’exécution de vos machines virtuelles protégées avant de continuer.

## <a name="create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard"></a>Créer un fichier de données de protection et ajouter des gardiens à l’aide de l’Assistant fichier de données de protection

Exécutez l’Assistant fichier de données de protection pour créer un fichier de données de protection (PDK). Ici, vous allez ajouter le certificat RDP, le fichier d’installation sans assistance, les catalogues de signatures de volume, le gardien propriétaire et les métadonnées du gardien téléchargées obtenues à l’étape précédente.

1. Installez **Outils d’administration de serveur distant &gt; outils d’administration de fonctionnalités &gt; outils de machine virtuelle protégée** sur votre ordinateur à l’aide de gestionnaire de serveur ou de la commande Windows PowerShell suivante :

    ```powershell
    Install-WindowsFeature RSAT-Shielded-VM-Tools
    ```

2. Ouvrez l’Assistant fichier de données de protection à partir de la section outils d’administration de votre menu Démarrer ou en exécutant l’exécutable suivant **C :\\Windows\\System32\\ShieldingDataFileWizard. exe**.

3. Sur la première page, utilisez la deuxième zone de sélection de fichier pour choisir un emplacement et un nom de fichier pour votre fichier de données de protection. Normalement, vous nommez un fichier de données de protection après l’entité qui possède les machines virtuelles créées avec ces données de protection (par exemple, HR, IT, finance) et le rôle de charge de travail qu’il exécute (par exemple, serveur de fichiers, serveur Web ou tout autre fichier configuré par le fichier d’installation sans assistance). Laissez la case d’option définie sur **protection des données pour les modèles protégés**.

    > [!NOTE]
    > Dans l’Assistant fichier de données de protection, vous remarquerez les deux options ci-dessous :
    >- **Données de protection pour les modèles protégés**
    >- **Protection des données pour les machines virtuelles existantes et les modèles non protégés**<br>
    > La première option est utilisée lors de la création de machines virtuelles protégées à partir de modèles protégés. La deuxième option vous permet de créer des données de protection qui peuvent uniquement être utilisées lors de la conversion de machines virtuelles existantes ou de la création de machines virtuelles protégées à partir de modèles non protégés.

    ![Assistant fichier de données de protection, sélection de fichier](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-01.png)

    En outre, vous devez choisir si les machines virtuelles créées à l’aide de ce fichier de données de protection seront véritablement protégées ou configurées en mode « chiffrement pris en charge ». Pour plus d’informations sur ces deux options, consultez [Quels sont les types de machines virtuelles qu’une infrastructure protégée peut exécuter ?](guarded-fabric-and-shielded-vms.md#what-are-the-types-of-virtual-machines-that-a-guarded-fabric-can-run).

    > [!IMPORTANT]
    > Portez une attention particulière à l’étape suivante, car elle définit le propriétaire de vos machines virtuelles protégées et les structures sur lesquelles vos machines virtuelles protégées seront autorisées à s’exécuter.<br>La possession d’un **gardien propriétaire** est nécessaire pour modifier ultérieurement une machine virtuelle dotée d' **une** protection contre le **chiffrement pris en charge** , ou vice versa.

4. L’objectif de cette étape est deux fois plus :

    - Créer ou sélectionner un gardien propriétaire qui vous représente en tant que propriétaire de la machine virtuelle

    - Importez le gardien que vous avez téléchargé à partir du service Guardian hôte (ou de votre propre) fournisseur d’hébergement à l’étape précédente.

    Pour désigner un gardien propriétaire existant, sélectionnez le gardien approprié dans le menu déroulant. Seuls les gardiens installés sur votre ordinateur local avec les clés privées intactes apparaissent dans cette liste. Vous pouvez également créer votre propre gardien propriétaire en sélectionnant **gérer les gardiens locaux** dans l’angle inférieur droit, puis en cliquant sur **créer** et en terminant l’Assistant.

    Nous allons ensuite importer les métadonnées Guardian téléchargées précédemment à l’aide de la page **propriétaire et gardiens** . Sélectionnez **gérer les gardiens locaux** dans l’angle inférieur droit. Utilisez la fonctionnalité **Importer** pour importer le fichier de métadonnées Guardian. Cliquez sur **OK** une fois que vous avez importé ou ajouté tous les gardiens nécessaires. Il est recommandé de nommer les gardiens après le fournisseur de services d’hébergement ou le centre de l’entreprise qu’ils représentent. Enfin, sélectionnez tous les gardiens qui représentent les centres de centres dans lesquels votre machine virtuelle protégée est autorisée à s’exécuter. Vous n’avez pas besoin de sélectionner à nouveau le gardien propriétaire. Une fois l’opération terminée, cliquez sur **suivant** .

    ![Assistant fichier de données de protection, propriétaire et gardiens](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-02.png)

5. Dans la page qualificateurs de l’ID de volume, cliquez sur **Ajouter** pour autoriser un disque de modèle signé dans votre fichier de données de protection. Lorsque vous sélectionnez un VSC dans la boîte de dialogue, il affiche des informations sur le nom, la version et le certificat de ce disque qui a été utilisé pour le signer. Répétez ce processus pour chaque disque de modèle que vous souhaitez autoriser.

6. Sur la page **valeurs de spécialisation** , cliquez sur **Parcourir** pour sélectionner votre fichier Unattend. XML qui sera utilisé pour spécialiser vos machines virtuelles.

    Utilisez le bouton **Ajouter** en bas pour ajouter tout fichier supplémentaire au PDK qui est nécessaire pendant le processus de spécialisation. Par exemple, si votre fichier d’installation sans assistance installe un certificat RDP sur la machine virtuelle (comme décrit dans [générer un fichier de réponses à l’aide de la fonction New-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md)), vous devez ajouter le fichier PFX du certificat RDP et le script RDPCertificateConfig. ps1 ici. Notez que tous les fichiers que vous spécifiez ici seront automatiquement copiés dans C :\\Temp\\ sur la machine virtuelle créée. Votre fichier d’installation sans assistance doit s’attendre à ce que les fichiers se trouvent dans ce dossier lors de leur référencement par chemin d’accès.

7. Passez en revue vos sélections sur la page suivante, puis cliquez sur **générer**.

8. Fermez l’Assistant une fois qu’il est terminé.

## <a name="create-a-shielding-data-file-and-add-guardians-using-powershell"></a>Créer un fichier de données de protection et ajouter des gardiens à l’aide de PowerShell

Comme alternative à l’Assistant fichier de données de protection, vous pouvez exécuter [New-ShieldingDataFile](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/new-shieldingdatafile?view=win10-ps) pour créer un fichier de données de protection.

Tous les fichiers de données de protection doivent être configurés avec le propriétaire et les certificats de gardien appropriés pour autoriser l’exécution de vos machines virtuelles protégées sur une infrastructure protégée.
Vous pouvez vérifier si des gardiens sont installés localement en exécutant la [HgsGuardian](https://docs.microsoft.com/powershell/module/hgsclient/get-hgsguardian?view=win10-ps). Les gardiens de propriétaire ont des clés privées, contrairement aux gardiens de votre centre de centres.

Si vous avez besoin de créer un gardien propriétaire, exécutez la commande suivante :

```powershell
New-HgsGuardian -Name "Owner" -GenerateCertificates
```

Cette commande crée une paire de certificats de signature et de chiffrement dans le magasin de certificats de l’ordinateur local sous le dossier « certificats locaux d’ordinateur virtuel protégé ».
Vous aurez besoin des certificats de propriétaire et de leurs clés privées correspondantes pour déprotéger une machine virtuelle. Assurez-vous donc que ces certificats sont sauvegardés et protégés contre le vol.
Une personne malveillante ayant accès aux certificats de propriétaire peut les utiliser pour démarrer votre machine virtuelle protégée ou modifier sa configuration de sécurité.

Si vous devez importer des informations de gardien à partir d’une infrastructure protégée dans laquelle vous souhaitez exécuter votre machine virtuelle (Centre de données principal, centres de données de sauvegarde, etc.), exécutez la commande suivante pour chaque [fichier de métadonnées récupéré à partir de vos infrastructures protégées](#select-trusted-fabrics).

```powershell
Import-HgsGuardian -Name 'EAST-US Datacenter' -Path '.\EastUSGuardian.xml'
```

> [!TIP]
> Si vous avez utilisé des certificats auto-signés ou si les certificats inscrits auprès de SGH sont arrivés à expiration, vous devrez peut-être utiliser les indicateurs `-AllowUntrustedRoot` et/ou `-AllowExpired` avec la commande Import-HgsGuardian pour contourner les vérifications de sécurité.

Vous devrez également [obtenir un catalogue de signatures de volume](#get-the-volume-signature-catalog-file) pour chaque disque de modèle que vous souhaitez utiliser avec ce fichier de données de protection et un [fichier de réponses de données de protection](#create-an-answer-file) pour permettre au système d’exploitation d’effectuer automatiquement ses tâches de spécialisation.
Enfin, déterminez si vous souhaitez que votre machine virtuelle soit entièrement protégée ou vTPM.
Utilisez `-Policy Shielded` pour une machine virtuelle entièrement protégée ou `-Policy EncryptionSupported` pour une machine virtuelle compatible vTPM qui autorise les connexions de la console de base et PowerShell direct.

Une fois que tout est prêt, exécutez la commande suivante pour créer votre fichier de données de protection :

```powershell
$viq = New-VolumeIDQualifier -VolumeSignatureCatalogFilePath 'C:\temp\marketing-ws2016.vsc' -VersionRule Equals
New-ShieldingDataFile -ShieldingDataFilePath "C:\temp\Marketing-LBI.pdk" -Policy EncryptionSupported -Owner 'Owner' -Guardian 'EAST-US Datacenter' -VolumeIDQualifier $viq -AnswerFile 'C:\temp\marketing-ws2016-answerfile.xml'
```

> [!TIP]
> Si vous utilisez un certificat RDP personnalisé, des clés SSH ou d’autres fichiers qui doivent être inclus dans votre fichier de données de protection, utilisez le paramètre `-OtherFile` pour les inclure. Vous pouvez fournir une liste séparée par des virgules de chemins d’accès aux fichiers, comme `-OtherFile "C:\source\myRDPCert.pfx", "C:\source\RDPCertificateConfig.ps1"`

Dans la commande ci-dessus, le gardien nommé « Owner » (obtenu à partir de la commande HgsGuardian) sera en mesure de modifier la configuration de sécurité de la machine virtuelle à l’avenir, tandis que « EAST-US datacenter » peut exécuter la machine virtuelle sans modifier ses paramètres.
Si vous avez plusieurs gardiens, séparez les noms des gardiens par des virgules comme `'EAST-US Datacenter', 'EMEA Datacenter'`.
Le qualificateur d’ID de volume spécifie si vous approuvez uniquement la version exacte (égale) des disques de modèle ou des futures versions (GreaterThanOrEquals).
Le nom du disque et le certificat de signature doivent correspondre exactement pour que la comparaison des versions soit prise en compte au moment du déploiement.
Vous pouvez faire confiance à plusieurs disques de modèle en fournissant une liste séparée par des virgules de qualificateurs d’ID de volume au paramètre `-VolumeIDQualifier`.
Enfin, si vous avez d’autres fichiers qui doivent accompagner le fichier de réponses avec la machine virtuelle, utilisez le paramètre `-OtherFile` et spécifiez une liste de chemins de fichiers séparés par des virgules.

Consultez la documentation sur les applets de commande [New-ShieldingDataFile](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/New-ShieldingDataFile?view=win10-ps) et [New-VolumeIDQualifier](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/New-VolumeIDQualifier?view=win10-ps) pour en savoir plus sur les autres méthodes de configuration de votre fichier de données de protection.

## <a name="see-also"></a>Voir aussi

- [Déployer des machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Structure protégée et machines virtuelles dotées d’une protection maximale](guarded-fabric-and-shielded-vms-top-node.md)
