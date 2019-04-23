---
title: Machines virtuelles protégées - préparation d’une machine virtuelle VHD d’assistance de protection
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 0e3414cf-98ca-4e91-9e8d-0d7bce56033b
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 8e14cdeed435f23f28ca514e232fbcfa6220fc74
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887720"
---
# <a name="shielded-vms---preparing-a-vm-shielding-helper-vhd"></a>Machines virtuelles protégées - préparation d’une machine virtuelle VHD d’assistance de protection

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

<!-- This comment creates a break between the Applies To above and the Important note below. -->

> [!IMPORTANT]
> Avant de commencer ces procédures, vérifiez que vous avez installé la mise à jour cumulative la plus récente pour Windows Server 2016 ou sont à l’aide de la plus récente de Windows 10 [outils d’Administration de serveur distant](https://www.microsoft.com/en-us/download/details.aspx?id=45520). Sinon, les procédures ne fonctionnera pas. 

Cette section décrit les étapes effectuées par un fournisseur de services pour activer la prise en charge pour la conversion de machines virtuelles existantes en machines virtuelles protégées.

Pour comprendre comment cette rubrique s’intègre dans le processus de déploiement de machines virtuelles protégées, consultez [hébergeant des étapes de configuration de fournisseur de service pour les hôtes service Guardian et des machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="which-vms-can-be-shielded"></a>Auquel les machines virtuelles peuvent être protégées ?

Le processus de protection des machines virtuelles existantes n’est disponible pour les machines virtuelles qui remplissent les conditions préalables suivantes :

- Le système d’exploitation invité est Windows Server 2012, 2012 R2, 2016 ou un semi-annuel channel mise en production. Impossible de convertir des machines virtuelles Linux existantes en machines virtuelles protégées.
- La machine virtuelle est une génération 2 (microprogramme UEFI) de machine virtuelle
- La machine virtuelle n’utilise pas de disques de différenciation pour son volume de système d’exploitation.

## <a name="prepare-helper-vhd"></a>Préparer le VHD d’assistance

1.  Sur un ordinateur avec Hyper-V et la fonctionnalité Outils d’Administration de serveur distant **outils de machine virtuelle protégée** installé, créez une nouvelle génération 2 machine virtuelle avec un VHDX vide et l’installation de Windows Server 2016 à l’aide de l’installation de l’image ISO de Windows Server média. Cette machine virtuelle ne doit pas être protégée et devez exécuter Server Core ou Server avec la fonctionnalité expérience utilisateur.

    > [!IMPORTANT]
    > Le VHD d’assistance de protection de machine virtuelle **ne doit pas** être liées pour les disques de modèle que vous avez créé dans [fournisseur de services d’hébergement crée un modèle de machine virtuelle protégée](guarded-fabric-create-a-shielded-vm-template.md). Si vous réutilisez un disque de modèle, il y aura une collision de signature de disque pendant le processus de protection, car les deux disques aura le même identificateur de disque GPT. Vous pouvez éviter cela en créant un nouveau disque dur virtuel (vide) et de l’installation de Windows Server 2016 à l’aide de votre support d’installation ISO.

2.  Démarrez la machine virtuelle, effectuez les étapes d’installation et ouvrir une session dans le bureau. Une fois que vous avez vérifié que la machine virtuelle est en cours de fonctionnement, arrêtez la machine virtuelle.

3.  Dans une fenêtre Windows PowerShell avec élévation de privilèges, exécutez la commande suivante pour préparer le disque VHDX créé précédemment pour qu’il devienne un disque d’assistance protection de machine virtuelle. Mettre à jour le chemin d’accès avec le chemin d’accès correct pour votre environnement.

    ```powershell
    Initialize-VMShieldingHelperVHD -Path 'C:\VHD\shieldingHelper.vhdx'
    ```

4.  Une fois que la commande a réussi, copiez le disque VHDX à votre partage de bibliothèque VMM. **Ne le faites pas** redémarrer la machine virtuelle à l’étape 1. Cela sera endommager le disque d’assistance.

5.  Vous pouvez maintenant supprimer la machine virtuelle à l’étape 1 dans Hyper-V.

## <a name="configure-vmm-host-guardian-server-settings"></a>Configurer les paramètres du serveur Guardian hôte VMM

Dans la Console VMM, ouvrez le volet Paramètres, puis **paramètres du Service Guardian hôte** sous **général**. En bas de cette fenêtre, il existe un champ pour configurer l’emplacement de votre disque dur virtuel d’assistance. Utilisez le bouton Parcourir pour sélectionner le disque dur virtuel à partir de votre partage de bibliothèque. Si vous ne voyez pas votre disque dans le partage, vous devrez peut-être actualiser manuellement la bibliothèque dans VMM pour qu’il s’affiche.

![VMM - paramètres du Service Guardian hôte](../media/Guarded-Fabric-Shielded-VM/guarded-host-vmm-hgs-settings-01.png)

## <a name="see-also"></a>Voir aussi

- [Hébergement des étapes de configuration de fournisseur de service pour les hôtes service Guardian et des machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Structure protégée et machines virtuelles protégées](guarded-fabric-and-shielded-vms-top-node.md)
