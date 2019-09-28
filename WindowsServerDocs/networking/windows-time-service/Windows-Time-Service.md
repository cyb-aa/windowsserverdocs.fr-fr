---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Référence technique du service de temps Windows
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 31c7c53a5dd28813076fcaa745093050808b5755
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405227"
---
# <a name="windows-time-service"></a>Service de temps Windows

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou version ultérieure


**Dans ce guide**  
  
* Où trouver les informations de configuration du service de temps Windows  
* Qu’est-ce que le service de temps Windows ?  
* Importance des protocoles de temps  
* Fonctionnement du service de temps Windows   
* Paramètres et outils du service de temps Windows  
  
> [!NOTE]  
> Dans Windows Server 2003 et Microsoft Windows 2000 Server, le service d’annuaire est nommé Active Directory Service d’annuaire. Dans Windows Server 2008 R2 et Windows Server 2008, le service d’annuaire est nommé Active Directory Domain Services (AD DS). Le reste de cette rubrique fait référence à AD DS, mais les informations s’appliquent également aux Active Directory Domain Services dans Windows Server 2016.  
  
Le service de temps Windows, également appelé W32Time, synchronise la date et l’heure de tous les ordinateurs s’exécutant dans un domaine AD DS. La synchronisation de l’heure est essentielle au bon fonctionnement de nombreux services Windows et applications métier. Le service de temps Windows utilise le protocole NTP (Network Time Protocol) pour synchroniser les horloges des ordinateurs sur le réseau afin qu’une valeur d’horloge exacte, ou un horodatage, puisse être affectée à la validation réseau et aux demandes d’accès aux ressources. Le service intègre NTP et les fournisseurs de temps, ce qui en fait un service de temps fiable et évolutif pour les administrateurs d’entreprise.
  
> [!IMPORTANT]  
> Avant Windows Server 2016, le service W32Time n’a pas été conçu pour répondre aux besoins des applications qui respectent le temps.  Toutefois, les mises à jour de Windows Server 2016 vous permettent désormais d’implémenter une solution pour une précision de 1 ms dans votre domaine.  Pour plus d’informations, consultez limite du temps et de l’assistance de [windows 2016](accurate-time.md) [pour configurer le service de temps Windows pour les environnements à haute précision](support-boundary.md) .  
  
## <a name="BKMK_Config"></a>Où trouver les informations de configuration du service de temps Windows  
Ce guide ne traite **pas** de la configuration du service de temps Windows. Il existe plusieurs rubriques sur Microsoft TechNet et dans la base de connaissances Microsoft qui décrivent les procédures de configuration du service de temps Windows. Si vous avez besoin d’informations de configuration, les rubriques suivantes doivent vous aider à localiser les informations appropriées.  
  
-   Pour configurer le service de temps Windows pour l’émulateur de contrôleur de domaine principal (PDC) racine de la forêt, consultez :  
  
    -   [Configurer le service de temps Windows sur l’émulateur de contrôleur de domaine principal dans le domaine racine de forêt](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29) 
  
    -   [Configuration d’une source de temps pour la forêt](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794823%28v%3dws.10%29) 
  
    -   Article 816042 de la base de connaissances Microsoft, [Comment configurer un serveur de temps faisant autorité dans Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402), qui décrit les paramètres de configuration pour les ordinateurs exécutant windows Server 2008 R2, windows Server 2008, windows Server 2003 et Windows Serveur 2003 R2.  
  
