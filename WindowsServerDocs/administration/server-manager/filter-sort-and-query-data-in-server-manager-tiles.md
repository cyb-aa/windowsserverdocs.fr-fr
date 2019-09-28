---
title: Filtrer, trier et interroger des données dans les vignettes du Gestionnaire de serveur
description: Gestionnaire de serveur
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 89604b73fd071030d0f800b3a38a7ac3858ef1c6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383204"
---
# <a name="filter-sort-and-query-data-in-server-manager-tiles"></a>Filtrer, trier et interroger des données dans les vignettes du Gestionnaire de serveur

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Dans Windows Server, les vignettes de Gestionnaire de serveur vous permettent de filtrer et de trier les données, ainsi que de créer et d’enregistrer des requêtes personnalisées. Vous pouvez trier, utiliser des filtres de mots clés et exécuter des requêtes sur les entrées de liste dans les vignettes événements, performances, Best Practices Analyzer, services et rôles et fonctionnalités sur les pages de groupe ou de rôle de serveur dans Gestionnaire de serveur.  
  
Cette rubrique contient les sections suivantes.  
  
-   [Filtrer les entrées de liste dans les vignettes](#BKMK_tiles)  
  
-   [Trier les entrées de liste dans les vignettes](#BKMK_sort)  
  
-   [créer et exécuter des requêtes personnalisées sur les données de mosaïque](#BKMK_query)  
  
## <a name="BKMK_tiles"></a>Filtrer les entrées de liste dans les vignettes  
La zone de texte **Filtre** est un moyen rapide de réduire la liste des entrées dans une vignette pour n’afficher que les entrées contenant une chaîne de texte précise.  
  
#### <a name="to-apply-a-filter-to-the-list-of-entries-in-a-tile"></a>Pour appliquer un filtre à la liste des entrées dans une vignette  
  
1.  Ouvrez une page de groupe de serveurs ou de rôles dans Gestionnaire de serveur.  
  
2.  Dans la zone de texte **filtre** d’une vignette événements, performances, Best Practices Analyzer, services ou rôles et fonctionnalités, tapez une chaîne que vous souhaitez filtrer.  
  
    par exemple, si vous souhaitez afficher uniquement les événements dont l’ID d’événement est 1014, tapez **1014** dans la zone de texte **filtre** . Tous les événements collectés contenant la chaîne **1014** dans un champ au minimum sont retournés en guise de résultats.  
  
3.  Notez que le filtre modifie le texte de la description sous le titre de la vignette. Au lieu de **Tout** , le texte affiché est **Résultats filtrés**.  
  
4.  Pour supprimer le filtre, supprimez la chaîne dans la zone de texte du filtre ou cliquez sur **X**.  
  
## <a name="BKMK_sort"></a>Trier les entrées de liste dans les vignettes  
Trier les entrées de liste dans Gestionnaire de serveur vignettes en cliquant sur les en-têtes de colonne. Lorsque vous cliquez sur un en-tête de colonne la première fois, les valeurs alphanumériques de la colonne sont triées dans l’ordre croissant (flèche pointant vers le haut) ; pour les trier par ordre décroissant (flèche vers le bas), cliquez une nouvelle fois dessus.  
  
## <a name="BKMK_query"></a>créer et exécuter des requêtes personnalisées sur les données de mosaïque  
Vous pouvez créer des requêtes personnalisées dans les vignettes événements, performances, Best Practices Analyzer, services ou rôles et fonctionnalités dans Gestionnaire de serveur. Par défaut, la zone de la barre d’outils de vignette dans laquelle vous sélectionnez les critères de génération d’une requête personnalisée est masquée. Cliquez sur **développer** (bouton Chevron sur le bord droit de la barre d’outils mosaïque) pour afficher les critères de la requête.  
  
#### <a name="to-create-a-custom-query-for-tile-data"></a>Pour créer une requête personnalisée pour des données de vignette  
  
1.  Ouvrez une page de groupe de serveurs ou de rôles dans Gestionnaire de serveur.  
  
2.  Dans une vignette événements, performances, Best Practices Analyzer, services ou rôles et fonctionnalités, développez la zone de création de requêtes en cliquant sur **développer**.  
  
3.  Cliquez sur **Ajouter des critères** pour ouvrir une liste d’attributs (ou de champs) qui s’appliquent aux entrées de la vignette.  
  
4.  Sélectionnez les critères à ajouter. Lorsque vous avez terminé, cliquez sur **Ajouter**. Les critères que vous avez choisis sont ajoutés dans la zone de création de requêtes.  
  
5.  Cliquez sur l’opérateur hypertexte pour sélectionner un opérateur. Par exemple, pour des critères numériques ou une date et une heure, la valeur par défaut est **inférieur ou égal à**.  
  
6.  Spécifiez des valeurs acceptables pour les critères. Par exemple, si vous avez sélectionné **date et heure**, indiquez une date au format *m/j/aaaa*.  
  
7.  Répétez ces étapes à partir de l’étape 3 pour ajouter d’autres critères à votre requête.  
  
    Vous pouvez ajouter une double occurrence pour des critères déjà inclus dans votre requête mais ces doublons seront ajoutés dans la requête avec l’opérateur **ou** .  
  
    par exemple, pour rechercher les ID d’événement 1003 ou 1014, vous devez d’abord ajouter les critères d’ID à votre requête, définir la valeur de l’ID sur **1003**, puis ajouter un deuxième ID à votre requête, en faisant de la valeur du second ID une valeur égale à **1014**. La requête obtenue est **et ID est égal à 1003 ou ID est égal à 1014**.  
  
8.  Une fois vos critères ajoutés et vos opérateurs et valeurs définis, cliquez sur **Enregistrer** pour enregistrer la requête.  
  
9. Entrez un nom convivial pour la requête. Par exemple, la requête créée à l’étape précédente peut s’appeler **Événements de licence**.  
  
10. Après avoir examiné les résultats de la requête, cliquez sur **Effacer tout** pour effacer l’ensemble des filtres et des requêtes et afficher toutes les entrées dans la liste.  
  
11. Pour exécuter une requête enregistrée, cliquez sur **Requêtes de recherche enregistrées**, puis sur le nom de la requête enregistrée à exécuter.  
  
12. Pour supprimer une requête enregistrée, cliquez sur **Requêtes de recherche enregistrées**, puis sur **X** en regard du nom de la requête enregistrée à supprimer.  
  
## <a name="see-also"></a>Voir aussi  
[Gestionnaire de serveur](server-manager.md)  
[Afficher et configurer les données de performances, d’événements et de services](view-and-configure-performance-event-and-service-data.md)  
  


