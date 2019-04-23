---
title: Paramètres de sécurité de machine virtuelle de génération 2 pour Hyper-V
description: Décrit les paramètres de sécurité disponibles dans le Gestionnaire Hyper-V pour les machines virtuelles de génération 2
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 06ab4f5f-6b8e-4058-8108-76785aa93d4c
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 90a2b7234ee55d8469b6e02ba3de3a0efc080a3e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889500"
---
# <a name="generation-2-virtual-machine-security-settings-for-hyper-v"></a>Paramètres de sécurité de machine virtuelle de génération 2 pour Hyper-V

>S'applique à : Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Utilisez les paramètres de sécurité de machine virtuelle dans le Gestionnaire Hyper-V pour aider à protéger les données et l’état d’une machine virtuelle. Vous pouvez protéger les machines virtuelles à partir de l’inspection, le vol et la falsification de contre les programmes malveillants qui peuvent s’exécuter sur l’hôte et les administrateurs de centre de données. Le niveau de sécurité que vous obtenez dépend du matériel hôte que vous exécutez, la génération de la machine virtuelle, et si vous configurez le service, appelé le Service Guardian hôte, qui autorise les hôtes pour démarrer des machines virtuelles protégées.  

Le Service Guardian hôte est un nouveau rôle dans Windows Server 2016. Il identifie les hôtes Hyper-V légitimes et leur permet d’exécuter une machine virtuelle donnée. Généralement, vous devez définir le service Guardian hôte pour un centre de données. Mais vous pouvez créer une machine virtuelle protégée pour l’exécuter localement sans configurer un Service de surveillance d’hôte. Vous pouvez distribuer plus tard de la machine virtuelle protégée à une infrastructure de Guardian hôte.  

Si vous n’avez pas configuré le Service Guardian hôte ou que vous l’exécutez en mode local sur l’ordinateur hôte Hyper-V et que l’hôte a la clé du gardien du propriétaire de la machine virtuelle, vous pouvez modifier les paramètres décrits dans cette rubrique.   Un propriétaire d’une clé de gardien est une organisation qui crée et partages une clé privée ou publique à posséder toutes les machines virtuelles créée avec cette clé.  

Pour savoir comment vous pouvez renforcer vos machines virtuelles avec le Service Guardian hôte, consultez les ressources suivantes.  

