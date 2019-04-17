---
title: Installer Nano Server
description: Nouvelle installation, mise à niveau, migration et évaluation de Nano Server
ms.prod: windows-server-threshold
ms.service: na
manager: dougkim
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 2c2fa45b-6f3b-4663-b421-2da6ecc463bf
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 295402a3bcdcec07025ad1f803cddd47127baa8d
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133735"
---
# Installer Nano Server

>S’applique à Windows Server2016

> [!IMPORTANT]
> À compter de WindowsServer, version1709, NanoServer sera uniquement disponible sous forme d’[image du système d’exploitation de base du conteneur](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Consultez [Modifications apportées à NanoServer](nano-in-semi-annual-channel.md) pour en savoir plus. 

WindowsServer2016 propose une nouvelle option d’installation: NanoServer. Nano Server est un système d’exploitation de serveur administré à distance et optimisé pour les centres de données et clouds privés. Il est similaire à WindowsServer en mode ServerCore (Installation minimale), mais il est beaucoup plus petit, n’a aucune fonction d’ouverture de session locale et ne prend en charge que les agents, outils et applications 64bits. Il occupe beaucoup moins d’espace disque, installe beaucoup plus rapidement et nécessite beaucoup moins de mises à jour et de redémarrages que WindowsServer. Lorsqu’il redémarre, il le fait beaucoup plus rapidement. L’option d’installation Nano Server est disponible dans les éditions Standard et Datacenter de WindowsServer2016.  

Nano Server est idéal dans différentes situations:  
  
-   Comme un hôte de «calcul» pour les machines virtuelles Hyper-V, dans des clusters ou non  
  
-   Comme un hôte de stockage pour un serveur de fichiers avec montée en puissance parallèle  
  
-   Comme un serveurDNS  
  
-   Comme un serveurweb exécutant Internet Information Services (IIS)  
  
-   Comme un hôte pour les applications développées à l’aide de modèles d’application cloud et exécutées dans le système d’exploitation invité d’un conteneur ou d’une machine virtuelle  
  
## Différences importantes dans Nano Server

Étant donné que Nano Server est optimisé comme un système d’exploitation léger pour exécuter des applications «cloud natives» basées sur des conteneurs et micro-services ou comme un hôte de centre de données agile et rentable avec un encombrement considérablement réduit, il existe des différences importantes dans Nano Server entre les options d’installation minimale et Serveur avec Expérience de bureau:

- Nano Server est «sans périphérique de contrôle»; il n’existe aucune fonctionnalité d’ouverture de session locale ni d’interface utilisateur graphique.
- Seuls les agents, outils et applications 64bits sont pris en charge.
- Nano Server ne peut pas servir de contrôleur de domaine Active Directory.
- Aucune stratégie de groupe n’est prise en charge. Toutefois, vous pouvez utiliser la [configuration d’état souhaité](https://msdn.microsoft.com/powershell/dsc/nanoDsc) pour appliquer les paramètres à l’échelle.
- Nano Server ne peut pas être configuré pour utiliser un serveur proxy pour accéder à Internet.
- L’association de cartes réseau (plus précisément, l’équilibrage de charge et le basculement ou LBFO) n’est pas pris en charge, contrairement à SET (Switch Embedded Teaming).
- System Center Configuration Manager et System Center Data Protection Manager ne sont pas pris en charge.
- Les applets de commande BPA (Best Practices Analyzer) et l’intégration de BPA avec le Gestionnaire de serveur ne sont pas prises en charge.
- NANO Server ne prend pas en charge les adaptateurs de bus hôte virtuels (HBA).
- NANOServer n’a pas besoin d’être activé avec une clé de produit. Lorsqu’il fonctionne comme une hôte Hyper-V, NanoServer ne prend pas en charge l’[Activation automatique d’ordinateur virtuel](https://technet.microsoft.com/library/dn303421%28v=ws.11%29.aspx) (AVMA). Les ordinateurs virtuels qui s’exécutent sur un hôte NanoServer peuvent être activés à l’aide du [Service de gestion de clés](https://technet.microsoft.com/library/jj612867(v=ws.11).aspx) (KMS), avec une clé de licence en volume générique, ou au moyen d’une [activation basée sur ActiveDirectory](https://technet.microsoft.com/library/dn502534(v=ws.11).aspx).
- La version de WindowsPowerShell fournie avec NanoServer présente des différences importantes. Pour plus d’informations, voir [PowerShell sur Nano Server](PowerShell-on-Nano-Server.md).
- Nano Server est pris en charge uniquement sur le modèle de branche CBB (Current Branch for Business): il n’existe aucune version de branche LTSB (Long Term Servicing Branch) pour Nano Server à ce stade. Pour plus d’informations, voir la sous-section suivante.

### Branche CBB
Nano Server est mis en service avec un modèle plus actif, appelé branche CBB, pour prendre en charge les clients qui adoptent une «cadence de cloud», avec des cycles de développement rapides. Dans ce modèle, les publications des mises à jour des fonctionnalités de Nano Server sont prévues deux à trois fois par an. Ce modèle nécessite la [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx) pour les serveurs Nano Server déployés et utilisés en production. Pour assurer la prise en charge, les administrateurs ne doivent pas utiliser une version CBB antérieure aux deux dernières publications. Toutefois, ces publications ne mettent pas automatiquement à jour les déploiements existants; les administrateurs effectuent une installation manuelle d’une nouvelle version CBB à leur convenance. Pour obtenir des informations supplémentaires, voir [WindowsServer2016 new Current Branch for Business servicing option](https://blogs.technet.microsoft.com/windowsserver/2016/07/12/windows-server-2016-new-current-branch-for-business-servicing-option/).

Les options d’installation minimale et Serveur avec Expérience de bureau sont toujours mises en service sur le [modèle LTSB (Long Term Servicing Branch)](https://support.microsoft.com/lifecycle#gp%2Fgp_msl_policy), avec 5ans de support standard et 5ans de support étendu.

## Scénarios d’installation

### Évaluation
Vous pouvez vous procurer une version d’évaluation de WindowsServer, avec une licence de 180jours, sur la page [WindowsServer - Évaluations](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016). Pour essayer Nano Server, choisissez **Nano Server |64-bit EXE option (Nano Server |64-bit EXE option)**, puis revenez à la rubrique [Nano Server Quick Start (Démarrage rapide de Nano Server)](Nano-Server-Quick-Start.md) ou [Deploy Nano Server (Déployer Nano Server)](Deploy-Nano-Server.md) pour commencer.

### Nouvelle installation
Comme vous installez Nano Server en configurant un disque dur virtuel, le mode de déploiement le plus rapide et le plus simple est une nouvelle installation.

- Pour effectuer rapidement un déploiement de base de Nano Server, en utilisant DHCP pour obtenir une adresseIP, consultez [Démarrage rapide de Nano Server](Nano-Server-Quick-Start.md). 
- Si vous êtes déjà familier avec les principes fondamentaux de Nano Server, les rubriques plus détaillées, en commençant par [Deploy Nano Server (Déployer Nano Server)](Deploy-Nano-Server.md), offrent un jeu complet d’instructions pour la personnalisation d’images, l’utilisation des domaines, l’installation de packages pour les rôles de serveur, d’autres fonctionnalités en ligne et hors connexion, et bien plus encore.

> [!IMPORTANT]  
> Une fois que l’exécution du programme d’installation est terminée et que vous avez installé tous les rôles et fonctionnalités de serveur dont vous avez besoin, recherchez et installez les mises à jour disponibles pour WindowsServer2016. Pour Nano Server, consultez la section «Gestion des mises à jour de Nano Server» de [Gérer Nano Server](Manage-Nano-Server.md).

### Mettre à niveau/Mise à niveau
Comme Nano Server est une nouveauté de WindowsServer2016, il n’existe aucune mise à niveau possible entre les anciennes versions de système d’exploitation et Nano Server.

### Migration
Comme Nano Server est une nouveauté de WindowsServer2016, il n’existe aucune migration possible entre les anciennes versions de système d’exploitation et Nano Server.
  
-------------------------------------
Si vous avez besoin d’une autre option d’installation, revenez à la [page principale de Windows Server2016](windows-server-2016.md). 

  


 
