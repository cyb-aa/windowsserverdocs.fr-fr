---
title: Créer un disque de modèle d’ordinateur virtuel protégé Windows
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 9c8b84e8-1f5a-47a1-83ca-b1dbd801cb0b
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 01/29/2019
ms.openlocfilehash: 70014c04bbb4425fe3c3fd0379f10cf00abe00ee
ms.sourcegitcommit: 4b4ff8d9e18b2ddcd1916ffa2cd58fffbed8e7ef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/28/2019
ms.locfileid: "72986443"
---
# <a name="create-a-windows-shielded-vm-template-disk"></a>Créer un disque de modèle d’ordinateur virtuel protégé Windows

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2019


Comme pour les machines virtuelles standard, vous pouvez créer un modèle d’ordinateur virtuel (par exemple, un [modèle d’ordinateur virtuel dans Virtual Machine Manager (VMM)](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-library-add-vm-templates)) pour permettre aux locataires et aux administrateurs de déployer facilement de nouvelles machines virtuelles sur l’infrastructure à l’aide d’un disque de modèle. Étant donné que les machines virtuelles protégées sont des ressources sensibles à la sécurité, il existe des étapes supplémentaires pour créer un modèle d’ordinateur virtuel qui prend en charge la protection. Cette rubrique décrit les étapes de création d’un disque de modèle protégé et d’un modèle d’ordinateur virtuel dans VMM.

Pour comprendre le fonctionnement de cette rubrique dans le processus global de déploiement de machines virtuelles protégées, consultez [étapes de configuration du fournisseur de services d’hébergement pour les hôtes service Guardian et les machines virtuelles](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)protégées.

## <a name="prepare-an-operating-system-vhdx"></a>Préparer le VHDX d’un système d’exploitation

Préparez tout d’abord un disque de système d’exploitation que vous exécuterez ensuite via l’Assistant de création de disque de modèle protégé. Ce disque sera utilisé comme disque de système d’exploitation dans les machines virtuelles de votre locataire. Vous pouvez utiliser n’importe quel outil existant pour créer ce disque, tel que Microsoft Desktop image Service Manager (DISM), ou configurer manuellement une machine virtuelle avec un VHDX vide et installer le système d’exploitation sur ce disque. Quand vous configurez le disque, celui-ci doit respecter les conditions suivantes qui sont spécifiques aux machines virtuelles de génération 2 et/ou protégées : 

| Exigence pour VHDX | Raison |
|-----------|----|
|Doit être un disque de table de partition GUID (GPT) | Nécessaire pour que les ordinateurs virtuels de génération 2 prennent en charge UEFI|
|Le type de disque doit être de **base** et non **dynamique**. <br>Remarque : cela fait référence au type de disque logique, et non à la fonctionnalité VHDX « extensible dynamiquement » prise en charge par Hyper-V. | BitLocker ne prend pas en charge les disques dynamiques.|
|Le disque comporte au moins deux partitions. Une partition doit inclure le lecteur sur lequel Windows est installé. Il s’agit du lecteur que BitLocker chiffrera. L’autre partition est la partition active, qui contient le chargeur de démarrage et reste non chiffrée afin que l’ordinateur puisse être démarré.|Requis pour BitLocker|
|Le système de fichiers est NTFS | Requis pour BitLocker|
|Le système d’exploitation installé sur le VHDX est l’un des éléments suivants :<br>-Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 <br>-Windows 10, Windows 8.1, Windows 8| Nécessaire pour prendre en charge les ordinateurs virtuels de 2e génération et le modèle de démarrage sécurisé Microsoft|
|Le système d’exploitation doit être généralisé (exécuter Sysprep. exe) | L’approvisionnement de modèles implique la spécialisation des machines virtuelles pour la charge de travail d’un locataire spécifique| 

> [!NOTE]
> Si vous utilisez VMM, ne copiez pas le disque de modèle dans la bibliothèque VMM à ce niveau. 

## <a name="run-windows-update-on-the-template-operating-system"></a>Exécuter Windows Update sur le modèle de système d’exploitation

Sur le disque de modèle, vérifiez que toutes les mises à jour Windows les plus récentes sont installées sur le système d’exploitation. Les mises à jour récemment publiées améliorent la fiabilité du processus de protection de bout en bout, un processus qui peut échouer si le système d’exploitation du modèle n’est pas à jour.

