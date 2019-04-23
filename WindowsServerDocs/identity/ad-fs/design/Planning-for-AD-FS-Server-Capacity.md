---
ms.assetid: ef91f1d8-2991-4d90-b687-5fa189737c88
title: Planification de la capacité des serveurs AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 484dd08edef85b91e777f8963f175a6172c75430
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847390"
---
# <a name="planning-for-ad-fs-server-capacity"></a>Planification de la capacité des serveurs AD FS

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
> [!NOTE]  
> Le contenu fourni dans cette rubrique ne reflète pas de tests réels effectués sur des serveurs exécutant Windows Server 2012. Cette rubrique sera mise à jour une fois les tests nécessaires effectués.  
  
Planification de capacité pour Active Directory Federation Services \(AD FS\) est le processus de prévision des pics d’utilisation pour votre Service de fédération et la planification ou la mise à l’échelle\-de votre déploiement de serveur AD FS en fonction de ces charger configuration requise.  
  
Cette section décrit les instructions de déploiement pour le serveur de fédération et les rôles de serveur proxy de serveur de fédération et est basée sur des tests de laboratoire a été effectuée par l’équipe de produit AD FS chez Microsoft. Ce contenu a pour but de vous aider à :  
  
-   Estimer de près les besoins en matériel spécifique déploiement de votre organisation d’AD FS, telles que le nombre de serveurs AD FS.  
  
-   Projet avec précision l’utilisation maximale attendue pour la connexion\-dans les requêtes, de planifier la croissance et de vous assurer que votre déploiement AD FS est capable de gérer ces pics.  
  
Avant de poursuivre la lecture de ce contenu consacré à la planification de la capacité, prenez le temps d'effectuer les tâches mentionnées dans les deux tableaux suivants, dans l'ordre indiqué. Le premier tableau répertorie des tâches recommandées pour la planification de la capacité, ainsi que des liens vers des sources d'information complémentaires.  
  
