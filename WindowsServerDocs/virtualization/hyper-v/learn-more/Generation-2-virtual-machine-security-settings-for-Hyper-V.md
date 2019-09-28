---
title: Paramètres de sécurité de l’ordinateur virtuel de génération 2 pour Hyper-V
description: Décrit les paramètres de sécurité disponibles dans le Gestionnaire Hyper-V pour les ordinateurs virtuels de génération 2
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
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364236"
---
# <a name="generation-2-virtual-machine-security-settings-for-hyper-v"></a>Paramètres de sécurité de l’ordinateur virtuel de génération 2 pour Hyper-V

>S'applique à : Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Utilisez les paramètres de sécurité de la machine virtuelle dans le Gestionnaire Hyper-V pour protéger les données et l’état d’une machine virtuelle. Vous pouvez protéger les machines virtuelles contre l’inspection, le vol et la falsification des logiciels malveillants qui s’exécutent sur l’ordinateur hôte, ainsi que les administrateurs du centre de. Le niveau de sécurité que vous recevez dépend du matériel hôte que vous exécutez, de la génération de la machine virtuelle et de la configuration du service, appelée service Guardian hôte, qui autorise les hôtes à démarrer des ordinateurs virtuels protégés.  

Le service Guardian hôte est un nouveau rôle dans Windows Server 2016. Il identifie les ordinateurs hôtes Hyper-V légitimes et leur permet d’exécuter une machine virtuelle donnée. Le plus souvent, vous configurez le service Guardian hôte pour un centre de centres. Toutefois, vous pouvez créer une machine virtuelle protégée pour l’exécuter localement sans configurer de service Guardian hôte. Vous pouvez distribuer ultérieurement l’ordinateur virtuel protégé à une infrastructure Guardian hôte.  

Si vous n’avez pas configuré le service Guardian hôte ou si vous l’exécutez en mode local sur l’hôte Hyper-V et que l’ordinateur hôte a la clé Guardian du propriétaire de l’ordinateur virtuel, vous pouvez modifier les paramètres décrits dans cette rubrique.   Le propriétaire d’une clé de gardien est une organisation qui crée et partage une clé privée ou publique pour posséder toutes les machines virtuelles créées avec cette clé.  

Pour savoir comment améliorer la sécurité de vos machines virtuelles avec le service Guardian hôte, consultez les ressources suivantes.  

