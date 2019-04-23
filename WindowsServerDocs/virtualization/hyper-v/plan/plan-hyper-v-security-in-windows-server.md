---
title: Planifier la sécurité Hyper-V dans Windows Server
description: Fournit une liste des considérations de sécurité pour les hôtes Hyper-v et les machines virtuelles
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 462124e1907ef03aa7746dd050be3e8c84c7abda
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877410"
---
# <a name="plan-for-hyper-v-security-in-windows-server"></a>Planifier la sécurité Hyper-V dans Windows Server

>S'applique à : Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Sécuriser le système d’exploitation de hôte Hyper-V, les machines virtuelles, les fichiers de configuration et les données de la machine virtuelle. Utilisez la liste suivante des pratiques recommandées en tant qu’une liste de vérification pour vous aider à sécuriser votre environnement Hyper-V.

## <a name="secure-the-hyper-v-host"></a>Sécuriser l’hôte Hyper-V
- **Gardez à l’hôte de système d’exploitation sécurisé.**
    - Réduire la surface d’attaque à l’aide de l’option d’installation de Windows Server minimale dont vous avez besoin pour le système d’exploitation de gestion. Pour plus d’informations, consultez le [section des Options d’Installation](/windows-server/windows-server#installation-options) de la bibliothèque de contenu technique de Windows Server. Nous ne recommandons pas que vous exécutez des charges de travail de production sur Hyper-V sur Windows 10.
    - Système de d’exploitation hôte Hyper-V, des microprogrammes et des pilotes de périphérique mise à jour avec les dernières mises à jour de sécurité. Vérifiez les recommandations du fabricant de votre mise à jour du microprogramme et les pilotes.
    - Ne pas utiliser l’hôte Hyper-V comme station de travail ou installer tout logiciel inutile.
    - Gérer à distance l’ordinateur hôte Hyper-V. Si vous devez gérer l’hôte Hyper-V localement, utilisez Credential Guard. Pour plus d’informations, consultez la section [Protéger les informations d’identification de domaine dérivées avec Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard).
    - Activer les stratégies d’intégrité du code. Utiliser la sécurité basée sur la virtualisation protégées des services de l’intégrité du Code. Pour plus d’informations, consultez [Guide de déploiement de Device Guard](https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide).
- **Utiliser un réseau sécurisé.**
    - Utiliser un réseau distinct avec une carte réseau dédiée pour l’ordinateur Hyper-V physique.
    - Utiliser un réseau privé ou sécurisé pour les configurations de machines virtuelles d’accès et les fichiers de disque dur virtuel.
    - Utilisez un réseau privé/dédié pour le trafic de migration en direct. Envisagez d’activer IPSec sur ce réseau à utiliser le chiffrement et de sécuriser les données de votre machine virtuelle va sur le réseau pendant la migration. Pour plus d’informations, consultez [configurer des hôtes pour la migration dynamique sans Clustering de basculement](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md).
- **Sécuriser le trafic de migration de stockage.** 

    Utiliser SMB 3.0 pour le chiffrement de bout en bout des données SMB et protection des données falsification ou d’écoute clandestine sur des réseaux non approuvés. Pour accéder au contenu du partage SMB pour empêcher les attaques man-in-the-middle, utilisez un réseau privé. Pour plus d’informations, consultez [les améliorations de sécurité SMB](https://technet.microsoft.com/library/dn551363.aspx). 
- **Configurez les hôtes pour faire partie d’une infrastructure protégée.** 

    Pour plus d’informations, consultez [service Guardian fabric](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md).
- **Sécuriser les appareils.** 

    Sécuriser les périphériques de stockage dans lequel se trouvent les fichiers de ressources de machine virtuelle.
    
- **Sécuriser le disque dur.** 

    Utilisez le chiffrement de lecteur BitLocker pour protéger les ressources.
    
- **Renforcer le système d’exploitation de hôte Hyper-V.** 

    Utilisez les recommandations de paramètre de sécurité de base décrites dans le [base de sécurité de Windows Server](https://docs.microsoft.com/windows/device-security/windows-security-baselines).
    
- **Accorder les autorisations appropriées.**
    - Ajouter des utilisateurs qui ont besoin de gérer l’hôte Hyper-V au groupe Administrateurs Hyper-V.
    - N’accordez pas d’administrateurs d’ordinateurs virtuels Hyper-V, les autorisations de système d’exploitation hôte.

- **Configurer des exclusions d’antivirus et les options de Hyper-V.**  

    Windows Defender a déjà [exclusions automatiques](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/configure-server-exclusions-windows-defender-antivirus) configuré. Pour plus d’informations sur les exclusions, consultez [recommandé des exclusions d’antivirus pour les ordinateurs hôtes Hyper-V](https://support.microsoft.com/kb/3105657). 

- **Ne montez des disques durs virtuels inconnus.** Cela peut exposer l’hôte pour les attaques au niveau de système de fichiers.

- **N’activez pas d’imbrication dans votre environnement de production, sauf si ce champ est obligatoire.**

    Si vous activez l’imbrication, n’exécutez pas hyperviseurs non pris en charge sur une machine virtuelle.  

Pour les environnements plus sécurisés :

- **Utiliser le matériel avec une puce Trusted Platform Module (TPM) 2.0 pour configurer une infrastructure protégée.** 

    Pour plus d’informations, consultez [configuration système requise pour Hyper-V sur Windows Server 2016](../system-requirements-for-hyper-v-on-windows.md).

## <a name="secure-virtual-machines"></a>Sécuriser les machines virtuelles
- **Créez de génération 2 machines virtuelles pour les systèmes d’exploitation invités pris en charge.** 

    Pour plus d’informations, consultez [les paramètres de sécurité de génération 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md).
    
- **Activer le démarrage sécurisé.** 

    Pour plus d’informations, consultez [les paramètres de sécurité de génération 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md).
    
- **Gardez à l’invité sécurisé du système d’exploitation.**

    - Installez les dernières mises à jour de sécurité avant d’activer une machine virtuelle dans un environnement de production.
    - Installer les services d’intégration pour les systèmes d’exploitation invités pris en charge que vous avez besoin et le maintenir à jour. Mises à jour du service de l’intégration des invités qui exécutent des versions Windows prises en charge sont disponibles via Windows Update.
    - Renforcer le système d’exploitation qui s’exécute sur chaque machine virtuelle en fonction du rôle qu’il effectue. Utilisez les recommandations de paramètre de sécurité de base qui sont décrites dans le [Windows Security Baseline](https://docs.microsoft.com/windows/device-security/windows-security-baselines).
    
- **Utiliser un réseau sécurisé.** 

    Assurez-vous que les cartes réseau virtuelles se connectent au commutateur virtuel correct et ont le paramètre de sécurité approprié et les limites appliqués.
    
- **Store les disques durs virtuels et les fichiers d’instantanés dans un emplacement sécurisé.**

- **Sécuriser les appareils.** 

    Configurer uniquement les appareils requis pour une machine virtuelle. N’activez pas attribution discrète d’appareils dans votre environnement de production, sauf si vous en avez besoin pour un scénario spécifique. Si vous l’activez, veillez à exposer uniquement les appareils à partir des fournisseurs approuvés. 
    
- **Configurer un antivirus, pare-feu et logiciels de détection d’intrusion** au sein de machines virtuelles comme il convient en fonction du rôle de machine virtuelle.

- **Activer la sécurité de la virtualisation en fonction des invités qui exécutent Windows 10 ou Windows Server 2016 ou version ultérieure.** 

    Pour plus d’informations, consultez le [Guide de déploiement de Device Guard](https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide).
    
- **Activer uniquement l’attribution de périphérique discrètes si nécessaire pour une charge de travail spécifique**. 

    En raison de la nature de passer par un appareil physique, fonctionnent avec le fabricant du périphérique pour comprendre si il doit être utilisé dans un environnement sécurisé.

Pour les environnements plus sécurisés :

- **Déployer des machines virtuelles avec la protection activée et les déployer sur une infrastructure protégée.** 

    Pour plus d’informations, consultez [les paramètres de sécurité de génération 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md) et [service Guardian fabric](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md).
