---
title: Machines virtuelles protégées pour les locataires - création d’un disque de modèle - facultatif
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: c1992f8b-6f88-4dbc-b4a5-08368bba2787
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 2709c84a16dadc2211af4a6f5b43c13266ede155
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874630"
---
# <a name="shielded-vms-for-tenants---creating-a-template-disk-optional"></a>Machines virtuelles protégées pour les locataires - création d’un disque de modèle (facultatif)

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Pour créer une machine virtuelle protégée, vous devez utiliser un disque de modèle signé, spécialement préparée. Les métadonnées à partir de disques de modèle signés permet de s’assurer que les disques ne sont pas modifiées une fois qu’ils ont été créés et vous permet de comme un client pour limiter les disques peut être utilisé pour créer vos machines virtuelles protégées. Un moyen de fournir ce disque consiste à vous, le client, de créer, comme décrit dans cette rubrique. 

> [!IMPORTANT]
> Si vous préférez, vous pouvez utiliser à la place un disque de modèle fourni par votre fournisseur de services. Si vous procédez ainsi, il est important de déployer un test de machine virtuelle à l’aide de ce disque de modèle et d’exécuter vos propres outils (antivirus, des analyseurs de vulnérabilité et ainsi de suite) pour valider le disque est en fait, dans un état qui vous faites confiance.

## <a name="prepare-an-operating-system-vhdx"></a>Préparer un système d’exploitation VHDX

Pour créer un disque de modèle protégée, vous devez tout d’abord préparer un disque de système d’exploitation qui est exécuté via l’Assistant modèle de disque. Ce disque sera utilisé en tant que le disque du système d’exploitation dans des machines virtuelles protégées. Vous pouvez utiliser les outils existants pour créer ce disque, telles que Microsoft Desktop Image Service Manager (DISM), ou configurer une machine virtuelle avec un VHDX vide et installer manuellement le système d’exploitation sur ce disque. Lorsque vous configurez le disque, il doit respecter les conditions suivantes qui sont spécifiques à la génération 2 et/ou machines virtuelles protégées : 

| Configuration requise pour le VHDX | Raison |
|-----------|----|
|Doit être un disque de Table de Partition GUID (GPT) | Nécessaire pour les machines virtuelles de génération 2 prendre en charge d’UEFI|
|Type de disque doit être **base** par opposition à **dynamique**. <br>Remarque: Cela vaut pour le type de disque logique, pas dans la « taille dynamique » fonctionnalité VHDX pris en charge par Hyper-V. | BitLocker ne prend pas en charge les disques dynamiques.|
|Le disque a au moins deux partitions. Une partition doit inclure le lecteur sur lequel Windows est installé. Il s’agit du lecteur BitLocker chiffre. L’autre partition est la partition active, qui contient le chargeur de démarrage et reste non chiffrée afin que l’ordinateur peut être démarré.|Nécessaire pour BitLocker|
|Système de fichiers est NTFS | Nécessaire pour BitLocker|
|Le système d’exploitation installé sur le disque VHDX est une des opérations suivantes :<br>-Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 <br>-Windows 10, Windows 8.1, Windows 8| Pour prendre en charge des ordinateurs virtuels de génération 2 et le modèle de démarrage sécurisé de Microsoft|
|Système d’exploitation doit être généralisé (exécution de sysprep.exe) | L’approvisionnement de modèle implique spécialisé des machines virtuelles pour la charge de travail d’un client spécifique| 

> [!NOTE]
> Ne copiez pas le disque de modèle dans la bibliothèque VMM à ce stade. 

### <a name="required-packages-to-create-a-nano-server-template-disk"></a>Les packages pour créer un disque de modèle de Nano Server nécessaires

Si vous prévoyez d’exécuter Nano Server en tant que votre système d’exploitation invité dans des machines virtuelles protégées, vous devez vérifier que votre image de Nano Server inclut les packages suivants :

- Microsoft-NanoServer-Guest-Package
- Microsoft-NanoServer-SecureStartup-Package

## <a name="run-windows-update-on-the-template-operating-system"></a>Exécuter la mise à jour de Windows sur le système d’exploitation du modèle

Sur le disque de modèle, vérifiez que le système d’exploitation possède toutes les dernières mises à jour de Windows installées. Récemment mises à jour publiées améliorent la fiabilité du processus de protection de bout en bout : un processus qui risque d’échouer if complète le système d’exploitation du modèle n’est pas à jour.

## <a name="sign-and-protect-the-vhdx-with-the-template-disk-wizard"></a>Se connecter et de protéger le VHDX avec l’Assistant modèle de disque

