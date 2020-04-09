---
ms.assetid: ef91f1d8-2991-4d90-b687-5fa189737c88
title: Planification de la capacité des serveurs AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 984579813a843e6c39bd85bb9aa07cdddfcfc067
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858632"
---
# <a name="planning-for-ad-fs-server-capacity"></a>Planification de la capacité des serveurs AD FS


  
> [!NOTE]  
> Le contenu fourni dans cette rubrique ne reflète pas les tests réels effectués sur les serveurs exécutant Windows Server 2012. Cette rubrique sera mise à jour une fois les tests nécessaires effectués.  
  
La planification de la capacité pour Services ADFS \(AD FS\) est le processus qui consiste à prévoir les pics d’utilisation de votre service FS (Federation Service), ainsi que la planification ou la mise à l’échelle\-de votre déploiement de serveur AD FS pour répondre à ces exigences de charge.  
  
Cette section décrit les instructions de déploiement pour les rôles serveur de Fédération et serveur proxy de Fédération, et est basée sur les tests de laboratoire effectués par l’équipe de produit AD FS chez Microsoft. Ce contenu a pour but de vous aider à :  
  
-   Évaluez attentivement les besoins en matériel pour le déploiement AD FS spécifique de votre organisation, comme le nombre de serveurs AD FS.  
  
-   Projetez avec précision le pic d’utilisation attendu pour la signature\-dans les demandes, planifiez la croissance et assurez-vous que votre déploiement AD FS est capable de gérer les pics d’utilisation attendus.  
  
Avant de poursuivre la lecture de ce contenu consacré à la planification de la capacité, prenez le temps d'effectuer les tâches mentionnées dans les deux tableaux suivants, dans l'ordre indiqué. Le premier tableau répertorie des tâches recommandées pour la planification de la capacité, ainsi que des liens vers des sources d'information complémentaires.  
  
