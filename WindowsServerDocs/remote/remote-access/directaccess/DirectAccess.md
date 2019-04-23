---
title: DirectAccess
description: Vous pouvez utiliser cette rubrique pour une vue d’ensemble de DirectAccess dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6b71d18e-1939-4fc0-bb42-29e0e5ffc8da
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 608d6b4dd3d5e894b28e767164b9370de9cb59ec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869570"
---
# <a name="directaccess"></a>DirectAccess

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour une vue d’ensemble de DirectAccess, y compris le serveur et les systèmes d’exploitation clients qui prennent en charge de DirectAccess, et pour obtenir des liens vers la documentation supplémentaire de DirectAccess pour Windows Server 2016.  
  
> [!NOTE]  
> Outre cette rubrique, la documentation de DirectAccess suivante est disponible.  
>   
> -   [Chemins de déploiement de DirectAccess dans Windows Server](DirectAccess-Deployment-Paths-in-Windows-Server.md)  
> -   [Conditions préalables au déploiement DirectAccess](Prerequisites-for-Deploying-DirectAccess.md)  
> -   [DirectAccess les Configurations non prises en charge](DirectAccess-Unsupported-Configurations.md)  
> -   [Guides de laboratoire de Test de DirectAccess](DirectAccess-Test-Lab-Guides.md)  
> -   [Problèmes connus de DirectAccess](DirectAccess-Known-Issues.md)  
> -   [Planification de la capacité DirectAccess](DirectAccess-Capacity-Planning.md) 
> -   [Jonction de domaine hors connexion DirectAccess](DirectAccess-Offline-Domain-Join.md)  
> -   [Dépannage de DirectAccess](Troubleshooting-DirectAccess.md)  
> -   [Déployer un serveur DirectAccess unique à l’aide de l’Assistant Mise en route](single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)  
> -   [Déployer un serveur DirectAccess unique avec des paramètres avancés](single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)  
> -   [Ajouter DirectAccess à un déploiement de l’accès à distance (VPN) existant](add-to-existing-vpn/Add-DirectAccess-to-an-Existing-Remote-Access-VPN-Deployment.md)  
  
DirectAccess permet une connectivité pour les utilisateurs distants aux ressources de réseau d’entreprise sans nécessiter de connexions de réseau privé virtuel (VPN) traditionnelles. Avec les connexions DirectAccess, les ordinateurs clients distants sont toujours connectées à votre organisation : il n’est pas nécessaire pour les utilisateurs distants démarrer et arrêter les connexions, en l’état requis avec les connexions VPN. En outre, vos administrateurs informatiques peuvent gérer les ordinateurs clients DirectAccess chaque fois qu’ils sont en cours d’exécution et connecté à Internet.

>[!IMPORTANT]
>N’essayez pas de déployer l’accès à distance sur une machine virtuelle \(machine virtuelle\) dans Microsoft Azure. L’accès à distance dans Microsoft Azure n’est pas pris en charge. Vous ne pouvez pas utiliser l’accès à distance dans une machine virtuelle Azure pour déployer le VPN, DirectAccess ou toute autre fonctionnalité d’accès à distance dans Windows Server 2016 ou versions antérieures de Windows Server. Pour plus d’informations, consultez [prise en charge du logiciel Microsoft server pour machines virtuelles Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).
  
DirectAccess prend en charge uniquement pour les clients joints au domaine qui incluent la prise en charge du système d’exploitation pour DirectAccess.  
  
Les systèmes d’exploitation de serveur suivants prennent en charge DirectAccess.  
  
-   Vous pouvez déployer toutes les versions de Windows Server 2016 comme un client DirectAccess ou un serveur DirectAccess.  
  
-   Vous pouvez déployer toutes les versions de Windows Server 2012 R2 en tant qu’un client DirectAccess ou un serveur DirectAccess.  
  
-   Vous pouvez déployer toutes les versions de Windows Server 2012 en tant qu’un client DirectAccess ou un serveur DirectAccess.  
  
-   Vous pouvez déployer toutes les versions de Windows Server 2008 R2 en tant qu’un client DirectAccess ou un serveur DirectAccess.  
  
Les systèmes d’exploitation de client suivants prennent en charge DirectAccess.  
  
-   Windows 10 Entreprise  
  
-   Windows 10 entreprise 2015 Long terme Servicing Branch (LTSB)  
  
-   Windows 8 et 8.1 Enterprise  
  
-   Windows 7 Édition Intégrale  
  
-   Windows 7 Entreprise