Pour utiliser un disque de modèle avec des machines virtuelles protégées, le disque doit être signé et chiffré avec BitLocker. Pour ce faire, vous allez utiliser l’Assistant Création de disque protégé du modèle. Cet Assistant générer un hachage pour le disque et ajoutez-le à un catalogue de signatures de volume (VSC). La VSC est signé à l’aide d’un certificat que vous spécifiez et êtes utilisé pendant le processus d’approvisionnement pour garantir le disque en cours de déploiement pour un client n’a pas été modifié ou remplacé par un disque, que le client n’approuve pas. Enfin, BitLocker est installé sur système d’exploitation du disque (si ce n’est déjà fait) pour préparer le disque pour le chiffrement lors de la configuration de la machine virtuelle.

> [!NOTE]
> L’Assistant modèle de disque modifiera le disque de modèle que vous spécifiez sur place. Voulez-vous effectuer une copie de la non protégé VHDX avant d’exécuter l’Assistant pour mettre à jour le disque à une date ultérieure. Vous ne serez pas en mesure de modifier un disque qui a été protégé par l’Assistant modèle de disque.

Sur un ordinateur exécutant Windows Server 2016 (n’a pas besoin d’être un hôte service Guardian ou votre serveur VMM), procédez comme suit :

1. Copier le VHDX généralisé créé dans [préparer un système d’exploitation VHDX](#prepare-an-operating-system-vhdx) sur le serveur, si ce n’est pas déjà fait.

2. Installer le **outils de machine virtuelle protégée** la fonctionnalité **outils d’Administration de serveur distant** sur l’ordinateur.

        Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart

3. Obtenir ou créer un certificat pour signer le VHDX qui deviendra le disque de modèle pour les nouvelles machines virtuelles protégées. Pour plus d’informations sur ce certificat seront incorporées dans un fichier de données de protection, qui autorise le disque comme disque de confiance. Par conséquent, il est important obtenir ce certificat à partir d’une autorité de certification que vous et votre hébergement approbation de fournisseur de service. Dans les scénarios d’entreprise où vous êtes l’hébergeur et le client, vous pouvez envisager d’émettre ce certificat à partir de votre infrastructure à clé publique.

    Si vous installez un environnement de test et que vous souhaitez simplement utiliser un certificat auto-signé pour signer votre disque de modèle, exécutez une commande semblable à ce qui suit sur votre ordinateur :

        New-SelfSignedCertificate -DnsName publisher.fabrikam.com

4. Démarrer le **Assistant disque de modèle** à partir de la **outils d’administration** dossier dans le menu Démarrer ou en tapant **TemplateDiskWizard.exe** dans une invite de commandes.

5. Sur le **certificat** , cliquez sur **Parcourir** pour afficher une liste de certificats. Sélectionnez le certificat avec lequel signer le modèle de disque. Cliquez sur **OK** , puis sur **Suivant**.

6. Dans la page de disque virtuel, cliquez sur **Parcourir** pour sélectionner le VHDX que vous avez préparé, puis cliquez sur **suivant**.

7. Dans la page catalogue de signatures, fournir un convivial **nom du disque** et **version.** Ces champs sont présents pour vous aider à identifier le disque une fois qu’il a été signé.

    Par exemple, pour **nom du disque** vous pouvez taper _WS2016_ et pour **Version**, _1.0.0.0_

8. Passez en revue vos sélections dans la page vérifier les paramètres de l’Assistant. Lorsque vous cliquez sur **générer**, l’Assistant Activation de BitLocker sur le disque de modèle, calculer le hachage du disque et créer le catalogue de signatures de Volume, qui est stocké dans les métadonnées VHDX.

    Attendez que le processus de signature est terminé avant d’essayer de monter ou de déplacer le disque de modèle. Ce processus peut prendre un certain temps, selon la taille de votre disque. 

9. Sur le **Résumé** page, les informations sur le modèle de disque, le certificat utilisé pour signer le modèle, et de l’émetteur du certificat est affiché. Cliquez sur **Fermer** pour quitter l'Assistant.


Indiquez le modèle de disque protégé pour le fournisseur de services, ainsi que d’un fichier de données de protection que vous créez, comme décrit dans [création de données pour définir une machine virtuelle protégée de protection](guarded-fabric-tenant-creates-shielding-data.md).

## <a name="see-also"></a>Voir aussi

- [Déployer des machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Structure protégée et machines virtuelles protégées](guarded-fabric-and-shielded-vms-top-node.md)
