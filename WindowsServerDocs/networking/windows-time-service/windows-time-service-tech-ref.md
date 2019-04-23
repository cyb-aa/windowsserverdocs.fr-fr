---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Référence technique du Service de temps Windows
description: Le service W32Time fournit la synchronisation d’horloge de réseau pour les ordinateurs sans nécessiter de configuration étendues. Le service W32Time est essentiel pour le bon fonctionnement de l’authentification Kerberos V5 et, par conséquent, pour l’authentification basée sur le service d’annuaire AD.
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 904a53797d22d2e06e0cd2f7572b99fc386dae16
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845120"
---
# <a name="windows-time-service-technical-reference"></a>Référence technique du Service de temps Windows
>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou version ultérieure

Le service W32Time fournit la synchronisation d’horloge de réseau pour les ordinateurs sans nécessiter de configuration étendues. Le service W32Time est essentiel pour le bon fonctionnement de l’authentification Kerberos V5 et, par conséquent, pour l’authentification basée sur le service d’annuaire AD. Toute application prenant en charge Kerberos, y compris la plupart des services de sécurité, s’appuie sur la synchronisation entre les ordinateurs qui participent à la demande d’authentification. Contrôleurs de domaine AD DS doivent aussi être synchronisées à horloges à contribuer à assurer la réplication des données précises.

> [!NOTE]  
> Dans Windows Server 2003 et Microsoft Windows 2000 Server, le service d’annuaire est nommé service d’annuaire Active Directory. Dans Windows Server 2008 R2 et Windows Server 2008, le service d’annuaire est nommé Services de domaine Active Directory (AD DS). Le reste de cette rubrique fait référence aux services AD DS, mais les informations sont également applicables aux Services de domaine Active Directory dans Windows Server 2016.

Le service W32Time est implémenté dans une bibliothèque de liens dynamiques appelée W32Time.dll, qui est installé par défaut dans **%Systemroot%\System32**. W32Time.dll a été développé pour Windows 2000 Server prendre en charge d’une spécification par le protocole d’authentification Kerberos V5 nécessitant des horloges sur un réseau à synchroniser. À compter de Windows Server 2003, W32Time.dll fourni une précision accrue de la synchronisation d’horloge de réseau sur le système d’exploitation Windows Server 2000. En outre, dans Windows Server 2003, W32Time.dll pris en charge une variété de périphériques matériels et les protocoles de temps réseau à l’aide de fournisseurs de temps.

Bien que conçu à l’origine pour assurer la synchronisation d’horloge pour l’authentification Kerberos, de nombreuses applications en cours utilisent les horodatages pour garantir la cohérence transactionnelle, d’enregistrer l’heure des événements importants et autres critiques, temps plus d’informations.  Ces applications bénéficient de la synchronisation de l’heure entre les ordinateurs qui sont fournies par le service de temps de Windows.

## <a name="importance-of-time-protocols"></a>Importance des protocoles de temps
Protocoles de temps la communication entre deux ordinateurs pour échanger des informations de temps, puis utiliser ces informations pour synchroniser leur horloge. Avec le protocole de temps de service de temps de Windows, un client demande des informations de temps à partir d’un serveur et synchronise son horloge en fonction des informations qui sont reçues.
  
Le service de temps de Windows utilise le protocole NTP afin de synchroniser l’heure sur un réseau. NTP est un protocole de temps Internet qui inclut les algorithmes de discipline nécessaires pour la synchronisation des horloges. NTP est un protocole de temps plus précis que le temps protocole SNTP (Simple Network) qui est utilisé dans certaines versions de Windows ; Toutefois, W32Time continue à prendre en charge SNTP pour permettre la compatibilité descendante avec les ordinateurs exécutant les services de temps basées sur SNTP tels que Windows 2000.
<!-- maybe this should be its own topic under the Tech Ref section -->
## <a name="where-to-find-windows-time-service-configuration-related-information"></a>Où trouver des informations de liée à la configuration de service de temps de Windows  
Ce guide est **pas** Décrivez la configuration du service de temps de Windows. Il existe plusieurs différentes rubriques sur Microsoft TechNet et dans la Base de connaissances Microsoft qui expliquent les procédures de configuration du service de temps de Windows. Si vous avez besoin des informations de configuration, les rubriques suivantes devraient vous aider à localiser les informations appropriées.  
<!-- should this be an if/then table -->
-   Pour configurer le service de temps de Windows pour l’émulateur de contrôleur de domaine principal racine forêt, voir :  
  
    -   [Configurer le service de temps de Windows sur l’émulateur de contrôleur de domaine principal dans le domaine racine de forêt](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29) 
  
    -   [Configuration d’une source de temps pour la forêt](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794823%28v%3dws.10%29) 
  
    -   L’article 816042 de la Base de connaissances Microsoft [comment configurer un serveur de temps faisant autorité dans Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402), qui décrit les paramètres de configuration pour les ordinateurs exécutant Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 et Windows Server 2003 R2.  
  