- [Renforcer l’infrastructure : Protection des secrets des locataires dans Hyper-V (allumage vidéo) ](https://go.microsoft.com/fwlink/?LinkId=746379)
- [Infrastructure protégée et machines virtuelles dotées d’une protection maximale](https://go.microsoft.com/fwlink/?LinkId=746381)

## <a name="secure-boot-setting-in-hyper-v-manager"></a>Paramètre de démarrage sécurisé dans le Gestionnaire Hyper-V  

Le démarrage sécurisé est une fonctionnalité disponible avec les ordinateurs virtuels de génération 2, qui permet d’empêcher l’exécution de microprogrammes, de systèmes d’exploitation ou d’Unified Extensible Firmware Interface (UEFI) non autorisés au moment du démarrage. Le démarrage sécurisé est activé par défaut. Vous pouvez utiliser le démarrage sécurisé avec les ordinateurs virtuels de génération 2 qui exécutent des systèmes d’exploitation de distribution Windows ou Linux.  

Les modèles décrits dans le tableau ci-dessous font référence aux certificats dont vous avez besoin pour vérifier l’intégrité du processus de démarrage.  

|Nom du modèle|Description|  
|-----------------|---------------|  
|Microsoft Windows|Sélectionnez cette option pour sécuriser le démarrage de la machine virtuelle pour un système d’exploitation Windows.|  
|Autorité de certification Microsoft UEFI|Sélectionnez cette option pour sécuriser le démarrage de la machine virtuelle pour un système d’exploitation de distribution Linux.|  
|Machine virtuelle protégée Open source|Ce modèle est utilisé pour sécuriser le démarrage des [machines virtuelles protégées basées sur Linux](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-create-a-linux-shielded-vm-template).|

Pour plus d’informations, voir les rubriques suivantes :  

- [Vue d’ensemble de la sécurité dans Windows 10](https://docs.microsoft.com/windows/security/threat-protection/overview-of-threat-mitigations-in-windows-10)  
- [Dois-je créer une machine virtuelle de génération 1 ou 2 dans Hyper-V ?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
- [Machines virtuelles Linux et FreeBSD sur Hyper-V](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Paramètres de prise en charge du chiffrement dans le Gestionnaire Hyper-V

Vous pouvez protéger les données et l’état de la machine virtuelle en sélectionnant les options de support de chiffrement suivantes.  

- **Activer les module de plateforme sécurisée (TPM)** : ce paramètre rend un processeur de module de plateforme sécurisée (TPM) (TPM) virtualisé disponible pour votre machine virtuelle. Cela permet à l’invité de chiffrer le disque de la machine virtuelle à l’aide de BitLocker.
  - Si votre hôte Hyper-V exécute Windows 10 1511, vous devez activer le mode utilisateur isolé. 
- **Chiffrer l’État et le trafic de migration de machine virtuelle** : chiffre l’état enregistré de la machine virtuelle et le trafic de migration dynamique.

### <a name="enable-isolated-user-mode"></a>Activer le mode utilisateur isolé

Si vous sélectionnez **activer la module de plateforme sécurisée (TPM)** sur les hôtes Hyper-V qui exécutent des versions de Windows antérieures à la mise à jour anniversaire de Windows 10, vous devez activer le mode utilisateur isolé. Vous n’avez pas besoin d’effectuer cette opération pour les hôtes Hyper-V qui exécutent Windows Server 2016 ou la mise à jour anniversaire Windows 10 ou version ultérieure.

Le mode utilisateur isolé est l’environnement d’exécution qui héberge les applications de sécurité au sein du mode sécurisé virtuel sur l’hôte Hyper-V. Le mode sécurisé virtuel est utilisé pour sécuriser et protéger l’état de la puce TPM virtuelle.  

Pour activer le mode utilisateur isolé sur l’hôte Hyper-V qui exécute des versions antérieures de Windows 10,  

1.  Ouvrez Windows PowerShell en tant qu'administrateur.  

2.  Exécutez les commandes suivantes :  

    ```  
    Enable-WindowsOptionalFeature -Feature IsolatedUserMode -Online  
    New-Item -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Force  
    New-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name EnableVirtualizationBasedSecurity -Value 1 -PropertyType DWord -Force  

    ```  

Vous pouvez migrer un ordinateur virtuel avec un module de plateforme sécurisée (TPM) virtuel activé sur n’importe quel hôte exécutant Windows Server 2016, Windows 10 Build 10586 ou versions ultérieures. Toutefois, si vous la migrez vers un autre hôte, vous ne pourrez peut-être pas le démarrer. Vous devez mettre à jour le protecteur de clé de cet ordinateur virtuel pour autoriser le nouvel hôte à exécuter l’ordinateur virtuel. Pour plus d’informations, consultez [infrastructure protégée et machines virtuelles dotées](https://go.microsoft.com/fwlink/?LinkId=746381) d’une protection maximale et [Configuration système requise pour Hyper-V sur Windows Server](../System-requirements-for-Hyper-V-on-Windows.md).  

## <a name="security-policy-in-hyper-v-manager"></a>Stratégie de sécurité dans le Gestionnaire Hyper-V  
Pour plus de sécurité des machines virtuelles, utilisez l’option **activer la protection** pour désactiver les fonctionnalités de gestion telles que connexion à la console, PowerShell direct et certains composants d’intégration. Si vous sélectionnez cette option, les options **Démarrage sécurisé**, **activer le module de plateforme sécurisée (TPM)** et **chiffrer le trafic de migration de machine virtuelle** sont sélectionnées et appliquées.   

Vous pouvez exécuter l’ordinateur virtuel protégé localement sans configurer de service Guardian hôte. Toutefois, si vous la migrez vers un autre hôte, vous ne pourrez peut-être pas le démarrer. Vous devez mettre à jour le protecteur de clé de cet ordinateur virtuel pour autoriser le nouvel hôte à exécuter l’ordinateur virtuel. Pour plus d’informations, consultez [Présentation de Guarded Fabric et des ordinateurs virtuels protégés](https://go.microsoft.com/fwlink/?LinkId=746381).  

Pour plus d’informations sur la sécurité dans Windows Server, consultez [sécurité et assurance](../../../security/Security-and-Assurance.md).  