- [Renforcer l’infrastructure : Protéger les Secrets des locataires dans Hyper-V (Ignite vidéo)](https://go.microsoft.com/fwlink/?LinkId=746379)
- [Structure protégée et machines virtuelles protégées](https://go.microsoft.com/fwlink/?LinkId=746381)

## <a name="secure-boot-setting-in-hyper-v-manager"></a>Sécuriser le paramètre de démarrage dans le Gestionnaire Hyper-V  

Le démarrage sécurisé est une fonctionnalité disponible avec les ordinateurs virtuels de génération 2 qui contribue à empêcher les microprogrammes, systèmes d’exploitation ou pilotes Unified Extensible Firmware Interface (UEFI) (également appelés ROM optionnelles) de l’exécution au moment du démarrage. Le démarrage sécurisé est activé par défaut. Vous pouvez utiliser le démarrage sécurisé avec des machines virtuelles de génération 2 qui exécutent des systèmes d’exploitation de distribution de Windows ou Linux.  

Les modèles décrits dans le tableau suivant font référence aux certificats dont vous avez besoin pour vérifier l’intégrité du processus de démarrage.  

|Nom du modèle|Description|  
|-----------------|---------------|  
|Microsoft Windows|Sélectionnez cette option pour le démarrage sécurisé la machine virtuelle pour un système d’exploitation de Windows.|  
|Autorité de certification Microsoft UEFI|Sélectionnez cette option pour le démarrage sécurisé la machine virtuelle pour un système d’exploitation de distribution Linux.|  
|Ouvrir la Source de machine virtuelle protégée|Ce modèle est utilisé pour le démarrage sécurisé pour [basés sur Linux de machines virtuelles protégées](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-create-a-linux-shielded-vm-template).|

Pour plus d’informations, voir les rubriques suivantes :  

- [Vue d’ensemble de la sécurité Windows 10](https://docs.microsoft.com/windows/security/threat-protection/overview-of-threat-mitigations-in-windows-10)  
- [Dois-je créer une machine virtuelle de génération 1 ou 2 dans Hyper-V ?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
- [Linux et les Machines virtuelles de FreeBSD sur Hyper-V](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Paramètres de prise en charge de chiffrement dans le Gestionnaire Hyper-V

Vous pouvez protéger les données et l’état de la machine virtuelle en sélectionnant les options de prise en charge de chiffrement suivantes.  

- **Activer le Module de plateforme sécurisée** -, ce paramètre rend une puce de Module de plateforme sécurisée (TPM) virtualisé disponible à votre machine virtuelle. Ainsi, l’invité pour chiffrer le disque de machine virtuelle à l’aide de BitLocker.
  - Si votre hôte Hyper-V est en cours d’exécution Windows 10 1511, vous devez activer le Mode utilisateur isolé. 
- **Chiffrer le trafic de migration d’état et la machine virtuelle** - chiffre l’état enregistré de l’ordinateur virtuel et le trafic de migration en direct.

### <a name="enable-isolated-user-mode"></a>Activer le Mode utilisateur isolé

Si vous sélectionnez **activer le Module de plateforme sécurisée** sur les hôtes Hyper-V qui exécutent des versions de Windows antérieures à la mise à jour anniversaire de Windows 10, vous devez activer le Mode utilisateur isolé. Vous n’avez pas besoin de procéder ainsi pour Hyper-V héberge cette exécution de Windows Server 2016 ou une mise à jour anniversaire de Windows 10 ou version ultérieure.

Mode utilisateur isolé est l’environnement d’exécution qui héberge les applications de sécurité dans le Mode sécurisé virtuel sur l’hôte Hyper-V. Le Mode sécurisé virtuel est utilisé pour sécuriser et protéger l’état de la puce TPM virtuelle.  

Pour activer le Mode utilisateur isolé sur l’hôte Hyper-V qui exécutent des versions antérieures de Windows 10,  

1.  Ouvrez Windows PowerShell en tant qu'administrateur.  

2.  Exécutez les commandes suivantes :  

    ```  
    Enable-WindowsOptionalFeature -Feature IsolatedUserMode -Online  
    New-Item -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Force  
    New-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name EnableVirtualizationBasedSecurity -Value 1 -PropertyType DWord -Force  

    ```  

Vous pouvez migrer une machine virtuelle avec le module TPM virtuel activé pour n’importe quel hôte qui exécute Windows Server 2016, Windows 10 build 10586 ou une version ultérieure des versions. Mais si vous la migrer vers un autre hôte, vous ne serez peut-être pas en mesure de démarrer l’application. Vous devez mettre à jour le protecteur de clé pour cette machine virtuelle pour autoriser le nouvel ordinateur hôte pour exécuter la machine virtuelle. Pour plus d’informations, consultez [infrastructure service Guardian et des machines virtuelles protégées](https://go.microsoft.com/fwlink/?LinkId=746381) et [configuration système requise pour Hyper-V sur Windows Server](../System-requirements-for-Hyper-V-on-Windows.md).  

## <a name="security-policy-in-hyper-v-manager"></a>Stratégie de sécurité dans le Gestionnaire Hyper-V  
Pour plus de sécurité de machine virtuelle, utilisez le **activer la protection** option pour désactiver les fonctionnalités de gestion telles que la connexion à la console, PowerShell Direct et certains composants d’intégration. Si vous sélectionnez cette option, **le démarrage sécurisé**, **activer le Module de plateforme sécurisée**, et **trafic de migration d’état de chiffrement et la machine virtuelle** options sont sélectionnées et appliquées.   

Vous pouvez exécuter la machine virtuelle protégée localement sans configurer un Service de surveillance d’hôte. Mais si vous la migrer vers un autre hôte, vous ne serez peut-être pas en mesure de démarrer l’application. Vous devez mettre à jour le protecteur de clé pour cette machine virtuelle pour autoriser le nouvel ordinateur hôte pour exécuter la machine virtuelle. Pour plus d’informations, consultez [Présentation de Guarded Fabric et des ordinateurs virtuels protégés](https://go.microsoft.com/fwlink/?LinkId=746381).  

Pour plus d’informations sur la sécurité dans Windows Server, consultez [sécurité et Assurance](../../../security/Security-and-Assurance.md).  
