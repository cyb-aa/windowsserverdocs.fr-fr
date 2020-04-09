---
title: Machines virtuelles protégées pour les locataires-création d’un disque de modèle-facultatif
ms.prod: windows-server
ms.topic: article
ms.assetid: c1992f8b-6f88-4dbc-b4a5-08368bba2787
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 1f51a0f90f60847929f6fe46732c98f355a6a859
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856442"
---
# <a name="shielded-vms-for-tenants---creating-a-template-disk-optional"></a>Machines virtuelles protégées pour les locataires-création d’un disque de modèle (facultatif)

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Pour créer une nouvelle machine virtuelle protégée, vous devez utiliser un disque de modèle signé spécialement préparé. Les métadonnées des disques de modèles signés permettent de s’assurer que les disques ne sont pas modifiés une fois qu’ils ont été créés et vous permettent de limiter les disques pouvant être utilisés pour créer vos machines virtuelles protégées. Pour ce faire, vous pouvez le faire pour vous, le locataire, pour le créer, comme décrit dans cette rubrique. 

> [!IMPORTANT]
> Si vous préférez, vous pouvez utiliser à la place un disque de modèle fourni par votre fournisseur de services d’hébergement. Dans ce cas, il est important de déployer une machine virtuelle de test à l’aide de ce disque de modèle et d’exécuter vos propres outils (antivirus, scanneurs de vulnérabilité, etc.) pour valider que le disque est, en fait, dans un état de confiance.

## <a name="prepare-an-operating-system-vhdx"></a>Préparer le VHDX d’un système d’exploitation

Pour créer un disque de modèle protégé, vous devez d’abord préparer un disque de système d’exploitation qui sera exécuté via l’Assistant disque de modèle. Ce disque sera utilisé comme disque de système d’exploitation dans les machines virtuelles protégées. Vous pouvez utiliser n’importe quel outil existant pour créer ce disque, tel que Microsoft Desktop image Service Manager (DISM), ou configurer manuellement une machine virtuelle avec un VHDX vide et installer le système d’exploitation sur ce disque. Quand vous configurez le disque, celui-ci doit respecter les conditions suivantes qui sont spécifiques aux machines virtuelles de génération 2 et/ou protégées : 

| Exigence pour VHDX | Raison |
|-----------|----|
|Doit être un disque de table de partition GUID (GPT) | Nécessaire pour que les ordinateurs virtuels de génération 2 prennent en charge UEFI|
|Le type de disque doit être de **base** et non **dynamique**. <br>Remarque : cela fait référence au type de disque logique, et non à la fonctionnalité VHDX « extensible dynamiquement » prise en charge par Hyper-V. | BitLocker ne prend pas en charge les disques dynamiques.|
|Le disque comporte au moins deux partitions. Une partition doit inclure le lecteur sur lequel Windows est installé. Il s’agit du lecteur que BitLocker chiffrera. L’autre partition est la partition active, qui contient le chargeur de démarrage et reste non chiffrée afin que l’ordinateur puisse être démarré.|Requis pour BitLocker|
|Le système de fichiers est NTFS | Requis pour BitLocker|
|Le système d’exploitation installé sur le VHDX est l’un des éléments suivants :<br>-Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 <br>-Windows 10, Windows 8.1, Windows 8| Nécessaire pour prendre en charge les ordinateurs virtuels de 2e génération et le modèle de démarrage sécurisé Microsoft|
|Le système d’exploitation doit être généralisé (exécuter Sysprep. exe) | L’approvisionnement de modèles implique la spécialisation des machines virtuelles pour la charge de travail d’un locataire spécifique| 

> [!NOTE]
> Ne copiez pas le disque de modèle dans la bibliothèque VMM à ce niveau. 

### <a name="required-packages-to-create-a-nano-server-template-disk"></a>Packages requis pour créer un disque de modèle nano Server

Si vous envisagez d’exécuter nano Server comme système d’exploitation invité dans des machines virtuelles protégées, vous devez vous assurer que votre image nano Server comprend les packages suivants :

- Microsoft-NanoServer-Guest-Package
- Microsoft-NanoServer-SecureStartup-Package

## <a name="run-windows-update-on-the-template-operating-system"></a>Exécuter Windows Update sur le modèle de système d’exploitation

Sur le disque de modèle, vérifiez que toutes les mises à jour Windows les plus récentes sont installées sur le système d’exploitation. Les mises à jour récemment publiées améliorent la fiabilité du processus de protection de bout en bout, un processus qui peut échouer si le système d’exploitation du modèle n’est pas à jour.

## <a name="sign-and-protect-the-vhdx-with-the-template-disk-wizard"></a>Signer et protéger le VHDX à l’aide de l’Assistant Création d’un disque de modèle

