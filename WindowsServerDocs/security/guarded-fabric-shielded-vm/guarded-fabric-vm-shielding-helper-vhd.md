---
title: Machines virtuelles protégées-préparation d’un disque dur virtuel d’assistance de protection de machine virtuelle
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 0e3414cf-98ca-4e91-9e8d-0d7bce56033b
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 2ab9d4afb6e4219c6e6aae23d2d58052f20d3998
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950327"
---
# <a name="shielded-vms---preparing-a-vm-shielding-helper-vhd"></a>Machines virtuelles protégées-préparation d’un disque dur virtuel d’assistance de protection de machine virtuelle

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

> [!IMPORTANT]
> Avant de commencer ces procédures, assurez-vous que vous avez installé la dernière mise à jour cumulative pour Windows Server 2016 ou que vous utilisez la dernière version de Windows 10 [Outils d’administration de serveur distant](https://www.microsoft.com/download/details.aspx?id=45520). Dans le cas contraire, les procédures ne fonctionneront pas. 

Cette section décrit les étapes effectuées par un fournisseur de services d’hébergement pour activer la prise en charge de la conversion de machines virtuelles existantes en machines virtuelles protégées.

Pour comprendre le fonctionnement de cette rubrique dans le processus global de déploiement de machines virtuelles protégées, consultez [étapes de configuration du fournisseur de services d’hébergement pour les hôtes service Guardian et les machines virtuelles](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)protégées.

## <a name="which-vms-can-be-shielded"></a>Quelles machines virtuelles peuvent être protégées ?

Le processus de protection des machines virtuelles existantes n’est disponible que pour les machines virtuelles qui remplissent les conditions préalables suivantes :

- Le système d’exploitation invité est Windows Server 2012, 2012 R2, 2016 ou une version de canal semi-annuel. Les machines virtuelles Linux existantes ne peuvent pas être converties en machines virtuelles protégées.
- La machine virtuelle est une machine virtuelle de génération 2 (microprogramme UEFI)
- La machine virtuelle n’utilise pas de disques de différenciation pour son volume de système d’exploitation.

## <a name="prepare-helper-vhd"></a>Préparer le disque dur virtuel d’assistance

1.  Sur un ordinateur doté d’Hyper-V et de la fonctionnalité de Outils d’administration de serveur distant **outils de machines virtuelles dotées** d’une protection maximale, créez une nouvelle machine virtuelle de 2e génération avec un VHDX vide et installez Windows Server 2016 sur celle-ci à l’aide du support d’installation Windows Server ISO. Cette machine virtuelle ne doit pas être protégée et doit exécuter Server Core ou Server avec expérience utilisateur.

    > [!IMPORTANT]
    > Le disque dur virtuel d’assistance de protection de machine virtuelle **ne doit pas** être lié aux disques de modèle que vous avez créés dans le [fournisseur de services d’hébergement crée un modèle de machine virtuelle protégée](guarded-fabric-create-a-shielded-vm-template.md). Si vous réutilisez un disque de modèle, une collision de signature de disque se produira pendant le processus de protection, car les deux disques auront le même identificateur de disque GPT. Vous pouvez éviter cela en créant un nouveau disque dur virtuel (vide) et en installant Windows Server 2016 sur celui-ci à l’aide de votre support d’installation ISO.

2.  Démarrez la machine virtuelle, effectuez toutes les étapes de configuration et connectez-vous au bureau. Une fois que vous avez vérifié que la machine virtuelle est en état de fonctionnement, arrêtez la machine virtuelle.

3.  Dans une fenêtre Windows PowerShell avec élévation de privilèges, exécutez la commande suivante pour préparer le VHDX créé précédemment pour qu’il devienne un disque d’assistance de protection de machine virtuelle. Mettez à jour le chemin d’accès avec le chemin d’accès correct pour votre environnement.

    ```powershell
    Initialize-VMShieldingHelperVHD -Path 'C:\VHD\shieldingHelper.vhdx'
    ```

4.  Une fois la commande terminée, copiez le VHDX sur votre partage de bibliothèque VMM. **Ne** redémarrez pas la machine virtuelle à partir de l’étape 1. Vous risquez d’endommager le disque d’assistance.

5.  Vous pouvez maintenant supprimer la machine virtuelle de l’étape 1 dans Hyper-V.

## <a name="configure-vmm-host-guardian-server-settings"></a>Configurer les paramètres du serveur Guardian hôte de VMM

Dans la console VMM, ouvrez le volet Paramètres, puis **hébergez paramètres du service Guardian** sous **général**. En bas de cette fenêtre, il existe un champ permettant de configurer l’emplacement de votre disque dur virtuel d’assistance. Utilisez le bouton Parcourir pour sélectionner le disque dur virtuel à partir de votre partage de bibliothèque. Si vous ne voyez pas votre disque dans le partage, vous devrez peut-être actualiser manuellement la bibliothèque dans VMM pour qu’elle s’affiche.

![VMM-paramètres du service Guardian hôte](../media/Guarded-Fabric-Shielded-VM/guarded-host-vmm-hgs-settings-01.png)

## <a name="see-also"></a>Articles associés

- [Étapes de configuration du fournisseur de services d’hébergement pour les hôtes service Guardian et les machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Structure protégée et machines virtuelles dotées d’une protection maximale](guarded-fabric-and-shielded-vms-top-node.md)