-   Pour configurer le service de temps de Windows sur n’importe quel client membre du domaine ou serveur ou même contrôleurs de domaine qui ne sont pas configurés en tant que l’émulateur de contrôleur de domaine principal de racine de forêt, consultez [configurer un ordinateur client pour la synchronisation date / heure automatique domaine](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
    > [!WARNING]  
    > Certaines applications peuvent nécessiter leur ordinateur pour que les services au moment de l’analyse de haute précision. Si tel est le cas, vous pouvez choisir de configurer une source de temps manuelle, mais n’oubliez pas que le service de temps de Windows n’est pas conçu pour fonctionner comme une source de temps très précis. Vérifiez que vous êtes conscient des limitations de prise en charge pour les environnements haute-précision time comme décrit dans le Base de connaissances Microsoft l’article 939322, [limites de prise en charge pour configurer le service de temps de Windows pour les environnements de haute-précision](support-boundary.md).  
  
-   Pour configurer le service de temps de Windows sur n’importe quel Windows client ou serveur ordinateurs qui sont configurés comme membres du groupe de travail au lieu de membres du domaine voient [configurer une source de temps manuelle pour un ordinateur client sélectionné](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816656%28v%3dws.10%29).  
  
-   Pour configurer le service de temps de Windows sur un ordinateur hôte qui exécute un environnement virtuel, consultez l’article 816042 de la Base de connaissances Microsoft [comment configurer un serveur de temps faisant autorité dans Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402). Si vous travaillez avec un produit de virtualisation non Microsoft, veillez à consulter la documentation du fournisseur de ce produit.  
  
-   Pour configurer le service de temps de Windows sur un contrôleur de domaine qui est en cours d’exécution sur une machine virtuelle, il est recommandé de désactiver partiellement la synchronisation horaire entre le système et invité système d’exploitation hôte agissant comme un contrôleur de domaine. Cela permet à votre contrôleur de domaine invité synchroniser l’heure pour la hiérarchie de domaine, mais il protège contre ayant un temps de décalage si elle est restaurée à partir d’un état enregistré. Pour plus d’informations, consultez l’article de la Base de connaissances Microsoft 976924, [vous recevez l’ID d’événement Service de temps Windows 24, 29 et 38 sur un contrôleur de domaine virtualisé qui s’exécute sur un serveur hôte Windows Server 2008 avec Hyper-V](https://go.microsoft.com/fwlink/?LinkID=192236) et [considérations relatives au déploiement de contrôleurs de domaine virtualisés](https://go.microsoft.com/fwlink/?LinkID=192235).  
  
-   Pour configurer le service de temps de Windows sur un contrôleur de domaine agissant en tant que l’émulateur de contrôleur de domaine principal de racine de forêt est également en cours d’exécution sur un ordinateur virtuel, suivez les mêmes instructions que pour un ordinateur physique, comme décrit dans [configurer le service de temps de Windows sur l’émulateur de contrôleur de domaine principal dans le domaine racine de forêt](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
-   Pour configurer le service de temps de Windows sur un serveur membre exécutant comme un ordinateur virtuel, utilisez la hiérarchie de temps du domaine comme décrit dans [configurer un ordinateur client pour la synchronisation date / heure automatique domaine](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).


> [!IMPORTANT]  
> Avant Windows Server 2016, le service W32Time n’est pas conçu pour répondre aux besoins de l’application de la contrainte de temps.  Toutefois, les mises à jour vers Windows Server 2016 maintenant vous permettent d’implémenter une solution de 1 MS précision dans votre domaine.  Pour plus d’informations, consultez [Windows 2016 précise temps](accurate-time.md) et [limite de prise en charge pour configurer le service de temps de Windows pour les environnements de haute-précision](support-boundary.md) pour plus d’informations.

## <a name="related-topics"></a>Rubriques connexes
- [Heure précise Windows 2016](accurate-time.md)
- [Améliorations de précision de temps pour Windows Server 2016](windows-server-2016-improvements.md)  
- [Fonctionnement du Service de temps Windows](How-the-Windows-Time-Service-Works.md)  
- [Paramètres et outils de Service de temps Windows](Windows-Time-Service-Tools-and-Settings.md)  
- [Limites de prise en charge pour configurer le service de temps de Windows pour les environnements de haute-précision](support-boundary.md)
- [Article 902229 de la Base de connaissances Microsoft](https://go.microsoft.com/fwlink/?LinkId=186066)