|Tâche recommandée|Description|Référence|  
|--------------------|---------------|-------------|  
|Comprendre la configuration requise pour le déploiement d’AD FS serveurs de Fédération et de serveurs proxys de Fédération|Passez en revue les configurations matérielles et logicielles requises qui ont leur importance pour le déploiement d'un serveur de fédération et de serveurs proxy de fédération.|[Annexe A : examen des exigences de AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md)|  
|Sélectionnez le type de AD FS base de données de configuration que vous allez déployer dans votre organisation|Avant de pouvoir commencer à utiliser les données de planification de la capacité dans cette section, vous devez d’abord déterminer le type de base de données de configuration de AD FS que vous allez déployer, base de données interne Windows \(WID\) ou langage SQL \(base de données SQL\).|[The Role of the AD FS Configuration Database](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md);<p>[Considérations sur la topologie du déploiement d’AD FS](AD-FS-Deployment-Topology-Considerations.md)|  
|Déterminer le type de topologie à utiliser en fonction de la base de données de configuration AD FS sélectionnée|Après avoir choisi le type de base de données de configuration AD FS à utiliser dans votre déploiement, vous devez déterminer la topologie de déploiement qui convient le mieux là où vous devrez placer des serveurs de fédération et des serveurs proxy de fédération au sein de votre environnement de production.|[Déterminer votre topologie de déploiement d’AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Comprendre les principaux termes liés à la planification de la capacité AD FS|Passez en revue les définitions des termes de planification de capacité courants qui sont utilisés tout au long de la AD FS discussion sur la planification de la capacité.|Consultez la section intitulée [Termes de la planification de la capacité AD FS](Planning-for-AD-FS-Server-Capacity.md#bk_terms) dans cette rubrique.|  
  
Après avoir consulté le contenu du tableau précédent, vous pouvez effectuer les tâches préalables indiquées dans le tableau ci-après.  
  
|Tâche préalable|Description|Référence|  
|---------------------|---------------|-------------|  
|Télécharger la feuille de calcul de dimensionnement de la planification de la capacité AD FS|La feuille de calcul de dimensionnement de la planification de la capacité AD FS peut vous aider à déterminer le nombre de serveurs de Fédération requis pour le déploiement d’une batterie de serveurs de fédération AD FS. Les instructions d'utilisation de cette feuille de calcul sont disponibles en cliquant sur le lien fourni ci-après pour la tâche suivante.|[Feuille de calcul de planification de la capacité AD FS](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx)|  
|Collectez des données sur le nombre d’utilisateurs qui auront besoin d’un\-d’authentification unique sur \(l’authentification unique\) l’accès à l’application prenant en charge les revendications\-cible et les périodes d’utilisation maximales attendues associées à cet accès|Ces données utilisateur permettront d'alimenter la feuille de calcul sur la planification de la capacité AD FS.|[Estimer le nombre de serveurs de Fédération pour votre organisation](Planning-for-Federation-Server-Capacity.md#bk_estimatefs)|  
|Feuille de calcul de planification de la capacité AD FS pour Windows Server 2016|Feuille de planification mise à jour pour Windows Server 2016|[AD FS de la planification de la capacité de Windows Server 2016](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)  
  
## <a name="ad-fs-capacity-planning-terms"></a><a name="bk_terms"></a>Conditions de planification de la capacité AD FS  
Le tableau suivant décrit les termes importants qui sont souvent utilisés dans cette section de planification de la capacité du Guide de conception de AD FS. Pour obtenir une liste plus complète des conditions d’AD FS, consultez [comprendre les concepts clés AD FS](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
|Terme|Définition|  
|--------|--------------|  
|Utilisateurs simultanés|Nombre estimé d'utilisateurs censés envoyer des demandes au service pendant une période donnée, généralement un pic d'activité.|  
|Utilisateurs actifs|Nombre moyen approximatif d'utilisateurs qui sont actifs sur un système, mais qui n'envoient pas nécessairement de demandes, pendant une période donnée.|  
|Utilisateurs définis|Nombre d'utilisateurs maximal théorique, généralement basé sur le nombre d'utilisateurs pour lesquels des comptes sont définis sur le système.|  
|Demandes par seconde|Nombre de demandes soumises par les clients \(lors de la discussion sur la charge sur un système\) ou traitées par les serveurs \(en ce qui concerne le débit du serveur\) en une seconde. Cette mesure permet de planifier la capacité en processeurs et en mémoire des serveurs.|  
|Réactivité et utilisation cibles des serveurs|Mesures qui délimitent la plage de performances acceptable pour les serveurs. En règle générale, si la réactivité est trop basse, ou l'utilisation trop élevée, par rapport aux valeurs cibles correspondantes, le système est considéré comme étant surchargé et la capacité doit être renforcée.|  
|Base de données interne Windows \(WID\)|La base de données de configuration AD FS par défaut peut être utilisée comme alternative à SQL Server dans certains déploiements de AD FS.|  
  
## <a name="configuration-environment-used-during-ad-fs-testing"></a>Environnement de configuration utilisé pendant les tests d'AD FS  
Cette section décrit l’environnement de configuration utilisé par l’équipe de produit AD FS pour effectuer ses tests. L'équipe a utilisé la configuration matérielle, logicielle et réseau suivante pour recueillir les données de performance et d'extensibilité au cours des tests du serveur de fédération :  
  
-   Dual Quad Core 2,27 gigahertz \(GHz\) \(8 cœurs\)  
  
-   16\-Go de RAM  
  
-   Windows Server 2008 R2 Édition Entreprise  
  
-   Réseau Gigabit  
  
> [!NOTE]  
> Bien que 16 Go de RAM aient été utilisés sur le serveur de Fédération pendant les tests, une taille de mémoire plus modérée, telle que 4 Go de RAM par serveur de Fédération, peut être utilisée pour la plupart des déploiements de AD FS. Les recommandations fournies dans ce AD FS la planification de la capacité, ainsi que les résultats fournis par la feuille de calcul de planification de la capacité AD FS, sont basés sur des hypothèses que chaque serveur de Fédération utilise approximativement 4 Go de RAM pour la plupart des environnements de production AD FS.  
  
L'équipe de produit a utilisé la configuration suivante pour recueillir les données de performance et d'extensibilité relatives aux tests du serveur proxy de fédération :  
  
-   Dual Quad Core 2,24 GHz \(4 cœurs\)  
  
-   4\-Go de RAM  
  
-   Windows Server 2008 R2 Édition Entreprise  
  
-   Réseau Gigabit  
  
> [!NOTE]  
> Les recommandations en matière de capacité pour les serveurs AD FS peuvent varier considérablement, selon les spécifications que vous choisissez pour la configuration matérielle et réseau à utiliser dans un environnement donné. À titre de référence, la configuration indiquée dans ce contenu correspond à une valeur d'utilisation cible de 80 % sur les ordinateurs mentionnés précédemment.  
  
## <a name="measure-ad-fs-server-capacity"></a>Mesurer la capacité des serveurs AD FS  
En règle générale, les composants matériels qui influent sur les performances et l'extensibilité des serveurs sont le processeur, la mémoire, le disque et les cartes réseau. Heureusement, chacun des composants de AD FS nécessite très peu de mémoire et d’espace disque. La connectivité réseau est une contrainte évidente. Ainsi, les tests de charge effectués pour mesurer la capacité des serveurs de fédération et des serveurs proxy de fédération portent sur deux aspects principaux :  
  
-   Nombre **maximal de demandes de AD FS par seconde :** Nombre de\-de signe dans les demandes traitées par seconde sur les serveurs de Fédération. Cette mesure vous permet de déterminer combien d'utilisateurs peuvent se connecter simultanément à un serveur donné. Cette mesure, associée à la mesure de consommation du processeur, vous donne une idée de l'impact sur les performances.  
  
-   **Consommation du processeur :** mesure, en pourcentage, de la capacité du processeur. Cette mesure peut vous aider à déterminer la charge de l’UC globale qui s’est produite en fonction du nombre de connexions entrantes\-dans les demandes par seconde.  
  
## <a name="continue-reading-more-about-ad-fs-capacity-planning"></a>Sources d'informations supplémentaires sur la planification de la capacité AD FS  
Une fois que vous avez terminé les tâches préalables et que vous vous êtes familiarisé avec les conditions associées et la configuration matérielle requise, vous pouvez utiliser le contenu de planification de capacité supplémentaire suivant pour vous aider à déterminer le nombre recommandé de serveurs AD FS requis pour votre déploiement :  
  
-   [Planification de la capacité des serveurs de fédération](Planning-for-Federation-Server-Capacity.md)  
  
-   [Planification de la capacité des serveurs proxy de fédération](Planning-for-Federation-Server-Proxy-Capacity.md)  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
