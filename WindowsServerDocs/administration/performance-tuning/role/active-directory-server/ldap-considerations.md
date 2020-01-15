---
title: Considérations relatives à LDAP dans ajoute l’optimisation des performances
description: Considérations relatives à LDAP dans les charges de travail Active Directory
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 5e407f9f32339e3f9c75e3722ad218228b608b9d
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75947110"
---
# <a name="ldap-considerations-in-adds-performance-tuning"></a>Considérations relatives à LDAP dans ajoute l’optimisation des performances

> [!IMPORTANT]
> Vous trouverez ci-dessous un résumé des recommandations et des considérations essentielles pour optimiser le matériel serveur pour les charges de travail de Active Directory couvertes plus en détail dans l’article [planification de la capacité pour Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) . Les lecteurs sont vivement encouragés à passer en revue la [planification de la capacité pour Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) pour une meilleure compréhension technique et les implications de ces recommandations.

## <a name="verify-ldap-queries"></a>Vérifier les requêtes LDAP

Vérifiez que les requêtes LDAP sont conformes aux recommandations de création de requêtes efficaces.

MSDN propose une documentation complète sur l’écriture, la structure et l’analyse correctes des requêtes à utiliser sur Active Directory. Pour plus d’informations, consultez [création d’applications Microsoft Active Directory plus efficaces](https://msdn.microsoft.com/library/ms808539.aspx).

## <a name="optimize-ldap-page-sizes"></a>Optimiser les tailles de page LDAP

Lors du retour de résultats avec plusieurs objets en réponse aux demandes des clients, le contrôleur de domaine doit stocker temporairement le jeu de résultats en mémoire. L’amélioration de la taille des pages entraînera une plus grande utilisation de la mémoire et peut inutiler l’ancienneté des éléments dans le cache. Dans ce cas, les paramètres par défaut sont optimaux. Il existe plusieurs scénarios dans lesquels des recommandations ont été formulées pour augmenter les paramètres de taille de page. Nous vous recommandons d’utiliser les valeurs par défaut, sauf si elles sont spécifiquement identifiées comme inadéquates.

Lorsque les requêtes ont de nombreux résultats, une limite de requêtes similaires exécutées simultanément peut être rencontrée.  Cela se produit lorsque le serveur LDAP peut épuiser une zone de mémoire globale appelée « pool de cookies ».  Il peut être nécessaire d’augmenter la taille du pool, comme indiqué dans [la rubrique gestion des cookies du serveur LDAP](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/manage/how-ldap-server-cookies-are-handled).

Pour régler ces paramètres, consultez [Windows Server 2008 et le contrôleur de domaine plus récent retourne seulement 5000 valeurs dans une réponse LDAP](https://support.microsoft.com/kb/2009267).

## <a name="determine-whether-to-add-indices"></a>Déterminer si les index doivent être ajoutés

Les attributs d’indexation sont utiles lors de la recherche d’objets qui ont le nom d’attribut dans un filtre. L’indexation peut réduire le nombre d’objets qui doivent être visités lors de l’évaluation du filtre. Toutefois, cela réduit les performances des opérations d’écriture, car l’index doit être mis à jour lorsque l’attribut correspondant est modifié ou ajouté. Cela augmente également la taille de la base de données d’annuaire, bien que les avantages l’emportent souvent sur le coût du stockage. La journalisation peut être utilisée pour rechercher les requêtes coûteuses et inefficaces. Une fois identifié, pensez à indexer certains attributs utilisés dans les requêtes correspondantes pour améliorer les performances de recherche. Pour plus d’informations sur le fonctionnement des Active Directory les recherches, consultez fonctionnement des [Active Directory](https://technet.microsoft.com/library/cc755809.aspx).

### <a name="scenarios-that-benefit-in-adding-indices"></a>Scénarios qui tirent parti de l’ajout d’index

-   La charge du client dans la demande des données génère une utilisation importante du processeur et le comportement de la requête client ne peut pas être modifié ou optimisé. Pour une charge significative, considérez qu’elle s’affichera dans une liste des 10 premiers contrevenants dans Server Performance Advisor ou dans l’ensemble de collecteurs de données Active Directory intégrés et utilise plus de 1% de l’UC.

-   La charge du client génère des e/s disque importantes sur un serveur en raison d’un attribut non indexé et le comportement de la requête client ne peut pas être modifié ou optimisé.

-   Une requête prend beaucoup de temps et ne se termine pas dans un délai acceptable pour le client en raison de l’absence d’index de couverture.

- Des volumes importants de requêtes avec des durées élevées sont à l’origine de la consommation et de l’épuisement des threads LDAP ATQ. Surveillez les compteurs de performances suivants :

    - **Latence des demandes NTDS\\** : cette durée est fonction du temps nécessaire au traitement de la demande. Active Directory les requêtes Expires après 120 secondes (par défaut). Toutefois, la majorité doit exécuter des requêtes beaucoup plus rapides et extrêmement longues doivent être masquées dans les nombres globaux. Recherchez les modifications dans cette ligne de base, plutôt que les seuils absolus.

        > [!NOTE]
        > Les valeurs élevées ici peuvent également être des indicateurs de retards dans les demandes de « proxy » à d’autres domaines et vérifications de liste de révocation de certificats.

    - **Délai de la file d’attente estimé de NTDS\\** . cette valeur doit idéalement être proche de 0 pour des performances optimales, car cela signifie que les demandes ne passent pas de temps à attendre la maintenance.

Ces scénarios peuvent être détectés à l’aide d’une ou plusieurs des approches suivantes :

-   [Détermination du minutage des requêtes avec le contrôle des statistiques](https://msdn.microsoft.com/library/ms808539.aspx)

-   [Suivi des recherches coûteuses et inefficaces](https://msdn.microsoft.com/library/ms808539.aspx)

-   Active Directory ensemble de collecteurs de données de diagnostics dans l’analyseur de performances ([fils de Spa : ensembles de collecteurs de données AD dans Win2008 et au-delà](https://blogs.technet.com/b/askds/archive/2010/06/08/son-of-spa-ad-data-collector-sets-in-win2008-and-beyond.aspx))

-   [Microsoft Server Performance Advisor](../../../server-performance-advisor/microsoft-server-performance-advisor.md) Active Directory Advisor Pack

-   Recherche à l’aide de n’importe quel filtre en dehors de « (objectClass =\*) » qui utilise l’index d’ancêtres.

### <a name="other-index-considerations"></a>Autres considérations relatives aux index

-   Assurez-vous que la création de l’index est la solution la plus adaptée au problème une fois que le paramétrage de la requête est épuisé. Le dimensionnement du matériel est très important. Les index doivent être ajoutés uniquement lorsque le correctif correct consiste à indexer l’attribut, et non à une tentative d’obscurcissement des problèmes matériels.

-   Les index augmentent la taille de la base de données d’un minimum de la taille totale de l’attribut indexé. Une estimation de la croissance de la base de données peut donc être évaluée en prenant la taille moyenne des données dans l’attribut et en multipliant par le nombre d’objets dont l’attribut sera rempli. En général, il s’agit d’une augmentation de 1% de la taille de la base de données. Pour plus d’informations, consultez fonctionnement [du magasin de données](https://technet.microsoft.com/library/cc772829.aspx).

-   Si le comportement de recherche est principalement effectué au niveau de l’unité d’organisation, envisagez d’indexer les recherches en conteneur.

-   Les index de tuple sont plus grands que les index normaux, mais il est beaucoup plus difficile d’estimer la taille. Utilisez des estimations de taille de l’index normal comme étage de croissance, avec un maximum de 20%. Pour plus d’informations, consultez fonctionnement [du magasin de données](https://technet.microsoft.com/library/cc772829.aspx).

-   Si le comportement de recherche est principalement effectué au niveau de l’unité d’organisation, envisagez d’indexer les recherches en conteneur.

-   Les index de tuple sont nécessaires pour prendre en charge les chaînes de recherche médiane et les chaînes de recherche finales. Les index de tuple ne sont pas nécessaires pour les chaînes de recherche initiales.

    -   Chaîne de recherche initiale – (samAccountName = MYPC\*)

    -   Chaîne de recherche médial-(samAccountName =\*MYPC\*)

    -   Chaîne de recherche finale – (samAccountName =\*MYPC $)

-   La création d’un index génère des e/s de disque pendant la génération de l’index. Cette opération s’effectue sur un thread d’arrière-plan avec une priorité plus faible et les demandes entrantes sont prioritaires par rapport à la création de l’index. Si la planification de la capacité pour l’environnement a été correctement effectuée, cela doit être transparent. Toutefois, les scénarios à écriture intensive ou un environnement dans lequel la charge sur le stockage du contrôleur de domaine est inconnue peuvent dégrader l’expérience du client et doivent être effectuées hors des heures.

-   Les répercussions sur le trafic de réplication sont minimes dans la mesure où la création d’index se produit localement.

Pour plus d’informations, consultez les rubriques suivantes :

-   [Création d’applications Microsoft Active Directory plus efficaces](https://msdn.microsoft.com/library/ms808539.aspx)

-   [Recherche dans Active Directory Domain Services](https://msdn.microsoft.com/library/aa746427.aspx)

-   [Attributs indexés](https://msdn.microsoft.com/library/windows/desktop/ms677112.aspx)

## <a name="see-also"></a>Articles associés

- [Réglage des performances Active Directory serveurs](index.md)
- [Considérations matérielles](hardware-considerations.md)
- [Positionnement correct des contrôleurs de domaine et considérations relatives au site](site-definition-considerations.md)
- [Résoudre les problèmes de performances d’AD DS](troubleshoot.md) 
- [Planification de la capacité pour les services de domaine Active Directory](https://go.microsoft.com/fwlink/?LinkId=324566)