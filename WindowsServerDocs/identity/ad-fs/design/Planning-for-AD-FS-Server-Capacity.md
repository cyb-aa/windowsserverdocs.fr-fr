---
ms.assetid: ef91f1d8-2991-4d90-b687-5fa189737c88
title: "Planification de la capacité du serveur FS AD"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 484dd08edef85b91e777f8963f175a6172c75430
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="planning-for-ad-fs-server-capacity"></a>Planification de la capacité du serveur FS AD

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

  
> [!NOTE]  
> Le contenu de cette rubrique ne reflète pas tests réels effectués sur des serveurs exécutant Windows Server2012. Cette rubrique sera mise à jour une fois les tests nécessaires a été effectuée.  
  
La capacité de planification d’ActiveDirectory Federation Services \(ADFS\) est le processus de planification et prévision des pics d’utilisation pour votre Service de fédération ou scaling\ des contraintes de charge de votre déploiement de serveur ADFS pour répondre à.  
  
Cette section fournit des directives de déploiement pour le serveur de fédération et les rôles de serveur proxy de serveur de fédération et est basée sur des tests de laboratoire effectués par l’équipe du produit ADFS chez Microsoft. L’objectif de ce contenu est de vous aider à:  
  
-   Estimer près les besoins en matériel de ADFS déploiement spécifique votre organisation, comme le nombre de serveurs ADFS.  
  
-   Avec précision les pics attendu pour les demandes de connexion, planifier les hausses et assurez-vous que votre déploiement d’ADFS est capable de la gestion des pics.  
  
Avant de poursuivre la lecture de ce contenu de la planification de capacité, nous vous recommandons abord d'effectuer les tâches dans l’ordre indiqué dans les deux tableaux suivants. Dans le premier tableau, nous fournissons des liens vers des tâches recommandées qui aideront à fournissent un contexte approprié pour cette présentation de la planification de capacité.  
  
