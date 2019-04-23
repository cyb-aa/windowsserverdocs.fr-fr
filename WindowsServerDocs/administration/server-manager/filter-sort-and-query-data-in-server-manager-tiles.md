---
title: Filtrer, trier et interroger des données dans les vignettes du Gestionnaire de serveur
description: Gestionnaire de serveur
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8786f791-73e5-4c75-8d12-46e88a196976
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51c0e1f3af727c4e7ddf18024fab9a95808d2338
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868910"
---
# <a name="filter-sort-and-query-data-in-server-manager-tiles"></a>Filtrer, trier et interroger des données dans les vignettes du Gestionnaire de serveur

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dans Windows Server, les vignettes dans le Gestionnaire de serveur vous permettent de filtrer et trier des données et créer et enregistrer des requêtes personnalisées. Vous pouvez trier, utiliser des filtres de mots clés et exécuter des requêtes sur les entrées de liste dans les vignettes événements, performances, Best Practices Analyzer, Services et rôles et fonctionnalités sur un rôle ou groupe de pages de serveur dans le Gestionnaire de serveur.  
  
Cette rubrique contient les sections suivantes.  
  
-   [Filtrer les entrées de liste dans les vignettes](#BKMK_tiles)  
  
-   [trier les entrées de liste de vignettes](#BKMK_sort)  
  
-   [créer et exécuter des requêtes personnalisées sur les données de mosaïque](#BKMK_query)  
  
## <a name="BKMK_tiles"></a>Filtrer les entrées de liste dans les vignettes  
La zone de texte **Filtre** est un moyen rapide de réduire la liste des entrées dans une vignette pour n’afficher que les entrées contenant une chaîne de texte précise.  
  
#### <a name="to-apply-a-filter-to-the-list-of-entries-in-a-tile"></a>Pour appliquer un filtre à la liste des entrées dans une vignette  
  
1.  Ouvrez une page de groupe de serveurs ou de rôles dans le Gestionnaire de serveur.  
  
2.  Dans le **filtre** zone de texte d’une vignette événements, performances, Best Practices Analyzer, Services ou rôles et fonctionnalités, tapez une chaîne sur laquelle vous souhaitez filtrer.  
  
    par exemple, si vous souhaitez afficher uniquement les événements avec un ID d’événement 1014, tapez **1014** dans le **filtre** zone de texte. Tous les événements collectés contenant la chaîne **1014** dans un champ au minimum sont retournés en guise de résultats.  
  
3.  Notez que le filtre modifie le texte de la description sous le titre de la vignette. Au lieu de **Tout** , le texte affiché est **Résultats filtrés**.  
  
4.  Pour supprimer le filtre, supprimez la chaîne dans la zone de texte du filtre ou cliquez sur **X**.  
  
## <a name="BKMK_sort"></a>trier les entrées de liste de vignettes  
trier les entrées de liste dans les vignettes du Gestionnaire de serveur en cliquant sur les en-têtes de colonne. Lorsque vous cliquez sur un en-tête de colonne la première fois, les valeurs alphanumériques de la colonne sont triées dans l’ordre croissant (flèche pointant vers le haut) ; pour les trier par ordre décroissant (flèche vers le bas), cliquez une nouvelle fois dessus.  
  
## <a name="BKMK_query"></a>créer et exécuter des requêtes personnalisées sur les données de mosaïque  
Vous pouvez créer des requêtes personnalisées dans les vignettes événements, performances, Best Practices Analyzer, Services ou rôles et fonctionnalités dans le Gestionnaire de serveur. Par défaut, la zone de la barre d’outils de la vignette vous permet de sélectionner les critères de création d’une requête personnalisée est masquée ; Cliquez sur **développez** (bouton chevron sur le bord droit de la barre d’outils de la vignette) pour afficher les critères de requête.  
  
#### <a name="to-create-a-custom-query-for-tile-data"></a>Pour créer une requête personnalisée pour des données de vignette  
  
1.  Ouvrez une page de groupe de serveurs ou de rôles dans le Gestionnaire de serveur.  
  
2.  Dans une vignette événements, performances, Best Practices Analyzer, Services ou rôles et fonctionnalités, développez la zone de création de la requête en cliquant sur **développez**.  
  
3.  Cliquez sur **ajouter des critères** pour ouvrir une liste d’attributs (ou champs) qui s’appliquent à des entrées dans la vignette.  
  
4.  Sélectionnez les critères à ajouter. Lorsque vous avez terminé, cliquez sur **ajouter**. Les critères que vous avez choisis sont ajoutés dans la zone de création de requêtes.  
  
5.  Cliquez sur l’opérateur hypertexte pour sélectionner un opérateur. Par exemple, pour des critères numériques ou une date et une heure, la valeur par défaut est **inférieur ou égal à**.  
  
6.  Spécifiez des valeurs acceptables pour les critères. Par exemple, si vous avez sélectionné **date et heure**, indiquez une date au format *j/aaaa*.  
  
7.  Répétez ces étapes à partir de l’étape 3 pour ajouter d’autres critères à votre requête.  
  
    Vous pouvez ajouter une double occurrence pour des critères déjà inclus dans votre requête mais ces doublons seront ajoutés dans la requête avec l’opérateur **ou** .  
  
    par exemple, pour rechercher l’événement ID 1003 ou 1014, vous tout d’abord ajouter le critère d’ID à votre requête, définir la valeur d’ID égal à **1003**et puis ajouter un deuxième ID à votre requête en la valeur de la deuxième ID égal à  **1014**. La requête obtenue est **et ID est égal à 1003 ou ID est égal à 1014**.  
  
8.  Une fois vos critères ajoutés et vos opérateurs et valeurs définis, cliquez sur **Enregistrer** pour enregistrer la requête.  
  
9. Entrez un nom convivial pour la requête. Par exemple, la requête créée à l’étape précédente peut s’appeler **Événements de licence**.  
  
10. Après avoir examiné les résultats de la requête, cliquez sur **Effacer tout** pour effacer l’ensemble des filtres et des requêtes et afficher toutes les entrées dans la liste.  
  
11. Pour exécuter une requête enregistrée, cliquez sur **Requêtes de recherche enregistrées**, puis sur le nom de la requête enregistrée à exécuter.  
  
12. Pour supprimer une requête enregistrée, cliquez sur **Requêtes de recherche enregistrées**, puis sur **X** en regard du nom de la requête enregistrée à supprimer.  
  
## <a name="see-also"></a>Voir aussi  
[Gestionnaire de serveur](server-manager.md)  
[Afficher et configurer les performances, d’événements et des données de Service](view-and-configure-performance-event-and-service-data.md)  
  


