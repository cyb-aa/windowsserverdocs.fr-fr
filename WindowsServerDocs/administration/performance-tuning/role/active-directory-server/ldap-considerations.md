---
title: Considérations relatives à LDAP de réglage des performances d’AD DS
description: Considérations relatives à LDAP dans les charges de travail Active Directory
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 79f95c88c49d384f8a13b8808c63a0dc00de53cb
ms.sourcegitcommit: d84dc3d037911ad698f5e3e84348b867c5f46ed8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266628"
---
# <a name="ldap-considerations-in-adds-performance-tuning"></a>Considérations relatives à LDAP de réglage des performances d’AD DS

>[!Important]
> Voici un résumé des recommandations essentielles et considérations pour optimiser le matériel de serveur pour les charges de travail Active Directory décrites plus en détail dans le [planification de la capacité pour les Services de domaine Active Directory](https://go.microsoft.com/fwlink/?LinkId=324566) article. Les lecteurs sont vivement encouragés à consulter [planification de la capacité pour les Services de domaine Active Directory](https://go.microsoft.com/fwlink/?LinkId=324566) pour une meilleure compréhension technique et les implications de ces recommandations.

## <a name="verify-ldap-queries"></a>Vérifier les requêtes LDAP

Vérifier que les requêtes LDAP sont conformes aux recommandations création des requêtes efficaces.

Il existe une documentation complète sur MSDN sur la façon d’écrire, structure et analyser des requêtes pour une utilisation par rapport à Active Directory correctement. Pour plus d’informations, consultez [efficient Applications](https://msdn.microsoft.com/library/ms808539.aspx).

## <a name="optimize-ldap-page-sizes"></a>Optimiser les tailles de page LDAP

Lors du renvoi des résultats avec plusieurs objets en réponse aux demandes des clients, le contrôleur de domaine a stocker temporairement le jeu de résultats en mémoire. Augmentation des tailles de page entraînent l’utilisation de la mémoire et peuvent classer des éléments hors cache inutilement. Dans ce cas, les paramètres par défaut sont optimales. Il existe plusieurs scénarios où les recommandations ont été apportées pour augmenter les paramètres de taille de page. Nous vous recommandons d’utiliser les valeurs par défaut, sauf si spécifiquement identifiées comme inadéquat.

Lorsque les requêtes présentent un grand nombre de résultats, une limite d’exécutées simultanément des requêtes semblables peut-être être rencontrée.  Cela se produit car le serveur LDAP peut épuiser une zone de mémoire globale appelée le pool de cookies.  Il peut être nécessaire d’augmenter la taille du pool, comme indiqué dans [comment LDAP Server les Cookies sont contrôlés](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/manage/how-ldap-server-cookies-are-handled).

Pour régler ces paramètres, consultez [Windows Server 2008 et version ultérieure contrôleur_domaine retourne des valeurs uniquement 5000 dans une réponse LDAP](https://support.microsoft.com/kb/2009267).

## <a name="determine-whether-to-add-indices"></a>Déterminer s’il faut ajouter des index

L’indexation des attributs est utile lorsque vous recherchez des objets qui ont le nom d’attribut dans un filtre. L’indexation peut réduire le nombre d’objets qui doivent être visités lors de l’évaluation du filtre. Toutefois, cela réduit les performances des opérations d’écriture, car l’index doit être mis à jour lorsque l’attribut correspondant est modifié ou ajouté. Elle augmente également la taille de la base de données d’annuaire, bien que les avantages l’emportent souvent sur le coût du stockage. La journalisation peut être utilisée pour rechercher des requêtes coûteux et inefficaces. Une fois identifiées, pensez à indexer certains attributs qui sont utilisés dans les requêtes correspondantes pour améliorer les performances de recherche. Pour plus d’informations sur le fonctionnement des recherches Active Directory, consultez [Active Directory recherches fonctionnement](https://technet.microsoft.com/library/cc755809.aspx).

### <a name="scenarios-that-benefit-in-adding-indices"></a>Scénarios qui bénéficient lors de l’ajout d’index

-   Charge du client demande les données génère une importante utilisation du processeur et le comportement de la requête client ne peut pas être modifié ou optimisé. Par une charge importante, considérez qu’il s’affiche lui-même dans une liste de coupable de 10 principales dans Server Performance Advisor ou l’intégrée Active Directory données ensemble de collecteurs et à l’aide de plus de 1 % du processeur.

-   La charge du client est la génération d’e/s disque importantes sur un serveur en raison d’un attribut non indexée et le comportement de la requête client ne peut pas être modifié ou optimisé.

-   Une requête est trop de temps et ne se termine pas dans un délai acceptable pour le client en raison d’un manque de couvrant les indices.

-   Grands volumes de requêtes avec des durées importantes sont à l’origine de la consommation et l’insuffisance des Threads de LDAP ATQ. Surveiller les compteurs de performances suivants :

    -   **NTDS\\latence des requêtes** – c’est soumis à la durée pendant laquelle la demande prend au processus. Active Directory arrive à expiration demandes après 120 secondes (valeur par défaut), toutefois, la majorité doit s’exécuter beaucoup plus rapidement et requêtes extrêmement longues doivent obtenir masqué dans le nombre global. Recherchez les modifications dans cette ligne de base, plutôt que les seuils absolus.

        > [!Note]   Les valeurs élevées ici peuvent également être des indicateurs de retards dans « proxy » pour les requêtes à d’autres domaines et les vérifications de révocation de certificats.


    -   **NTDS\\délai de file d’attente estimé** : cela doit idéalement être proche de 0 pour des performances optimales car cela signifie que les demandes ne passent aucun temps d’attente d’être traitées.

Ces scénarios peuvent être détectés à l’aide d’un ou plusieurs des approches suivantes :

-   [Détermination de minutage de requête avec le contrôle de statistiques](https://msdn.microsoft.com/library/ms808539.aspx)

-   [Suivi des recherches coûteux et inefficaces](https://msdn.microsoft.com/library/ms808539.aspx)

-   Ensemble de collecteurs de données de Diagnostics de Active Directory dans l’Analyseur de performances ([fils de SPA : Ensembles de collecteurs de données AD Win2008 et au-delà](http://blogs.technet.com/b/askds/archive/2010/06/08/son-of-spa-ad-data-collector-sets-in-win2008-and-beyond.aspx))

-   [Microsoft Server Performance Advisor](../../../server-performance-advisor/microsoft-server-performance-advisor.md) Pack conseiller d’Active Directory

-   Recherches à l’aide de n’importe quel filtre outre » (objectClass =\*) » qui utilisent l’Index d’ancêtres.

### <a name="other-index-considerations"></a>Autres considérations relatives aux index

-   Assurez-vous que la création de l’index est la bonne solution au problème une fois le paramétrage de la requête sont épuisée en tant qu’option. Il est très important de dimensionnement matériel correctement. Index doivent être ajoutés uniquement lorsque la solution appropriée consiste à indexer l’attribut et pas une tentative de brouiller les problèmes matériels.

-   Indices augmenter la taille de la base de données en un minimum de la taille totale de l’attribut en cours d’indexation. Une estimation de la croissance de la base de données peut par conséquent être évaluée en prenant la taille moyenne des données dans l’attribut et en multipliant par le nombre d’objets qui possédera l’attribut rempli. Il s’agit généralement une augmentation de 1 % de la taille de la base de données. Pour plus d’informations, consultez [Data Store fonctionnement](https://technet.microsoft.com/library/cc772829.aspx).

-   Si le comportement de la recherche est principalement effectuée au niveau de l’unité d’organisation, pensez à indexer pour des recherches.

-   Index de tuple est supérieures à index normal, mais il est beaucoup plus difficile à estimer la taille. Utilisent des indices normales taille estime que la valeur plancher de croissance, avec un maximum de 20 %. Pour plus d’informations, consultez [Data Store fonctionnement](https://technet.microsoft.com/library/cc772829.aspx).

-   Si le comportement de la recherche est principalement effectuée au niveau de l’unité d’organisation, pensez à indexer pour des recherches.

-   Index de tuple sont nécessaires pour prendre en charge les chaînes de recherche médiane et les chaînes de recherche finale. Index de tuple n’est pas nécessaires pour les chaînes de recherche initiale.

    -   Initiale de chaîne de recherche – (samAccountName = MYPC\*)

    -   Chaîne de recherche médiane - (samAccountName =\*MYPC\*)

    -   Chaîne de recherche finale – (samAccountName =\*MYPC$)

-   Création d’un index génère d’e/s de disque pendant que l’index est en cours de génération. Cela est effectué sur un thread d’arrière-plan avec une priorité plus faible et les demandes entrantes seront prioritaire par rapport à la création d’index. Si la planification de capacité pour l’environnement a été effectuée correctement, cela doit être transparente. Toutefois, les scénarios nécessitant beaucoup d’écritures ou un environnement où la charge sur le stockage de contrôleur de domaine est inconnue peut dégrader l’expérience client et doit être effectué les heures creuses.

-   Impact sur le trafic de réplication est minime, car la création d’index est effectuée localement.

Pour plus d’informations, consultez les rubriques suivantes :

-   [Création d’Applications d’annuaire Microsoft Active plus efficaces](https://msdn.microsoft.com/library/ms808539.aspx)

-   [Recherche dans les Services de domaine Active Directory](https://msdn.microsoft.com/library/aa746427.aspx)

-   [Attributs indexés](https://msdn.microsoft.com/library/windows/desktop/ms677112.aspx)


## <a name="see-also"></a>Voir aussi
- [Les serveurs Active Directory de réglage des performances](index.md)
- [Considérations matérielles](hardware-considerations.md)
- [Positionnement correct des contrôleurs de domaine et considérations relatives au site](site-definition-considerations.md)
- [Résoudre les problèmes de performances d’AD DS](troubleshoot.md) 
- [Planification de la capacité pour les services de domaine Active Directory](https://go.microsoft.com/fwlink/?LinkId=324566)