|Tâche recommandée|Description|Référence|  
|--------------------|---------------|-------------|  
|Comprendre la configuration requise pour le déploiement de serveurs de fédération ADFS et des serveurs proxy de fédération|Passez en revue importante configuration matérielle et logicielle requise pour le déploiement de serveur de fédération et les serveurs proxy de fédération.|[Annexe a: examen de configuration ADFS requise](Appendix-A--Reviewing-AD-FS-Requirements.md)|  
|Sélectionnez le type de base de données de configuration ADFS que vous allez déployer dans votre organisation|Avant de commencer à l’aide de données de planification de la capacité de cette section, vous devez tout d’abord déterminer le type de base de données de configuration ADFS que vous allez déployer, \(WID\) base de données interne Windows ou une base de données \(SQL\) Structured Query Language.|[Le rôle de la base de données de Configuration ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md);<br /><br />[Considérations sur la topologie déploiement ADFS](AD-FS-Deployment-Topology-Considerations.md)|  
|Déterminer le type de topologie à utiliser avec votre nouvelle sélection de base de données de configuration ADFS|Une fois que vous avez décidé du type de base de données de configuration ADFS à utiliser dans votre déploiement, vous devez prendre en compte topologie de déploiement qui correspond le mieux à où vous devez placer les serveurs de fédération et la fédération serveurs proxy au sein de votre environnement de production.|[Déterminer votre topologie de déploiement ADFS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Comprendre la clé ADFS liées capacité termes de planification|Passez en revue les définitions des termes sont utilisés dans l’ensemble de la présentation de la planification de la capacité ADFS de planification de capacité courants.|Consultez la section intitulée [la capacité ADFS en termes de planification](Planning-for-AD-FS-Server-Capacity.md#bk_terms) dans cette rubrique.|  
  
Une fois que vous avez consulté le contenu dans le tableau précédent, vous pouvez effectuer les tâches préalables dans le tableau suivant.  
  
|Tâche préalable|Description|Référence|  
|---------------------|---------------|-------------|  
|Télécharger la capacité ADFS planification de la feuille de calcul sur|La feuille de calcul sur la planification de la capacité ADFS peut vous aider à déterminer le nombre de serveurs de fédération requis pour un déploiement de la batterie de serveurs de fédération ADFS. Des instructions pour l’utilisation de cette feuille de calcul sont disponibles dans le lien ci-dessous pour la tâche suivante.|[Calcul de planification de la capacité ADFS](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx)|  
|Collecter des données sur le nombre d’utilisateurs qui auront besoin de connexion-\(SSO\) accès unique à l’application prenant en charge claims\ cible et les périodes de l’utilisation de pointe attendu associés à cet accès|Ces données utilisateur que vous recueillez seront être utilisées pour les valeurs d’entrée requis dans le contexte de la feuille de dimensionnement d’une planification ADFS la capacité.|[Estimer le nombre de serveurs de fédération pour votre organisation](Planning-for-Federation-Server-Capacity.md#bk_estimatefs)|  
|Calcul de planification de la capacité ADFS pour Windows Server2016|Feuille de planification de la mise à jour pour Windows Server2016|[ADFS Windows Server2016 planification](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)  
  
## <a name="bk_terms"></a>Termes de la planification de la capacité ADFS  
Le tableau suivant décrit les termes importants couramment utilisés dans cette section du Guide de conception ADFS de planification de capacité. Pour obtenir une liste plus complète des termes du contrat de services ADFS, voir [Understanding Key ADFS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
|Terme|Définition|  
|--------|--------------|  
|Utilisateurs simultanés|Nombre estimé d’utilisateurs censés envoyer des demandes au service au sein d’une période donnée, généralement une période d’activité maximale.|  
|Utilisateurs actifs|Le nombre moyen approximatif d’utilisateurs qui sont actifs sur un système, mais pas nécessairement envoi des demandes, pendant une période donnée.|  
|Utilisateurs définis|Un nombre d’utilisateurs maximal théorique, généralement en fonction du nombre d’utilisateurs qui ont défini des comptes dans le système.|  
|Demandes par seconde|Le nombre de demandes soit soumises par les clients \ (le cas la charge sur une transcription) ou traitées par les serveurs \ (le cas throughput\ serveur) dans une seconde. Cette mesure permet de planifier le processeur du serveur et la capacité de mémoire.|  
|La réactivité du serveur cible et utilisation|Mesures qui délimitent la plage de performances acceptable pour les serveurs. En règle générale, si la réactivité est ou l’utilisation la cible, le système est considérée comme surchargée et plus de capacité est nécessaire.|  
|\(WID\) de la base de données interne Windows|La par défaut ADFS configuration base de données qui permettre être utilisée comme alternative à SQLServer dans certains déploiements ADFS.|  
  
## <a name="configuration-environment-used-during-ad-fs-testing"></a>Environnement de configuration utilisé pendant le test d’ADFS  
Cette section décrit l’environnement de configuration que l’équipe du produit ADFS utilisé pour effectuer des tests. L’équipe a utilisé le matériel de l’ordinateur suivant, logiciels et de configuration réseau pour collecter des données de performances et évolutivité dans les tests du serveur de fédération:  
  
-   Double quadruple cœur 2,27gigahertz \(GHz\) \(8cores\)  
  
-   16GO DE RAM  
  
-   Windows Server2008R2, Enterprise Edition  
  
-   Réseau Gigabit  
  
> [!NOTE]  
> Bien que 16Go de RAM a été utilisé sur le serveur de fédération au cours du test, une mémoire moindre, par exemple 4Go de RAM par serveur de fédération peut servir pour la plupart des déploiements ADFS. Les recommandations fournies dans cette ADFS planification de la capacité contenu ainsi que les résultats fournis par la feuille de planification ADFS capacité sont basées sur les hypothèses que chaque serveur de fédération utilise approximativement 4Go 's de RAM pour la plupart des environnements de production ADFS.  
  
L’équipe de produit permet de collecter des données de performances et évolutivité pour le serveur proxy de fédération test la configuration suivante:  
  
-   Double \(4cores\) quadruple cœur 2,24GHz  
  
-   4\ GO DE RAM  
  
-   Windows Server2008R2, Enterprise Edition  
  
-   Réseau Gigabit  
  
> [!NOTE]  
> Recommandations concernant la capacité pour les serveurs ADFS peut varier considérablement, suivant les spécifications que vous choisissez pour la configuration matérielle et réseau à utiliser dans un environnement donné. En tant que point de référence, les recommandations de dimensionnement fournies dans ce contenu sont basée sur une cible de l’utilisation de 80% sur les ordinateurs mentionnés précédemment.  
  
## <a name="measure-ad-fs-server-capacity"></a>ADFS de mesurer la capacité du serveur  
En règle générale, les composants matériels qui affectent l’évolutivité et les performances du serveur sont le processeur, mémoire, le disque et les cartes réseau. Heureusement, chacun des composants ADFS nécessite très peu de mémoire et espace disque. Connectivité réseau est une contrainte évidente. Par conséquent, les tests de charge qui sont effectuées sur les serveurs de fédération et les serveurs proxy de fédération se concentrer sur deux aspects principaux pour mesurer la capacité du serveur:  
  
-   **Nombre maximal de demandes ADFS par seconde:** le nombre de demandes de connexion traitées par seconde sur les serveurs de fédération. Cette mesure vous permet de déterminer combien d’utilisateurs permettre se connecter à un serveur donné. Vous pouvez utiliser cette mesure conjointement avec la mesure de consommation du processeur pour comprendre l’effet de cette mesure sur les performances.  
  
-   **Consommation du processeur:** le pourcentage de mesure, la capacité en processeur. Cette mesure vous permet de déterminer la charge processeur globale qui s’est produite en fonction du nombre de demandes de connexion entrantes par seconde.  
  
## <a name="continue-reading-more-about-ad-fs-capacity-planning"></a>Poursuivez la lecture plus d’informations sur la planification de la capacité ADFS  
Une fois que vous avez effectué les tâches préalables et familiarisez-vous avec la terminologie et la configuration matérielle requise, vous pouvez utiliser la contenu de planification de la capacité supplémentaire suivante pour vous aider à déterminer le nombre recommandé de serveurs ADFS requis pour le déploiement:  
  
-   [Planification de la capacité de serveur de fédération](Planning-for-Federation-Server-Capacity.md)  
  
-   [Planification de capacité des serveurs Proxy de fédération](Planning-for-Federation-Server-Proxy-Capacity.md)  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
