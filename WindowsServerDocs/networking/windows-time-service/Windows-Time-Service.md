---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Informations techniques de référence sur le service de temps Windows
author: dcuomo
ms.author: dacuo
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 51711d423582aee4ebc3762a51abe9156754b55a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860132"
---
# <a name="windows-time-service"></a>Service de temps Windows

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou version ultérieure


**Contenu de ce guide**  
  
* Sources d’information traitant de la configuration du service de temps Windows  
* Qu’est-ce que le service de temps Windows ?  
* Importance des protocoles de temps  
* Fonctionnement du service de temps Windows   
* Paramètres et outils du service de temps Windows  
  
> [!NOTE]  
> Dans Windows Server 2003 et Microsoft Windows 2000 Server, le service d’annuaire se nomme service d’annuaire Active Directory. Dans Windows Server 2008 R2 et Windows Server 2008, le service d’annuaire se nomme Active Directory Domain Services (AD DS). Le reste de cette rubrique fait référence aux services AD DS, mais les informations sont également applicables à Active Directory Domain Services dans Windows Server 2016.  
  
Le service de temps Windows, également appelé W32Time, synchronise la date et l’heure de tous les ordinateurs s’exécutant dans un domaine AD DS. La synchronisation de l’heure est essentielle au bon fonctionnement de nombreux services Windows et applications métier. Le service de temps Windows utilise le protocole NTP (Network Time Protocol) pour synchroniser les horloges des ordinateurs du réseau de sorte qu’une valeur d’horloge exacte, ou horodatage, puisse être affectée aux demandes de validation réseau et aux demandes d’accès aux ressources. Le service intègre NTP et des fournisseurs de temps, ce qui en fait un service de temps fiable et scalable pour les administrateurs d’entreprise.
  
> [!IMPORTANT]  
> Avant Windows Server 2016, le service W32Time n’était pas conçu pour répondre aux besoins des applications sensibles au facteur temps.  Cependant, les mises à jour apportées à Windows Server 2016 vous permettent désormais d’implémenter une solution pour une précision de 1 ms dans votre domaine.  Pour plus d’informations, consultez [Précision de l’heure dans Windows Server 2016](accurate-time.md) et [Limites de prise en charge pour configurer le service de temps Windows pour les environnements de haute précision](support-boundary.md).  
  
## <a name="where-to-find-windows-time-service-configuration-information"></a><a name="BKMK_Config"></a>Sources d’information traitant de la configuration du service de temps Windows  
Ce guide n’**aborde pas** la question de la configuration du service de temps Windows. Il existe sur Microsoft TechNet et dans la Base de connaissances Microsoft plusieurs rubriques qui décrivent les procédures de configuration du service de temps Windows. Si vous avez besoin d’informations sur la configuration, les rubriques suivantes devraient vous aider à trouver les informations nécessaires.  
  
-   Pour configurer le service de temps Windows pour l’émulateur de contrôleur de domaine principal (PDC) racine de forêt, consultez :  
  
    -   [Configurer le service de temps Windows sur l’émulateur PDC du domaine racine de forêt](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29) 
  
    -   [Configuration d’une source de temps pour la forêt](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794823%28v%3dws.10%29) 
  
    -   L’article 816042 de la Base de connaissances Microsoft, [Configuration d’un serveur de temps de référence dans Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402), qui décrit les paramètres de configuration pour les ordinateurs exécutant Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 et Windows Server 2003 R2.  
  