|Tâche recommandée|Description|Référence|  
|--------------------|---------------|-------------|  
|Comprendre la configuration requise pour le déploiement de serveurs de fédération AD FS et des serveurs proxy de fédération|Passez en revue les configurations matérielles et logicielles requises qui ont leur importance pour le déploiement d'un serveur de fédération et de serveurs proxy de fédération.|[Annexe a : Examen des exigences en matière de AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md)|  
|Sélectionnez le type de base de données de configuration AD FS que vous allez déployer dans votre organisation|Avant de commencer à l’aide des données de planification de capacité dans cette section, vous devez d’abord déterminer quelle configuration AD FS que vous allez déployer, de type base de données interne Windows de base de données \(WID\) ou un langage de requête structuré \(SQL\) base de données.|[The Role of the AD FS Configuration Database](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md);<br /><br />[Considérations sur la topologie déploiement AD FS](AD-FS-Deployment-Topology-Considerations.md)|  
|Déterminer le type de topologie à utiliser en fonction de la base de données de configuration AD FS sélectionnée|Après avoir choisi le type de base de données de configuration AD FS à utiliser dans votre déploiement, vous devez déterminer la topologie de déploiement qui convient le mieux là où vous devrez placer des serveurs de fédération et des serveurs proxy de fédération au sein de votre environnement de production.|[Déterminer votre topologie de déploiement AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Comprendre les clés AD FS liées planification de capacité termes du contrat|Passez en revue les définitions des termes utilisés tout au long de la capacité AD FS présentation de la planification de planification de capacité courants.|Consulter la section intitulée [AD FS capacity planning terms](Planning-for-AD-FS-Server-Capacity.md#bk_terms) dans cette rubrique|  
  
Après avoir consulté le contenu du tableau précédent, vous pouvez effectuer les tâches préalables indiquées dans le tableau ci-après.  
  
|Tâche préalable|Description|Référence|  
|---------------------|---------------|-------------|  
|Télécharger la planification de la feuille de calcul sur la capacité AD FS|La feuille de calcul sur la planification de la capacité AD FS peut vous aider à déterminer le nombre de serveurs de fédération requis pour un déploiement de batterie de serveurs de fédération AD FS. Les instructions d'utilisation de cette feuille de calcul sont disponibles en cliquant sur le lien fourni ci-après pour la tâche suivante.|[Calcul de planification de la capacité AD FS](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx)|  
|Collecter des données sur le nombre d’utilisateurs qui auront besoin d’authentification unique\-sur \(SSO\) accès aux revendications cible\-application prenant en charge et les périodes d’utilisation maximale attendue associés à cet accès|Ces données utilisateur permettront d'alimenter la feuille de calcul sur la planification de la capacité AD FS.|[Estimer le nombre de serveurs de fédération pour votre organisation](Planning-for-Federation-Server-Capacity.md#bk_estimatefs)|  
|Calcul de planification de la capacité AD FS pour Windows Server 2016|Feuille de planification de mise à jour pour Windows Server 2016|[AD FS Windows planification de capacité du serveur 2016](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)  
  
## <a name="bk_terms"></a>Termes planification de la capacité AD FS  
Le tableau suivant décrit les termes importants qui sont souvent utilisées dans cette section du Guide de conception AD FS de planification de la capacité. Pour obtenir une liste plus complète des termes d’AD FS, consultez [Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
|Terme|Définition|  
|--------|--------------|  
|Utilisateurs simultanés|Nombre estimé d'utilisateurs censés envoyer des demandes au service pendant une période donnée, généralement un pic d'activité.|  
|Utilisateurs actifs|Nombre moyen approximatif d'utilisateurs qui sont actifs sur un système, mais qui n'envoient pas nécessairement de demandes, pendant une période donnée.|  
|Utilisateurs définis|Nombre d'utilisateurs maximal théorique, généralement basé sur le nombre d'utilisateurs pour lesquels des comptes sont définis sur le système.|  
|Demandes par seconde|Le nombre de demandes soit soumis par les clients \(lorsque nous parlons de la charge sur un système\) ou traités par les serveurs \(lorsque nous parlons de débit du serveur\) en une seconde. Cette mesure permet de planifier la capacité en processeurs et en mémoire des serveurs.|  
|Réactivité et utilisation cibles des serveurs|Mesures qui délimitent la plage de performances acceptable pour les serveurs. En règle générale, si la réactivité est trop basse, ou l'utilisation trop élevée, par rapport aux valeurs cibles correspondantes, le système est considéré comme étant surchargé et la capacité doit être renforcée.|  
|Base de données interne Windows \(WID\)|La valeur par défaut AD FS configuration base de données qui peut être utilisé comme alternative à SQL Server dans certains déploiements AD FS.|  
  
## <a name="configuration-environment-used-during-ad-fs-testing"></a>Environnement de configuration utilisé pendant les tests d'AD FS  
Cette section décrit l’environnement de configuration utilisé par l’équipe de produit AD FS pour les différents tests. L'équipe a utilisé la configuration matérielle, logicielle et réseau suivante pour recueillir les données de performance et d'extensibilité au cours des tests du serveur de fédération :  
  
-   Biprocesseur quadruple cœur 2,27 gigahertz \(GHz\) \(8 cœurs\)  
  
-   16\-GO DE RAM  
  
-   Windows Server 2008 R2 Édition Entreprise  
  
-   Réseau Gigabit  
  
> [!NOTE]  
> Bien que 16 Go de RAM a été utilisé sur le serveur de fédération pendant le test, une taille de mémoire plus modérée, par exemple, 4 Go de RAM par serveur de fédération utilisable pour la plupart des déploiements AD FS. Les recommandations sont fournies dans cette AD FS planification de la capacité contenu, ainsi que les résultats fournis par l’AD FS planification de la feuille de calcul capacité sont basées sur les hypothèses que chaque serveur de fédération utilise approximativement 's de 4 Go de RAM pour la plupart des production AD FS environnements.  
  
L'équipe de produit a utilisé la configuration suivante pour recueillir les données de performance et d'extensibilité relatives aux tests du serveur proxy de fédération :  
  
-   Biprocesseur quadruple cœur 2,24 GHz \(4 cœurs\)  
  
-   4\-GO DE RAM  
  
-   Windows Server 2008 R2 Édition Entreprise  
  
-   Réseau Gigabit  
  
> [!NOTE]  
> Recommandations concernant la capacité pour les serveurs AD FS peut varier considérablement, selon les spécifications que vous choisissez pour la configuration matérielle et réseau à utiliser dans un environnement donné. À titre de référence, la configuration indiquée dans ce contenu correspond à une valeur d'utilisation cible de 80 % sur les ordinateurs mentionnés précédemment.  
  
## <a name="measure-ad-fs-server-capacity"></a>Mesurer la capacité des serveurs AD FS  
En règle générale, les composants matériels qui influent sur les performances et l'extensibilité des serveurs sont le processeur, la mémoire, le disque et les cartes réseau. Heureusement, chacun des composants AD FS nécessite très peu de mémoire et espace disque. La connectivité réseau est une contrainte évidente. Ainsi, les tests de charge effectués pour mesurer la capacité des serveurs de fédération et des serveurs proxy de fédération portent sur deux aspects principaux :  
  
-   **Demandes AD FS maximales par seconde :** Le nombre de connexion\-dans les demandes qui sont traitées par seconde sur les serveurs de fédération. Cette mesure vous permet de déterminer combien d'utilisateurs peuvent se connecter simultanément à un serveur donné. Cette mesure, associée à la mesure de consommation du processeur, vous donne une idée de l'impact sur les performances.  
  
-   **Consommation du processeur :** mesure, en pourcentage, de la capacité du processeur. Cette mesure peut vous aider à déterminer la charge processeur globale qui s’est produite en fonction du nombre de connexion entrantes\-dans les demandes par seconde.  
  
## <a name="continue-reading-more-about-ad-fs-capacity-planning"></a>Sources d'informations supplémentaires sur la planification de la capacité AD FS  
Une fois que vous avez terminé les tâches préalables requises et que vous êtes familiarisé avec la terminologie et la configuration matérielle requise, vous pouvez utiliser la planification de la capacité supplémentaire suivante pour vous aider à déterminer le nombre recommandé de serveurs AD FS requis pour votre déploiement :  
  
-   [Planification de la capacité de serveur de fédération](Planning-for-Federation-Server-Capacity.md)  
  
-   [Planification de la capacité de Proxy de serveur de fédération](Planning-for-Federation-Server-Proxy-Capacity.md)  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
