---
title: Paramètres de sécurité des machines virtuelles de génération 2 pour Hyper-V
description: Décrit les paramètres de sécurité disponibles dans le Gestionnaire Hyper-V pour les machines virtuelles de génération 2
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 06ab4f5f-6b8e-4058-8108-76785aa93d4c
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 82544a58a8d46b3063605557be3c63cfa799e4fb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364236"
---
# <a name="generation-2-virtual-machine-security-settings-for-hyper-v"></a>Paramètres de sécurité des machines virtuelles de génération 2 pour Hyper-V

>S'applique à : Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Utilisez les paramètres de sécurité des machines virtuelles dans le Gestionnaire Hyper-V pour protéger les données et l’état d’une machine virtuelle. Vous pouvez protéger des machines virtuelles contre l’inspection, le vol et la falsification effectués par des logiciels malveillants qui s’exécutent sur l’hôte et par des administrateurs du centre de données. Le niveau de sécurité que vous obtenez dépend du matériel hôte que vous utilisez, de la génération de la machine virtuelle, et du fait que vous configurez ou non le service (appelé Service Guardian hôte) qui autorise les hôtes à démarrer des machines virtuelles dotées d’une protection maximale.  

Le Service Guardian hôte est un nouveau rôle dans Windows Server 2016. Il identifie les hôtes Hyper-V légitimes et leur permet d’exécuter une machine virtuelle donnée. Le plus souvent, vous configurez le Service Guardian hôte pour un centre de données. Vous pouvez néanmoins aussi créer une machine virtuelle dotée d’une protection maximale pour l’exécuter localement sans configurer un Service Guardian hôte. Vous pouvez distribuer ultérieurement la machine virtuelle dotée d’une protection maximale sur une structure fabric Guardian hôte.  

Si vous n’avez pas configuré le Service Guardian hôte ou si vous l’exécutez en mode local sur l’hôte Hyper-V, et que l’hôte a la clé Guardian du propriétaire de la machine virtuelle, vous pouvez changer les paramètres décrits dans cette rubrique.   Le propriétaire d’une clé Guardian est une organisation qui crée et partage une clé privée ou publique pour être propriétaire de toutes les machines virtuelles créées avec cette clé.  

Pour découvrir comment améliorer la sécurité de vos machines virtuelles avec le Service Guardian hôte, consultez les ressources suivantes.  

