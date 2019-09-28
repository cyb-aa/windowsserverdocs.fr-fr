---
title: Planifier la sécurité Hyper-V dans Windows Server
description: Fournit une liste de considérations sur la sécurité pour les ordinateurs hôtes Hyper-v et les machines virtuelles
ms.prod: windows-server
ms.service: na
ms.suite: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 115db481-b57e-41c3-8354-504f4bc6113a
manager: dongill
author: larsiwer
ms.author: kathyDav
ms.date: 08/03/2018
ms.openlocfilehash: 8fd86ae500fff1e6b8c27b0d34d1dcbeeade9f81
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392961"
---
# <a name="plan-for-hyper-v-security-in-windows-server"></a>Planifier la sécurité Hyper-V dans Windows Server

>S'applique à : Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Sécurisez le système d’exploitation hôte Hyper-V, les machines virtuelles, les fichiers de configuration et les données des machines virtuelles. Utilisez la liste suivante de pratiques recommandées comme liste de vérification pour vous aider à sécuriser votre environnement Hyper-V.

## <a name="secure-the-hyper-v-host"></a>Sécuriser l’hôte Hyper-V
- **Sécurisez le système d’exploitation hôte.**
    - Réduisez la surface d’attaque en utilisant l’option d’installation minimale de Windows Server dont vous avez besoin pour le système d’exploitation de gestion. Pour plus d’informations, consultez la [section Options d’installation](/windows-server/windows-server#installation-options) de la bibliothèque de contenu technique de Windows Server. Nous vous déconseillons d’exécuter des charges de travail de production sur Hyper-V sur Windows 10.
    - Gardez à jour le système d’exploitation, le microprogramme et les pilotes de périphérique de l’hôte Hyper-V avec les dernières mises à jour de sécurité. Vérifiez les recommandations de votre fournisseur pour mettre à jour le microprogramme et les pilotes.
    - N’utilisez pas l’hôte Hyper-V en tant que station de travail ou installez les logiciels inutiles.
    - Gérez à distance l’hôte Hyper-V. Si vous devez gérer l’hôte Hyper-V localement, utilisez Credential Guard. Pour plus d’informations, consultez la section [Protéger les informations d’identification de domaine dérivées avec Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard).
    - Activez les stratégies d’intégrité du code. Utilisez des services d’intégrité du code protégés par la sécurité basée sur la virtualisation. Pour plus d’informations, consultez le [Guide de déploiement de Device Guard](https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide).
- **Utilisez un réseau sécurisé.**
    - Utilisez un réseau distinct avec une carte réseau dédiée pour l’ordinateur physique Hyper-V.
    - Utilisez un réseau privé ou sécurisé pour accéder aux configurations de machine virtuelle et aux fichiers de disque dur virtuel.
    - Utilisez un réseau privé/dédié pour votre trafic de migration dynamique. Envisagez d’activer IPSec sur ce réseau pour utiliser le chiffrement et sécuriser les données de votre machine virtuelle en transitant sur le réseau pendant la migration. Pour plus d’informations, consultez [configurer des hôtes pour la migration dynamique sans clustering de basculement](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md).
- **Trafic de migration de stockage sécurisé.** 

    Utilisez SMB 3,0 pour le chiffrement de bout en bout des données SMB et la falsification de la protection des données ou de l’écoute clandestine sur des réseaux non approuvés. Utilisez un réseau privé pour accéder au contenu du partage SMB afin d’éviter les attaques de l’intercepteur. Pour plus d’informations, consultez améliorations de la [sécurité SMB](https://technet.microsoft.com/library/dn551363.aspx). 
- **Configurez les ordinateurs hôtes pour qu’ils fassent partie d’une infrastructure protégée.** 

    Pour plus d’informations, consultez [infrastructure protégée](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md).
- **Sécuriser les appareils.** 

    Sécurisez les périphériques de stockage où vous conservez les fichiers de ressources des ordinateurs virtuels.
    
- **Sécurisez le disque dur.** 

    Utilisez Chiffrement de lecteur BitLocker pour protéger les ressources.
    
- **Sécurisez le système d’exploitation hôte Hyper-V.** 

    Utilisez les recommandations relatives aux paramètres de sécurité de base décrites dans la [ligne de base de sécurité de Windows Server](https://docs.microsoft.com/windows/device-security/windows-security-baselines).
    
- **Accordez les autorisations appropriées.**
    - Ajoutez des utilisateurs qui doivent gérer l’hôte Hyper-V au groupe administrateurs Hyper-V.
    - N’accordez pas d’autorisations d’administrateur de machine virtuelle sur le système d’exploitation hôte Hyper-V.

- **Configurer les exclusions et les options d’antivirus pour Hyper-V.**  

    Windows Defender a déjà configuré des [exclusions automatiques](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/configure-server-exclusions-windows-defender-antivirus) . Pour plus d’informations sur les exclusions, consultez [exclusions de virus recommandées pour les hôtes Hyper-V](https://support.microsoft.com/kb/3105657). 

- **Ne montez pas les disques durs virtuels inconnus.** Cela peut exposer l’hôte aux attaques au niveau du système de fichiers.

- **N’activez pas l’imbrication dans votre environnement de production, sauf si elle est requise.**

    Si vous activez l’imbrication, n’exécutez pas d’hyperviseurs non pris en charge sur un ordinateur virtuel.  

Pour les environnements plus sécurisés :

- **Utilisez le matériel avec une puce Module de plateforme sécurisée (TPM) (TPM) 2,0 pour configurer une infrastructure protégée.** 

    Pour plus d’informations, consultez [Configuration système requise pour Hyper-V sur Windows Server 2016](../system-requirements-for-hyper-v-on-windows.md).

## <a name="secure-virtual-machines"></a>Sécuriser les machines virtuelles
- **Créer des ordinateurs virtuels de 2e génération pour les systèmes d’exploitation invités pris en charge.** 

    Pour plus d’informations, consultez [paramètres de sécurité de génération 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md).
    
- **Activez le démarrage sécurisé.** 

    Pour plus d’informations, consultez [paramètres de sécurité de génération 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md).
    
- **Sécurisez le système d’exploitation invité.**

    - Installez les dernières mises à jour de sécurité avant d’activer une machine virtuelle dans un environnement de production.
    - Installez les services d’intégration pour les systèmes d’exploitation invités pris en charge qui en ont besoin et maintenez-les à jour. Les mises à jour du service d’intégration pour les invités qui exécutent des versions de Windows prises en charge sont disponibles via Windows Update.
    - Renforcez le système d’exploitation qui s’exécute sur chaque ordinateur virtuel en fonction du rôle qu’il exécute. Utilisez les recommandations relatives aux paramètres de sécurité de base qui sont décrites dans la [ligne de base de sécurité Windows](https://docs.microsoft.com/windows/device-security/windows-security-baselines).
    
- **Utilisez un réseau sécurisé.** 

    Assurez-vous que les cartes réseau virtuelles se connectent au commutateur virtuel approprié et que les limites et les paramètres de sécurité appropriés sont appliqués.
    
- **Stocker les disques durs virtuels et les fichiers d’instantanés dans un emplacement sécurisé.**

- **Sécuriser les appareils.** 

    Configurez uniquement les appareils requis pour un ordinateur virtuel. N’activez pas l’affectation discrète des appareils dans votre environnement de production, sauf si vous en avez besoin pour un scénario spécifique. Si vous l’activez, veillez à exposer uniquement les appareils des fournisseurs de confiance. 
    
- **Configurez le logiciel antivirus, le pare-feu et la détection d’intrusion** dans les machines virtuelles selon les besoins en fonction du rôle de machine virtuelle.

- **Activez la sécurité basée sur la virtualisation pour les invités qui exécutent Windows 10 ou Windows Server 2016 ou une version ultérieure.** 

    Pour plus d’informations, consultez le [Guide de déploiement de Device Guard](https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide).
    
- **Activez uniquement l’attribution d’appareils discrets si nécessaire pour une charge de travail spécifique**. 

    En raison de la nature du passage via un appareil physique, collaborez avec le fabricant de l’appareil pour savoir s’il doit être utilisé dans un environnement sécurisé.

Pour les environnements plus sécurisés :

- **Déployez des machines virtuelles avec protection activée et déployez-les sur une infrastructure protégée.** 

    Pour plus d’informations, consultez [paramètres de sécurité de génération 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md) et [infrastructure protégée](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md).