## <a name="prepare-and-protect-the-vhdx-with-the-template-disk-wizard"></a>Préparer et protéger le VHDX à l’aide de l’Assistant Création d’un disque de modèle

Pour utiliser un disque de modèle avec des machines virtuelles dotées d’une protection maximale, le disque doit être préparé et chiffré avec BitLocker à l’aide de l’Assistant Création de disque de modèle protégé. Cet Assistant génère un hachage pour le disque et l’ajoute à un catalogue de signatures de volume (VSC). Le VSC est signé à l’aide d’un certificat que vous spécifiez et il est utilisé pendant le processus de configuration pour garantir que le disque en cours de déploiement pour un locataire n’a pas été modifié ou remplacé par un disque non approuvé par le locataire. Enfin, BitLocker est installé sur le système d’exploitation du disque (s’il n’est pas déjà présent) pour préparer le disque au chiffrement lors de l’approvisionnement de la machine virtuelle.

> [!NOTE]
> L’Assistant disque de modèle va modifier le disque de modèle que vous spécifiez sur place. Vous pouvez effectuer une copie du VHDX non protégé avant d’exécuter l’Assistant pour effectuer ultérieurement des mises à jour sur le disque. Vous ne pourrez pas modifier un disque qui a été protégé à l’aide de l’Assistant Création d’un disque de modèle.

Effectuez les étapes suivantes sur un ordinateur exécutant Windows Server 2016, Windows 10 (avec les outils d’administration de serveur distant, RSAT installés) ou version ultérieure (il n’est pas nécessaire qu’il s’agit d’un hôte service Guardian ou d’un serveur VMM) :