- [Renforcer l’infrastructure : Protéger les secrets des locataires dans Hyper-V (vidéo Ignite)](https://go.microsoft.com/fwlink/?LinkId=746379)
- [Structure fabric protégée et machines virtuelles dotées d’une protection maximale](https://go.microsoft.com/fwlink/?LinkId=746381)

## <a name="secure-boot-setting-in-hyper-v-manager"></a>Paramètre de démarrage sécurisé dans le Gestionnaire Hyper-V  

Le démarrage sécurisé est une fonctionnalité disponible avec les machines virtuelles de génération 2, qui aide à empêcher l’exécution des microprogrammes, des systèmes d’exploitation ou des pilotes UEFI (également appelés ROM facultatives) au moment du démarrage. Le démarrage sécurisé est activé par défaut. Vous pouvez utiliser le démarrage sécurisé avec les machines virtuelles de génération 2 qui exécutent des systèmes d’exploitation Windows ou d’une distribution Linux.  

Les modèles décrits dans le tableau ci-dessous font référence aux certificats dont vous avez besoin pour vérifier l’intégrité du processus de démarrage.  

|Nom du modèle|Description|  
|-----------------|---------------|  
|Microsoft Windows|Sélectionnez cette option pour sécuriser le démarrage de la machine virtuelle pour un système d’exploitation Windows.|  
|Autorité de certification UEFI Microsoft|Sélectionnez cette option pour sécuriser le démarrage de la machine virtuelle pour un système d’exploitation d’une distribution Linux.|  
|Machine virtuelle dotée d’une protection maximale Open Source|Ce modèle est utilisé pour sécuriser le démarrage de [machines virtuelles dotées d’une protection maximale basées sur Linux](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-create-a-linux-shielded-vm-template).|

Pour plus d’informations, voir les rubriques suivantes :  

- [Vue d’ensemble de la sécurité de Windows 10](https://docs.microsoft.com/windows/security/threat-protection/overview-of-threat-mitigations-in-windows-10)  
- [Dois-je créer une machine virtuelle de génération 1 ou 2 dans Hyper-V ?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
- [Machines virtuelles Linux et FreeBSD sur Hyper-V](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Paramètres de prise en charge du chiffrement dans le Gestionnaire Hyper-V

Vous pouvez protéger les données et l’état de la machine virtuelle en sélectionnant les options de prise en charge du chiffrement suivantes.  

- **Activer le module de plateforme sécurisée** - Ce paramètre rend une puce TPM virtualisée disponible pour votre machine virtuelle. Ceci permet à l’invité de chiffrer le disque de la machine virtuelle avec BitLocker.
  - Si votre hôte Hyper-V exécute Windows 10 1511, vous devez activer le mode utilisateur isolé. 
- **Chiffrer l’état et le trafic de migration de la machine virtuelle** - Chiffre l’état enregistré et le trafic de migration dynamique de la machine virtuelle.

### <a name="enable-isolated-user-mode"></a>Activer le mode utilisateur isolé

Si vous sélectionnez **Activer le module de plateforme sécurisée** sur les hôtes Hyper-V qui exécutent des versions de Windows antérieures à la Mise à jour anniversaire Microsoft Windows 10, vous devez activer le mode utilisateur isolé. Vous n’avez pas besoin d’effectuer cette opération pour les hôtes Hyper-V qui exécutent Windows Server 2016, ou la Mise à jour anniversaire Microsoft Windows 10 ou ultérieur.

Le mode utilisateur isolé est l’environnement d’exécution qui héberge les applications de sécurité au sein du mode sécurisé virtuel sur l’hôte Hyper-V. Le mode sécurisé virtuel est utilisé pour sécuriser et protéger l’état de la puce TPM virtuelle.  

Pour activer le mode utilisateur isolé sur l’hôte Hyper-V qui exécute des versions antérieures de Windows 10,  

1.  Ouvrez Windows PowerShell en tant qu'administrateur.  

2.  Exécutez les commandes suivantes :  

    ```  
    Enable-WindowsOptionalFeature -Feature IsolatedUserMode -Online  
    New-Item -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Force  
    New-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name EnableVirtualizationBasedSecurity -Value 1 -PropertyType DWord -Force  

    ```  

Vous pouvez migrer une machine virtuelle avec un module de plateforme sécurisée (TPM) virtuel activé sur n’importe quel hôte exécutant Windows Server 2016, Windows 10 build 10586 ou des versions ultérieures. Cependant, si vous la migrez vers un autre hôte, vous ne pourrez peut-être pas la démarrer. Vous devez mettre à jour le protecteur de clés de cette machine virtuelle pour autoriser le nouvel hôte à exécuter la machine virtuelle. Pour plus d’informations, consultez [Structure fabric protégée et machines virtuelles dotées d’une protection maximale](https://go.microsoft.com/fwlink/?LinkId=746381) et [Configuration système requise pour Hyper-V sur Windows Server](../System-requirements-for-Hyper-V-on-Windows.md).  

## <a name="security-policy-in-hyper-v-manager"></a>Stratégie de sécurité dans le Gestionnaire Hyper-V  
Pour plus de sécurité des machines virtuelles, utilisez l’option **Activer la protection** pour désactiver les fonctionnalités de gestion comme la connexion de la console, PowerShell Direct et certains composants d’intégration. Si vous sélectionnez cette option, les options **Démarrage sécurisé**, **Activer le module de plateforme sécurisée** et **Chiffrer l’état et le trafic de migration de la machine virtuelle** sont sélectionnées et appliquées.   

Vous pouvez exécuter la machine virtuelle dotée d’une protection maximale localement sans configurer un Service Guardian hôte. Cependant, si vous la migrez vers un autre hôte, vous ne pourrez peut-être pas la démarrer. Vous devez mettre à jour le protecteur de clés de cette machine virtuelle pour autoriser le nouvel hôte à exécuter la machine virtuelle. Pour plus d’informations, consultez [Structure fabric protégée et machines virtuelles dotées d’une protection maximale](https://go.microsoft.com/fwlink/?LinkId=746381).  

Pour plus d’informations sur la sécurité dans Windows Server, consultez [Sécurité et assurance](../../../security/Security-and-Assurance.md).  