Pour utiliser un disque de modèle avec des machines virtuelles dotées d’une protection maximale, le disque doit être signé et chiffré avec BitLocker. Pour ce faire, vous allez utiliser l’Assistant de création de disque de modèle protégé. Cet Assistant génère un hachage pour le disque et l’ajoute à un catalogue de signatures de volume (VSC). Le VSC est signé à l’aide d’un certificat que vous spécifiez et il est utilisé pendant le processus de configuration pour garantir que le disque en cours de déploiement pour un locataire n’a pas été modifié ou remplacé par un disque non approuvé par le locataire. Enfin, BitLocker est installé sur le système d’exploitation du disque (s’il n’est pas déjà présent) pour préparer le disque au chiffrement lors de l’approvisionnement de la machine virtuelle.

> [!NOTE]
> L’Assistant disque de modèle va modifier le disque de modèle que vous spécifiez sur place. Vous pouvez effectuer une copie du VHDX non protégé avant d’exécuter l’Assistant pour effectuer ultérieurement des mises à jour sur le disque. Vous ne pourrez pas modifier un disque qui a été protégé à l’aide de l’Assistant Création d’un disque de modèle.

Effectuez les étapes suivantes sur un ordinateur exécutant Windows Server 2016 (il n’est pas nécessaire qu’il s’agit d’un hôte service Guardian ou de votre serveur VMM) :

1. Copiez le VHDX généralisé créé dans [préparer un système d’exploitation vhdx](#prepare-an-operating-system-vhdx) au serveur, s’il n’est pas déjà présent.

2. Installez la fonctionnalité **outils de machine virtuelle protégée** à partir de **Outils d’administration de serveur distant** sur l’ordinateur.

        Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart

3. Obtenez ou créez un certificat pour signer le VHDX qui deviendra le disque de modèle pour les nouvelles machines virtuelles protégées. Les détails relatifs à ce certificat seront incorporés dans un fichier de données de protection, qui autorise le disque en tant que disque approuvé. Par conséquent, il est important d’obtenir ce certificat auprès d’une autorité de certification que vous et votre fournisseur de services d’hébergement approuvent. Dans les scénarios d’entreprise où vous êtes à la fois l’hébergeur et le locataire, vous pouvez envisager d’émettre ce certificat à partir de votre infrastructure à clé publique.

    Si vous configurez un environnement de test et que vous souhaitez simplement utiliser un certificat auto-signé pour signer votre disque de modèle, exécutez une commande semblable à la suivante sur votre ordinateur :

        New-SelfSignedCertificate -DnsName publisher.fabrikam.com

4. Démarrez l' **Assistant disque de modèle** à partir du dossier **Outils d’administration** du menu Démarrer ou en tapant **TemplateDiskWizard. exe** dans une invite de commandes.

5. Sur la page **certificat** , cliquez sur **Parcourir** pour afficher la liste des certificats. Sélectionnez le certificat avec lequel signer le modèle de disque. Cliquez sur **OK**, puis sur **Suivant**.

6. Sur la page disque virtuel, cliquez sur **Parcourir** pour sélectionner le VHDX que vous avez préparé, puis cliquez sur **suivant**.

7. Sur la page catalogue de signatures, indiquez un **nom de disque** et une **version conviviaux.** Ces champs sont présents pour vous aider à identifier le disque une fois qu’il a été signé.

    Par exemple, pour le **nom de disque** , vous pouvez taper _WS2016_ et pour **version**, _1.0.0.0_

8. Passez en revue vos sélections dans la page vérifier les paramètres de l’Assistant. Lorsque vous cliquez sur **générer**, l’Assistant Active BitLocker sur le disque de modèle, calcule le hachage du disque et crée le catalogue de signatures de volume, qui est stocké dans les métadonnées VHDX.

    Attendez la fin du processus de signature avant de tenter de monter ou de déplacer le disque de modèle. Ce processus peut prendre un certain temps, en fonction de la taille de votre disque. 

9. Sur la page **Résumé** , vous pouvez afficher des informations sur le modèle de disque, le certificat utilisé pour signer le modèle et l’émetteur du certificat. Cliquez sur **Fermer** pour quitter l'Assistant.


Fournissez le modèle de disque protégé au fournisseur de services d’hébergement, ainsi qu’un fichier de données de protection que vous créez, comme décrit dans [création de données de protection pour définir une machine virtuelle protégée](guarded-fabric-tenant-creates-shielding-data.md).

## <a name="see-also"></a>Voir aussi

- [Déployer des machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Structure protégée et machines virtuelles dotées d’une protection maximale](guarded-fabric-and-shielded-vms-top-node.md)
