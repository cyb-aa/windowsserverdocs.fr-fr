---
title: Créer un disque de modèle de machine virtuelle protégé Windows
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 9c8b84e8-1f5a-47a1-83ca-b1dbd801cb0b
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 01/29/2019
ms.openlocfilehash: d9f07d2e6e93d4f8d198c2fc3b62c28c940bdefb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447511"
---
# <a name="create-a-windows-shielded-vm-template-disk"></a>Créer un disque de modèle de machine virtuelle protégé Windows

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Comme avec ceux des machines virtuelles, vous pouvez créer un modèle de machine virtuelle (par exemple, un [modèle d’ordinateur virtuel dans Virtual Machine Manager (VMM)](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-library-add-vm-templates)) pour faciliter le travail pour les clients et aux administrateurs de déployer de nouvelles machines virtuelles sur l’infrastructure à l’aide d’un disque de modèle. Étant donné que les machines virtuelles protégées sont des ressources de sécurité sensibles, il existe des étapes supplémentaires pour créer un modèle de machine virtuelle qui prend en charge la protection. Cette rubrique décrit les étapes pour créer un disque de modèle protégé et un modèle d’ordinateur virtuel dans VMM.

Pour comprendre comment cette rubrique s’intègre dans le processus de déploiement de machines virtuelles protégées, consultez [hébergeant des étapes de configuration de fournisseur de service pour les hôtes service Guardian et des machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="prepare-an-operating-system-vhdx"></a>Préparer un système d’exploitation VHDX

Tout d’abord préparer un disque de système d’exploitation que vous exécuterez ensuite via l’Assistant Création de disque protégé du modèle. Ce disque sera utilisé en tant que le disque du système d’exploitation sur des machines virtuelles de votre client. Vous pouvez utiliser les outils existants pour créer ce disque, telles que Microsoft Desktop Image Service Manager (DISM), ou configurer une machine virtuelle avec un VHDX vide et installer manuellement le système d’exploitation sur ce disque. Lorsque vous configurez le disque, il doit respecter les conditions suivantes qui sont spécifiques à la génération 2 et/ou machines virtuelles protégées : 

| Configuration requise pour le VHDX | Reason |
|-----------|----|
|Doit être un disque de Table de Partition GUID (GPT) | Nécessaire pour les machines virtuelles de génération 2 prendre en charge d’UEFI|
|Type de disque doit être **base** par opposition à **dynamique**. <br>Remarque: Cela vaut pour le type de disque logique, pas dans la « taille dynamique » fonctionnalité VHDX pris en charge par Hyper-V. | BitLocker ne prend pas en charge les disques dynamiques.|
|Le disque a au moins deux partitions. Une partition doit inclure le lecteur sur lequel Windows est installé. Il s’agit du lecteur BitLocker chiffre. L’autre partition est la partition active, qui contient le chargeur de démarrage et reste non chiffrée afin que l’ordinateur peut être démarré.|Nécessaire pour BitLocker|
|Système de fichiers est NTFS | Nécessaire pour BitLocker|
|Le système d’exploitation installé sur le disque VHDX est une des opérations suivantes :<br>-Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 <br>-Windows 10, Windows 8.1, Windows 8| Pour prendre en charge des ordinateurs virtuels de génération 2 et le modèle de démarrage sécurisé de Microsoft|
|Système d’exploitation doit être généralisé (exécution de sysprep.exe) | L’approvisionnement de modèle implique spécialisé des machines virtuelles pour la charge de travail d’un client spécifique| 

> [!NOTE]
> Si vous utilisez VMM, ne copiez pas le disque de modèle dans la bibliothèque VMM à ce stade. 

## <a name="run-windows-update-on-the-template-operating-system"></a>Exécuter la mise à jour de Windows sur le système d’exploitation du modèle

Sur le disque de modèle, vérifiez que le système d’exploitation possède toutes les dernières mises à jour de Windows installées. Récemment mises à jour publiées améliorent la fiabilité du processus de protection de bout en bout : un processus qui risque d’échouer if complète le système d’exploitation du modèle n’est pas à jour.

## <a name="prepare-and-protect-the-vhdx-with-the-template-disk-wizard"></a>Préparer et de protéger le VHDX avec l’Assistant modèle de disque

Pour utiliser un disque de modèle avec des machines virtuelles protégées, le disque doit être préparé et chiffré avec BitLocker à l’aide de l’Assistant Création de disque protégé du modèle. Cet Assistant générer un hachage pour le disque et ajoutez-le à un catalogue de signatures de volume (VSC). La VSC est signé à l’aide d’un certificat que vous spécifiez et êtes utilisé pendant le processus d’approvisionnement pour garantir le disque en cours de déploiement pour un client n’a pas été modifié ou remplacé par un disque, que le client n’approuve pas. Enfin, BitLocker est installé sur système d’exploitation du disque (si ce n’est déjà fait) pour préparer le disque pour le chiffrement lors de la configuration de la machine virtuelle.

> [!NOTE]
> L’Assistant modèle de disque modifiera le disque de modèle que vous spécifiez sur place. Voulez-vous effectuer une copie de la non protégé VHDX avant d’exécuter l’Assistant pour mettre à jour le disque à une date ultérieure. Vous ne serez pas en mesure de modifier un disque qui a été protégé par l’Assistant modèle de disque.

Sur un ordinateur exécutant Windows Server 2016 (n’a pas besoin d’être un hôte service Guardian ou un serveur VMM), procédez comme suit :

1. Copier le VHDX généralisé créé dans [préparer un système d’exploitation VHDX](#prepare-an-operating-system-vhdx) sur le serveur, si ce n’est pas déjà fait.

2. Pour administrer le serveur local, installez le **outils de machine virtuelle protégée** la fonctionnalité **outils d’Administration de serveur distant** sur le serveur.

        Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
        
    Vous pouvez également administrer le serveur à partir d’un ordinateur client sur lequel vous avez installé le [outils d’Administration de serveur distant Windows 10](https://www.microsoft.com/en-us/download/details.aspx?id=45520).

3. Obtenir ou créer un certificat pour signer la VSC pour le VHDX qui deviendra le disque de modèle pour les nouvelles machines virtuelles protégées. Plus d’informations sur ce certificat seront affichera pour les clients lorsqu’ils créent leurs fichiers de données de protection et que vous autorisant disques qu'ils font confiance. Par conséquent, il est important obtenir ce certificat à partir d’une autorité de certification mutuellement approuvée par vous et vos clients. Dans les scénarios d’entreprise où vous êtes l’hébergeur et le client, vous pouvez envisager d’émettre ce certificat à partir de votre infrastructure à clé publique.

    Si vous installez un environnement de test et que vous souhaitez simplement utiliser un certificat auto-signé pour préparer votre disque de modèle, exécutez une commande semblable à ce qui suit :

        New-SelfSignedCertificate -DnsName publisher.fabrikam.com

4. Démarrer le **Assistant disque de modèle** à partir de la **outils d’administration** dossier dans le menu Démarrer ou en tapant **TemplateDiskWizard.exe** dans une invite de commandes.

5. Sur le **certificat** , cliquez sur **Parcourir** pour afficher une liste de certificats. Sélectionnez le certificat avec lequel préparer le modèle de disque. Cliquez sur **OK** , puis sur **Suivant**.

6. Dans la page de disque virtuel, cliquez sur **Parcourir** pour sélectionner le VHDX que vous avez préparé, puis cliquez sur **suivant**.

7. Dans la page catalogue de signatures, fournir un convivial **nom du disque** et **version.** Ces champs sont présents pour vous aider à identifier le disque une fois qu’il a été préparé.

    Par exemple, pour **nom du disque** vous pouvez taper _WS2016_ et pour **Version**, _1.0.0.0_

8. Passez en revue vos sélections dans la page vérifier les paramètres de l’Assistant. Lorsque vous cliquez sur **générer**, l’Assistant Activation de BitLocker sur le disque de modèle, calculer le hachage du disque et créer le catalogue de signatures de Volume, qui est stocké dans les métadonnées VHDX.

    Attendez que le processus de préparation terminée avant d’essayer de monter ou de déplacer le disque de modèle. Ce processus peut prendre un certain temps, selon la taille de votre disque.

    > [!IMPORTANT]
    > Modèle disques peuvent uniquement être utilisés avec la machine virtuelle sécurisée protégée processus d’approvisionnement.
    > Tentative de démarrage d’une machine virtuelle standard (non blindée) à l’aide d’un disque de modèle est susceptible de provoquer une erreur d’arrêt (écran bleu) et non pris en charge.

9. Sur le **Résumé** page, les informations sur le modèle de disque, le certificat utilisé pour signer la VSC, et de l’émetteur du certificat est affiché. Cliquez sur **Fermer** pour quitter l'Assistant.

Si vous utilisez VMM, suivez les étapes décrites dans les sections restantes dans cette rubrique pour incorporer un disque de modèle dans un modèle de machine virtuelle protégée dans VMM. 

## <a name="copy-the-template-disk-to-the-vmm-library"></a>Copiez le disque de modèle dans la bibliothèque VMM

Si vous utilisez VMM, après avoir créé un disque de modèle, vous devez copier vers un partage de bibliothèque VMM afin d’hôtes peuvent télécharger et utiliser le disque lors de l’approvisionnement de nouvelles machines virtuelles. Utilisez la procédure suivante pour copier le disque de modèle dans la bibliothèque VMM, puis actualisez la bibliothèque.

1. Copiez le fichier VHDX dans le dossier de partage de bibliothèque VMM. Si vous avez utilisé la configuration de VMM par défaut, copiez le disque de modèle à  _\\ <vmmserver>\MSSCVMMLibrary\VHDs_.

2. Actualiser le serveur de bibliothèque. Ouvrez le **bibliothèque** espace de travail, développez **serveurs de bibliothèque**, avec le bouton droit sur le serveur de bibliothèque que vous souhaitez actualiser, puis cliquez sur **Actualiser**.

3. Ensuite, fournir à VMM des informations sur le système d’exploitation installé sur le disque de modèle :

    a. Rechercher votre disque de modèle qui vient d’être importé sur votre serveur de bibliothèque dans le **bibliothèque** espace de travail.

    b. Cliquez sur le disque, puis sur **propriétés**.

    c. Pour **système d’exploitation**, développez la liste et sélectionnez le système d’exploitation installé sur le disque. Sélection d’un système d’exploitation indique à VMM, que le VHDX n’est pas vide.

    d. Lorsque vous avez mis à jour les propriétés, cliquez sur **OK**.

L’icône de bouclier petite en regard du nom du disque désigne le disque comme disque de modèle préparé pour les machines virtuelles protégées. Vous pouvez également avec le bouton droit cliquez sur les en-têtes de colonne et les activer/désactiver le **blindé** colonne pour afficher une représentation textuelle, qui indique si un disque est conçu pour les déploiements de machines virtuelles protégées ou non conforme.

![Disque de modèle de machine virtuelle protégée](../media/Guarded-Fabric-Shielded-VM/shielded-vm-template-disk.png)

## <a name="create-the-shielded-vm-template-in-vmm-using-the-prepared-template-disk"></a>Créer le modèle de machine virtuelle protégée dans VMM à l’aide du disque de modèle préparé

Avec un disque de modèle préparé dans votre bibliothèque VMM, vous êtes prêt à créer un modèle de machine virtuelle pour des machines virtuelles protégées. Modèles de machine virtuelle pour les machines virtuelles protégées diffèrent légèrement des modèles de machine virtuelle classiques que certains paramètres sont fixes (génération 2 de machine virtuelle, UEFI et le démarrage sécurisé est activé et ainsi de suite) et d’autres ne sont pas disponibles (personnalisation de locataire est limitée à quelques, sélectionnez les propriétés de la machine virtuelle) . Pour créer le modèle de machine virtuelle, procédez comme suit :

1. Dans le **bibliothèque** espace de travail, cliquez sur **créer un modèle de machine virtuelle** sous l’onglet Accueil en haut.

2. Dans la page **Sélectionner une source**, cliquez sur **Utiliser un modèle d'ordinateur virtuel existant ou un disque dur virtuel stocké dans la bibliothèque**, puis cliquez sur **Parcourir**.

3. Dans la fenêtre qui s’affiche, sélectionnez un disque de modèle préparé à partir de la bibliothèque VMM. Pour identifier plus facilement quels disques sont prêts, cliquez sur un en-tête de colonne et activer le **blindé** colonne. Cliquez sur **OK** puis **suivant**.

4. Spécifiez un nom de modèle de machine virtuelle et éventuellement une description, puis cliquez sur **suivant**.

5. Sur le **configurer le matériel** page, spécifier les capacités des machines virtuelles créées à partir de ce modèle. Assurez-vous qu’au moins une carte réseau est disponible et configuré sur le modèle de machine virtuelle. La seule façon pour un client pour se connecter à une machine virtuelle protégée est via la connexion Bureau à distance, de gestion à distance de Windows ou d’autres outils de gestion à distance préconfigurée qui fonctionnent sur les protocoles réseau.

    Si vous choisissez d’utiliser des pools IP statiques dans VMM au lieu d’exécuter un serveur DHCP sur le réseau de locataire, vous devrez avertir vos clients à cette configuration. Lorsqu’un client fournit leur un fichier de données qui contient le fichier d’installation sans assistance pour le VMM, ils devrez fournir des valeurs de l’espace réservé spécial pour les informations de pool IP statique. Pour plus d’informations sur les espaces réservés VMM dans les fichiers d’installation sans assistance de client, consultez [créer un fichier de réponses](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file). 

6. Sur le **configurer le système d’exploitation** page, VMM affiche uniquement quelques options pour les machines virtuelles protégées, y compris la clé de produit, le fuseau horaire et le nom de l’ordinateur. Certaines informations sécurisées, telles que le nom de domaine et le mot de passe d’administrateur, sont spécifiées par le client via un fichier de données de protection (. Fichier PDK). 

    > [!NOTE]
    > Si vous choisissez de spécifier une clé de produit sur cette page, vérifiez qu’il est valide pour le système d’exploitation sur le disque de modèle. Si une clé de produit incorrecte est utilisée, la création de machine virtuelle échoue.

Une fois que le modèle est créé, les locataires peuvent l’utiliser pour créer des machines virtuelles. Vous devez vérifier que le modèle de machine virtuelle est une des ressources disponibles pour le rôle d’utilisateur administrateur client (dans VMM, les rôles d’utilisateur sont dans le **paramètres** espace de travail).

## <a name="prepare-and-protect-the-vhdx-using-powershell"></a>Préparer et de protéger le VHDX à l’aide de PowerShell

Comme alternative à l’exécution de l’Assistant modèle de disque, vous pouvez copier votre disque de modèle et le certificat à un ordinateur exécutant le serveur distant et exécuter [Protect-TemplateDisk](https://docs.microsoft.com/powershell/module/shieldedvmtemplate/protect-templatedisk?view=win10-ps
) pour lancer le processus de signature.
L’exemple suivant utilise les informations de nom et la version spécifiées par la _TemplateName_ et _Version_ paramètres.
Le VHDX que vous fournissez à la `-Path` paramètre sera remplacé par le disque de modèle mis à jour, par conséquent, veillez à faire une copie avant d’exécuter la commande.

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Certificate $certificate -Path "WindowsServer2019-ShieldedTemplate.vhdx" -TemplateName "Windows Server 2019" -Version 1.0.0.0
```

Votre disque de modèle est maintenant prêt à être utilisé pour des machines virtuelles protégées.
Si vous utilisez System Center Virtual Machine Manager pour déployer votre machine virtuelle, vous pouvez maintenant copier le disque VHDX à votre bibliothèque VMM.

Vous souhaiterez également extraire le catalogue de signatures de volume du disque VHDX.
Ce fichier est utilisé pour fournir des informations sur la version, le nom du disque et le certificat de signature pour les propriétaires d’ordinateurs virtuels qui souhaitent utiliser votre modèle.
Dont ils ont besoin importer ce fichier dans l’Assistant de fichier de données de protection pour vous autoriser, l’auteur du modèle en possession du certificat de signature, pour créer ce et disques de modèle futures pour eux.

Pour extraire le catalogue de signatures de volume, exécutez la commande suivante dans PowerShell :

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```


## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Créer un fichier de données de protection](guarded-fabric-tenant-creates-shielding-data.md)

## <a name="see-also"></a>Voir aussi

- [Hébergement des étapes de configuration de fournisseur de service pour les hôtes service Guardian et des machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Structure protégée et machines virtuelles dotées d’une protection maximale](guarded-fabric-and-shielded-vms-top-node.md)