-   Pour configurer le service de temps Windows sur un client ou un serveur membre du domaine, ou même sur des contrôleurs de domaine qui ne sont pas configurés en tant qu’émulateur PDC racine de la forêt, consultez [configurer un ordinateur client pour la synchronisation automatique de l’heure du domaine](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
    > [!WARNING]  
    > Certaines applications peuvent nécessiter que leurs ordinateurs disposent de services de temps de grande précision. Si tel est le cas, vous pouvez choisir de configurer une source de temps manuelle, mais sachez que le service de temps Windows n’a pas été conçu pour fonctionner comme une source de temps très précise. Assurez-vous que vous avez pris connaissance des limitations de prise en charge pour les environnements à durée de précision élevée, comme décrit dans l’article 939322 de la base de connaissances Microsoft, [limite de support pour configurer le service de temps Windows pour les environnements à haute précision](support-boundary.md).  
  
-   Pour configurer le service de temps Windows sur tous les ordinateurs clients ou serveurs Windows configurés en tant que membres du groupe de travail au lieu des membres du domaine, consultez [configurer une source de temps manuelle pour un ordinateur client sélectionné](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816656%28v%3dws.10%29).  
  
-   Pour configurer le service de temps Windows sur un ordinateur hôte qui exécute un environnement virtuel, consultez l’article 816042 de la base de connaissances Microsoft, [Comment configurer un serveur de temps faisant autorité dans Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402). Si vous utilisez un produit de virtualisation non-Microsoft, veillez à consulter la documentation du fournisseur de ce produit.  
  
-   Pour configurer le service de temps Windows sur un contrôleur de domaine qui s’exécute sur un ordinateur virtuel, il est recommandé de désactiver partiellement la synchronisation de l’heure entre le système hôte et le système d’exploitation invité jouant le rôle de contrôleur de domaine. Cela permet à votre contrôleur de domaine invité de synchroniser l’heure de la hiérarchie de domaine, mais le protège de l’existence d’un décalage horaire s’il est restauré à partir d’un état enregistré. Pour plus d’informations, consultez l’article 976924 de la base de connaissances Microsoft, [qui reçoit les ID d’événements du service de temps Windows 24, 29 et 38 sur un contrôleur de domaine virtualisé qui s’exécute sur un serveur hôte Windows server 2008 avec Hyper-V et le](https://go.microsoft.com/fwlink/?LinkID=192236) [déploiement Éléments à prendre en considération pour les contrôleurs de domaine virtualisés](https://go.microsoft.com/fwlink/?LinkID=192235).  
  
-   Pour configurer le service de temps Windows sur un contrôleur de domaine jouant le rôle d’émulateur de contrôleur de domaine principal racine de la forêt qui s’exécute également sur un ordinateur virtuel, suivez les mêmes instructions pour un ordinateur physique, comme décrit dans [configurer le service de temps Windows sur le contrôleur de domaine principal. émulateur dans le domaine racine de la forêt](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
-   Pour configurer le service de temps Windows sur un serveur membre qui exécute en tant qu’ordinateur virtuel, utilisez la hiérarchie de temps de domaine comme décrit dans ([configurer un ordinateur client pour la synchronisation automatique de l’heure du domaine](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
## <a name="BKMK_WTS"></a>Qu’est-ce que le service de temps Windows ?  
Le service de temps Windows (W32Time) assure la synchronisation de l’horloge réseau pour les ordinateurs sans nécessiter de configuration étendue.  
  
Le service de temps Windows est essentiel au bon fonctionnement de l’authentification Kerberos version 5 et, par conséquent, à l’authentification AD DS. Toute application prenant en charge Kerberos, y compris la plupart des services de sécurité, s’appuie sur la synchronisation de l’heure entre les ordinateurs qui participent à la demande d’authentification. AD DS contrôleurs de domaine doivent également avoir des horloges synchronisées pour garantir une réplication des données précise.  
  
Le service de temps Windows est implémenté dans une bibliothèque de liens dynamiques appelée W32Time. dll. W32Time. dll est installé par défaut dans le dossier **%systemroot%\System32** lors de la configuration et de l’installation du système d’exploitation.  
  
W32Time. dll a été développé à l’origine pour Windows 2000 Server afin de prendre en charge une spécification par le protocole d’authentification Kerberos V5 qui nécessitait des horloges sur un réseau à synchroniser. À compter de Windows Server 2003, W32Time. dll offrait une plus grande précision dans la synchronisation des horloges réseau sur le système d’exploitation du serveur Windows 2000 et, en outre, prenait en charge un large éventail de périphériques matériels et de protocoles de temps réseau par le biais du temps. éditeurs. Bien que conçu à l’origine pour assurer la synchronisation de l’horloge pour l’authentification Kerberos, de nombreuses applications utilisent des horodateurs pour garantir la cohérence transactionnelle, enregistrer l’heure des événements importants et d’autres tâches critiques, sensibles au temps informations. Ces applications tirent parti de la synchronisation de l’heure entre les ordinateurs fournis par le service de temps Windows.  
  
## <a name="BKMK_TimeProtocols"></a>Importance des protocoles de temps  
Les protocoles de temps communiquent entre deux ordinateurs pour échanger des informations d’heure, puis utiliser ces informations pour synchroniser leurs horloges. Avec le protocole de temps du service de temps Windows, un client demande des informations de temps à un serveur et synchronise son horloge en fonction des informations reçues.  
  
Le service de temps Windows utilise NTP pour synchroniser l’heure sur un réseau. NTP est un protocole Internet qui comprend les algorithmes de discipline nécessaires à la synchronisation des horloges. NTP est un protocole de temps plus précis que le protocole SNTP (simple Network Time Protocol) utilisé dans certaines versions de Windows. Toutefois, W32Time continue à prendre en charge SNTP pour activer la compatibilité descendante avec les ordinateurs qui exécutent des services de temps basés sur SNTP tels que Windows 2000.  
  
## <a name="see-also"></a>Voir aussi  
[Fonctionnement du service de temps Windows](How-the-Windows-Time-Service-Works.md)  
[Paramètres et outils du service de temps Windows](Windows-Time-Service-Tools-and-Settings.md)  
[Article 902229 de la base de connaissances Microsoft](https://go.microsoft.com/fwlink/?LinkId=186066)