1. Copiez le VHDX généralisé créé dans [préparer un système d’exploitation vhdx](#prepare-an-operating-system-vhdx) au serveur, s’il n’est pas déjà présent.

2. Pour administrer le serveur localement, installez la fonctionnalité **outils de machines virtuelles protégées** à partir de **Outils d’administration de serveur distant** sur le serveur.

        Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
        
    Vous pouvez également administrer le serveur à partir d’un ordinateur client sur lequel vous avez installé le [Outils d’administration de serveur distant Windows 10](https://www.microsoft.com/en-us/download/details.aspx?id=45520).

3. Obtenez ou créez un certificat pour signer le VSC pour le VHDX qui deviendra le disque de modèle pour les nouvelles machines virtuelles protégées. Les détails relatifs à ce certificat sont présentés aux locataires lorsqu’ils créent leurs fichiers de données de protection et qu’ils autorisent les disques auxquels ils font confiance. Par conséquent, il est important d’obtenir ce certificat auprès d’une autorité de certification que vous et vos locataires approuvent de façon mutuelle. Dans les scénarios d’entreprise où vous êtes à la fois l’hébergeur et le locataire, vous pouvez envisager d’émettre ce certificat à partir de votre infrastructure à clé publique.

    Si vous configurez un environnement de test et que vous souhaitez simplement utiliser un certificat auto-signé pour préparer votre disque de modèle, exécutez une commande semblable à la suivante :

        New-SelfSignedCertificate -DnsName publisher.fabrikam.com

4. Démarrez l' **Assistant disque de modèle** à partir du dossier **Outils d’administration** du menu Démarrer ou en tapant **TemplateDiskWizard. exe** dans une invite de commandes.

5. Sur la page **certificat** , cliquez sur **Parcourir** pour afficher la liste des certificats. Sélectionnez le certificat avec lequel préparer le modèle de disque. Cliquez sur **OK** , puis sur **Suivant**.

6. Sur la page disque virtuel, cliquez sur **Parcourir** pour sélectionner le VHDX que vous avez préparé, puis cliquez sur **suivant**.

7. Sur la page catalogue de signatures, indiquez un **nom de disque** et une **version conviviaux.** Ces champs sont présents pour vous aider à identifier le disque une fois qu’il a été préparé.

    Par exemple, pour le **nom de disque** , vous pouvez taper _WS2016_ et pour **version**, _1.0.0.0_

8. Passez en revue vos sélections dans la page vérifier les paramètres de l’Assistant. Lorsque vous cliquez sur **générer**, l’Assistant Active BitLocker sur le disque de modèle, calcule le hachage du disque et crée le catalogue de signatures de volume, qui est stocké dans les métadonnées VHDX.

    Attendez la fin du processus de préparation avant de tenter de monter ou de déplacer le disque de modèle. Ce processus peut prendre un certain temps, en fonction de la taille de votre disque.

    > [!IMPORTANT]
    > Les disques de modèle ne peuvent être utilisés qu’avec le processus de configuration de machine virtuelle protégé protégé.
    > Si vous tentez de démarrer une machine virtuelle standard (non protégée) à l’aide d’un disque de modèle, vous obtiendrez probablement une erreur d’arrêt (écran bleu) et n’est pas prise en charge.

9. Sur la page **Résumé** , vous pouvez afficher des informations sur le modèle de disque, le certificat utilisé pour signer le VSC et l’émetteur du certificat. Cliquez sur **Fermer** pour quitter l'Assistant.

Si vous utilisez VMM, suivez les étapes décrites dans les autres sections de cette rubrique pour incorporer un disque de modèle dans un modèle de machine virtuelle protégée dans VMM. 

## <a name="copy-the-template-disk-to-the-vmm-library"></a>Copier le disque de modèle dans la bibliothèque VMM

Si vous utilisez VMM, après avoir créé un disque de modèle, vous devez le copier dans un partage de bibliothèque VMM afin que les hôtes puissent télécharger et utiliser le disque lors de la configuration de nouvelles machines virtuelles. Utilisez la procédure suivante pour copier le disque de modèle dans la bibliothèque VMM, puis actualisez la bibliothèque.

1. Copiez le fichier VHDX dans le dossier de partage de bibliothèque VMM. Si vous avez utilisé la configuration VMM par défaut, copiez le disque de modèle sur _\\<vmmserver>\MSSCVMMLibrary\VHDs_.

2. Actualisez le serveur de bibliothèque. Ouvrez l’espace de travail **bibliothèque** , développez **serveurs de bibliothèque**, cliquez avec le bouton droit sur le serveur de bibliothèque que vous souhaitez actualiser, puis cliquez sur **Actualiser**.

3. Ensuite, fournissez à VMM des informations sur le système d’exploitation installé sur le disque de modèle :

    a. Recherchez votre disque de modèle nouvellement importé sur votre serveur de bibliothèque dans l’espace de travail **bibliothèque** .

    b. Cliquez avec le bouton droit sur le disque, puis cliquez sur **Propriétés**.

    c. Pour **système d’exploitation**, développez la liste et sélectionnez le système d’exploitation installé sur le disque. La sélection d’un système d’exploitation indique à VMM que le VHDX n’est pas vide.

    d. Lorsque vous avez mis à jour les propriétés, cliquez sur **OK**.

La petite icône de bouclier à côté du nom du disque désigne le disque en tant que disque de modèle préparé pour les machines virtuelles protégées. Vous pouvez également cliquer avec le bouton droit sur les en-têtes de colonne et activer/désactiver la colonne **protégée** pour afficher une représentation textuelle indiquant si un disque est destiné à des déploiements de machines virtuelles standard ou protégées.

![Disque de modèle d’ordinateur virtuel protégé](../media/Guarded-Fabric-Shielded-VM/shielded-vm-template-disk.png)

## <a name="create-the-shielded-vm-template-in-vmm-using-the-prepared-template-disk"></a>Créer le modèle de machine virtuelle protégée dans VMM à l’aide du disque de modèle préparé

Avec un disque de modèle préparé dans votre bibliothèque VMM, vous êtes prêt à créer un modèle de machine virtuelle pour les machines virtuelles protégées. Les modèles de machine virtuelle pour les machines virtuelles protégées diffèrent légèrement des modèles de machine virtuelle traditionnels dans la mesure où certains paramètres sont fixes (les machines virtuelles de génération 2, UEFI et le démarrage sécurisé sont activées, etc.) et d’autres ne sont pas disponibles (la personnalisation des locataires est limitée à quelques-unes, sélectionnez les propriétés de la machine virtuelle) . Pour créer le modèle d’ordinateur virtuel, procédez comme suit :

1. Dans l’espace de travail **bibliothèque** , cliquez sur **créer un modèle d’ordinateur virtuel** sous l’onglet dossier de démarrage en haut.

2. Dans la page **Sélectionner une source**, cliquez sur **Utiliser un modèle d'ordinateur virtuel existant ou un disque dur virtuel stocké dans la bibliothèque**, puis cliquez sur **Parcourir**.

3. Dans la fenêtre qui s’affiche, sélectionnez un disque de modèle préparé dans la bibliothèque VMM. Pour identifier plus facilement les disques qui sont préparés, cliquez avec le bouton droit sur un en-tête de colonne et activez la colonne **protégée** . Cliquez sur **OK** , puis sur **suivant**.

4. Spécifiez un nom de modèle d’ordinateur virtuel et éventuellement une description, puis cliquez sur **suivant**.

5. Dans la page **configurer le matériel** , spécifiez les fonctionnalités des machines virtuelles créées à partir de ce modèle. Assurez-vous qu’au moins une carte réseau est disponible et configurée sur le modèle d’ordinateur virtuel. Pour qu’un locataire se connecte à une machine virtuelle protégée, le seul moyen est de Connexion Bureau à distance, Windows Remote Management ou d’autres outils de gestion à distance préconfigurés qui fonctionnent sur les protocoles réseau.

    Si vous choisissez de tirer parti des pools d’adresses IP statiques dans VMM au lieu d’exécuter un serveur DHCP sur le réseau client, vous devrez alerter vos locataires pour cette configuration. Lorsqu’un locataire fournit son fichier de données de protection, qui contient le fichier d’installation sans assistance pour VMM, il doit fournir des valeurs d’espace réservé spéciales pour les informations du pool d’adresses IP statiques. Pour plus d’informations sur les espaces réservés VMM dans les fichiers d’installation sans assistance du locataire, consultez [créer un fichier de réponses](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file). 

6. Dans la page **configurer le système d’exploitation** , VMM n’affiche que quelques options pour les machines virtuelles protégées, y compris la clé de produit, le fuseau horaire et le nom de l’ordinateur. Certaines informations sécurisées, telles que le mot de passe de l’administrateur et le nom de domaine, sont spécifiées par le locataire via un fichier de données de protection (. Fichier PDK). 

    > [!NOTE]
    > Si vous choisissez de spécifier une clé de produit sur cette page, assurez-vous qu’elle est valide pour le système d’exploitation sur le disque de modèle. Si une clé de produit incorrecte est utilisée, la création de la machine virtuelle échoue.

Une fois le modèle créé, les locataires peuvent l’utiliser pour créer des machines virtuelles. Vous devez vérifier que le modèle d’ordinateur virtuel est l’une des ressources disponibles pour le rôle d’utilisateur Administrateur client (dans VMM, les rôles d’utilisateur se trouvent dans l’espace de travail **paramètres** ).

## <a name="prepare-and-protect-the-vhdx-using-powershell"></a>Préparer et protéger le VHDX à l’aide de PowerShell

Comme alternative à l’exécution de l’Assistant disque de modèle, vous pouvez copier votre disque de modèle et votre certificat sur un ordinateur exécutant RSAT et exécuter [Protect-TemplateDisk](https://docs.microsoft.com/powershell/module/shieldedvmtemplate/protect-templatedisk?view=win10-ps
) pour initier le processus de signature.
L’exemple suivant utilise les informations de nom et de version spécifiées par les paramètres _TemplateName_ et _version_ .
Le VHDX que vous fournissez au paramètre `-Path` sera remplacé par le disque de modèle mis à jour. Assurez-vous de faire une copie avant d’exécuter la commande.

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Certificate $certificate -Path "WindowsServer2019-ShieldedTemplate.vhdx" -TemplateName "Windows Server 2019" -Version 1.0.0.0
```

Votre disque de modèle est maintenant prêt à être utilisé pour approvisionner des machines virtuelles protégées.
Si vous utilisez System Center Virtual Machine Manager pour déployer votre machine virtuelle, vous pouvez maintenant copier le VHDX dans votre bibliothèque VMM.

Vous souhaiterez peut-être également extraire le catalogue de signatures de volume du VHDX.
Ce fichier est utilisé pour fournir des informations sur le certificat de signature, le nom du disque et la version aux propriétaires de machines virtuelles qui veulent utiliser votre modèle.
Ils doivent importer ce fichier dans l’Assistant fichier de données de protection pour vous autoriser, l’auteur du modèle en possession du certificat de signature, pour créer ce modèle et les disques de modèle à venir pour eux.

Pour extraire le catalogue de signatures de volume, exécutez la commande suivante dans PowerShell :

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```


## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Créer un fichier de données de protection](guarded-fabric-tenant-creates-shielding-data.md)

## <a name="see-also"></a>Articles associés

- [Étapes de configuration du fournisseur de services d’hébergement pour les hôtes service Guardian et les machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Structure protégée et machines virtuelles dotées d’une protection maximale](guarded-fabric-and-shielded-vms-top-node.md)