-   Pour configurer le service de temps Windows sur un client ou un serveur membre de domaine, ou même sur des contrôleurs de domaine qui ne sont pas configurés en tant qu’émulateur PDC de racine de forêt, consultez [Configurer un ordinateur client pour la synchronisation automatique de l’heure du domaine](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
    > [!WARNING]  
    > Certaines applications peuvent exiger l’utilisation de services de temps de grande précision sur l’ordinateur. Dans ce cas, vous pouvez choisir de configurer une source de temps manuelle, tout en sachant que le service de temps Windows n’a pas été conçu pour fonctionner comme une source de temps hautement précise. Veillez à prendre connaissance des limitations de prise en charge pour les environnements exigeant une grande précision d’heure, comme décrit dans l’article 939322 de la Base de connaissances Microsoft, [Limite de prise en charge pour configurer le service de temps Windows pour les environnements à haute précision](support-boundary.md).  
  
-   Pour configurer le service de temps Windows sur des ordinateurs clients ou serveurs Windows configurés comme membres de groupe de travail et non comme membres de domaine, consultez [Configurer une source de temps manuelle pour un ordinateur client sélectionné](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816656%28v%3dws.10%29).  
  
-   Pour configurer le service de temps Windows sur un ordinateur hôte qui exécute un environnement virtuel, consultez l’article 816042 de la Base de connaissances Microsoft, [Configuration d’un serveur de temps de référence dans Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402). Si vous utilisez un produit de virtualisation non-Microsoft, veillez à consulter la documentation de l’éditeur de ce produit.  
  
-   Pour configurer le service de temps Windows sur un contrôleur de domaine qui s’exécute dans une machine virtuelle, il est recommandé de désactiver partiellement la synchronisation de l’heure entre le système hôte et le système d’exploitation invité jouant le rôle de contrôleur de domaine. Cela permet à votre contrôleur de domaine invité de synchroniser l’heure pour la hiérarchie de domaines, mais cela lui évite de subir un décalage horaire s’il est restauré à partir d’un état enregistré. Pour plus d’informations, consultez l’article 976924 de la Base de connaissances Microsoft, [Vous recevez les ID d’événement 24, 29 et 38 du service de temps Windows sur un contrôleur de domaine virtualisé qui s’exécute sur un serveur hôte basé sur Windows Server 2008 avec Hyper-V](https://go.microsoft.com/fwlink/?LinkID=192236) et [Considérations sur le déploiement pour les contrôleurs de domaine virtualisés](https://go.microsoft.com/fwlink/?LinkID=192235).  
  
-   Pour configurer le service de temps Windows sur un contrôleur de domaine jouant le rôle d’émulateur PDC de racine de forêt qui s’exécute également dans un ordinateur virtuel, suivez les mêmes instructions que pour un ordinateur physique décrites dans [Configurer le service de temps Windows sur l’émulateur PDC du domaine racine de forêt](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
-   Pour configurer le service de temps Windows sur un serveur membre qui s’exécute en tant qu’ordinateur virtuel, utilisez la hiérarchie temporelle du domaine comme décrit dans [Configurer un ordinateur client pour la synchronisation automatique de l’heure du domaine](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
## <a name="what-is-the-windows-time-service"></a><a name="BKMK_WTS"></a>Qu’est-ce que le service de temps Windows ?  
Le service de temps Windows (W32Time) assure la synchronisation de l’horloge réseau pour les ordinateurs sans nécessiter de configuration étendue.  
  
Le service le temps Windows est essentiel au bon fonctionnement de l’authentification Kerberos version 5 et donc de l’authentification AD DS. Toute application prenant en charge Kerberos, ce qui inclut la plupart des services de sécurité, repose sur la synchronisation de l’heure entre les ordinateurs qui participent à la demande d’authentification. Les horloges des contrôleurs de domaine AD DS doivent aussi être synchronisées pour garantir la réplication correcte des données.  
  
Le service de temps Windows est implémenté dans une bibliothèque de liens dynamiques appelée W32Time.dll. W32Time.dll est installé par défaut dans le dossier **%Systemroot%\System32** pendant la configuration et l’installation du système d’exploitation.  
  
W32Time.dll a été développé à l’origine pour Windows 2000 Server de façon à prendre en charge une spécification du protocole d’authentification Kerberos V5 qui exigeait la synchronisation des horloges d’un réseau. Dès Windows Server 2003, W32Time.dll a offert une synchronisation des horloges réseau plus précise par rapport au système d’exploitation Windows Server 2000. De plus, il a pris en charge une grande diversité d’appareils et de protocoles de temps réseau au moyen de fournisseurs de temps. Bien que conçues à l’origine pour assurer la synchronisation des horloges pour l’authentification Kerberos, de nombreuses applications actuelles utilisent des horodatages pour garantir la cohérence transactionnelle, enregistrer l’heure des événements importants et d’autres informations critiques pour l’entreprise et sensibles au facteur temps. Ces applications bénéficient de la synchronisation de l’heure entre les ordinateurs assurée par le service de temps Windows.  
  
## <a name="importance-of-time-protocols"></a><a name="BKMK_TimeProtocols"></a>Importance des protocoles de temps  
Les protocoles de temps établissent une communication entre deux ordinateurs pour échanger les informations d’heure dont ils se servent pour synchroniser leurs horloges. Avec le protocole de temps du service de temps Windows, un client demande les informations d’heure à un serveur et synchronise son horloge en fonction des informations qu’il reçoit.  
  
Le service de temps Windows utilise NTP pour permettre la synchronisation de l’heure sur un réseau. NTP est un protocole de temps Internet qui comprend les algorithmes de discipline nécessaires à la synchronisation des horloges. Ce protocole de temps est plus précis que SNTP (Simple Network Time Protocol) qui est utilisé dans certaines versions de Windows. Cependant, W32Time continue de prendre en charge SNTP pour permettre une compatibilité descendante avec les ordinateurs qui exécutent des services de temps SNTP, comme Windows 2000.  
  
## <a name="see-also"></a>Voir aussi  
[Fonctionnement du service de temps Windows](How-the-Windows-Time-Service-Works.md)  
[Paramètres et outils du service de temps Windows](Windows-Time-Service-Tools-and-Settings.md)  
[Article 902229 de la Base de connaissances Microsoft](https://go.microsoft.com/fwlink/?LinkId=186066)