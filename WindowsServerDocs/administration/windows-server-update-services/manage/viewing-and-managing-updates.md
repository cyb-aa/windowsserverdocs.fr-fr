---
title: Affichage et gestion des mises à jour
description: Rubrique de Windows Server Update Service (WSUS) - comment afficher et gérer les mises à jour dans la console WSUS
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac70192b-0309-4385-b697-2e8eda51911c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cd517be0de3ba6ca97ca11f4bbe8f59111a01216
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870950"
---
# <a name="viewing-and-managing-updates"></a>Affichage et gestion des mises à jour

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez utiliser la console WSUS pour afficher et gérer les mises à jour.

## <a name="viewing-updates"></a>Affichage des mises à jour
Sur le **mises à jour** page, vous pouvez procédez comme suit :

-   Afficher les mises à jour. La vue d’ensemble de la mise à jour affiche les mises à jour qui ont été synchronisées à partir de la source de mise à jour sur votre serveur WSUS et sont disponibles pour approbation.

-   Filtrer les mises à jour. Dans la vue par défaut, vous pouvez filtrer par état d’approbation et l’état de l’installation des mises à jour. Le paramètre par défaut est pour les mises à jour non approuvées qui sont requis par certains clients ou qui ont eu des échecs d’installation sur certains clients. Vous pouvez modifier cet affichage en modifiant les filtres d’état état et l’installation d’approbation et puis en cliquant sur **Actualiser**.

-   Créer des vues de mise à jour. Dans le **Actions** volet, cliquez sur **nouvelle vue de la mise à jour**. Vous pouvez filtrer par classification, produit, le groupe pour lequel elles ont été approuvées et la date de synchronisation des mises à jour. Vous pouvez trier la liste en cliquant sur l’en-tête de colonne appropriée dans la barre de titre.

-   Recherche des mises à jour. Pour une mise à jour individuelle ou un ensemble de mises à jour, vous pouvez rechercher par titre, description, base de connaissances ou le nombre de Microsoft Security Response Center pour la mise à jour.

-   Afficher les détails, l’état et l’historique de révision pour chaque mise à jour.

-   Approuver ou refuser des mises à jour.

#### <a name="to-view-updates"></a>Pour afficher les mises à jour

1.  Dans la console d’administration WSUS, développez le nœud mises à jour, puis cliquez sur toutes les mises à jour.

2.  Par défaut, les mises à jour sont affichés avec leur titre, classification, pourcentage applicable/non installé et l’état d’approbation. Si vous souhaitez afficher les propriétés de mise à jour différents ou plus, avec le bouton droit de la barre de titre de colonne et sélectionnez les colonnes appropriées.

3.  Pour trier selon différents critères, tels que l’état du téléchargement, title, classification, date de publication ou état d’approbation, cliquez sur l’en-tête de colonne approprié.

#### <a name="to-filter-the-list-of-updates-displayed-on-the-updates-page"></a>Pour filtrer la liste des mises à jour affichées dans la page mises à jour

1.  Dans la console d’administration WSUS, développez **mises à jour**, puis cliquez sur **toutes les mises à jour**.

2.  Dans le volet central regard **approbation**, sélectionnez l’état d’approbation de votre choix et en regard **état** sélectionner l’état d’installation souhaitée. Cliquez sur **Actualiser**.

#### <a name="to-create-a-new-update-view-on-wsus"></a>Pour créer un nouvel affichage de mise à jour dans WSUS

1.  Dans la console d’administration WSUS, développez **mises à jour**, puis cliquez sur **toutes les mises à jour**.

2.  Dans le **Actions** volet, cliquez sur **nouvelle vue de la mise à jour**.

3.  Dans le **ajouter une vue de mettre à jour** fenêtre, sous **étape 1 : sélectionnez les propriétés**, sélectionnez les propriétés que vous avez besoin pour filtrer le mode de mise à jour :

    -   Sélectionnez les mises à jour sont dans une classification spécifique pour filtrer sur les mises à jour appartenant à un ou plusieurs classifications de mise à jour.

    -   Sélectionnez les mises à jour sont pour un produit spécifique filtrer sur les mises à jour pour un ou plusieurs produits ou les familles de produits.

    -   Sélectionnez les mises à jour sont approuvées pour un groupe spécifique filtrer sur les mises à jour approuvées pour un ou plusieurs groupes d’ordinateurs.

    -   Sélectionnez les mises à jour ont été synchronisés au sein d’une période spécifique pour filtrer sur les mises à jour synchronisées à un moment donné.

    -   Sélectionnez les mises à jour sont mises à jour WSUS pour filtrer sur les mises à jour WSUS.

4.  Sous **étape 2 : modifier les propriétés**, cliquez sur les mots soulignés pour récupérer les valeurs souhaitées.

5.  Sous **étape 3 : Spécifiez un nom**, donnez un nom à votre nouvelle vue.

6.  Cliquez sur **OK**.

Votre nouvel affichage apparaîtra dans le volet d’arborescence sous les mises à jour. Il s’affichera, comme les vues standards, dans le volet central, lorsque vous la sélectionnez.

#### <a name="to-search-for-an-update"></a>Pour rechercher une mise à jour

1.  Sélectionnez le **mises à jour** nœud (ou n’importe quel nœud dans cette section).

2.  Dans le **Actions** volet, cliquez sur **recherche**.

3.  Dans le **recherche** fenêtre, dans le **mises à jour** , entrez vos critères de recherche. Vous pouvez utiliser le texte de la **titre**, **Description**, et **numéro d’article de Base de connaissances Microsoft (KB)** champs. Chacun de ces éléments est une propriété répertoriée sur le **détails** onglet dans les propriétés de mise à jour.

#### <a name="to-view-the-properties-for-an-update"></a>Pour afficher les propriétés pour une mise à jour

1.  Dans la console d’administration WSUS, développez **mises à jour**, puis cliquez sur **toutes les mises à jour**.

2.  Dans la liste des mises à jour, cliquez sur la mise à jour que vous souhaitez afficher.

3.  Dans le volet inférieur, vous verrez les sections de la propriété différents :

    -   La barre de titre affiche le titre de la mise à jour ; par exemple, sécurité mise à jour pour Windows Media Player 9 (KB911565).

    -   La section état affiche l’état d’installation de la mise à jour (ordinateurs sur lesquels il doit être installé, les ordinateurs sur lesquels il a été installé avec des erreurs, ordinateurs sur lesquels il a été installé ou n’est pas applicable et les ordinateurs qui n’ont pas signalé état de la mise à jour), ainsi que des informations générales (date de publication de nombres Ko et MSRC, etc.).

    -   La section de Description affiche une brève description de la mise à jour.

    -   La section des détails supplémentaires affiche les informations suivantes :

        -   Le comportement d’installation de la mise à jour (si elle peut être supprimé, demande un redémarrage, nécessite une entrée utilisateur ou doit être installé exclusivement).

        -   La mise à jour ait ou non des termes du contrat de licence de logiciel Microsoft

        -   Les produits auxquels s’applique la mise à jour

        -   Les mises à jour qui remplacent cette mise à jour

        -   Les mises à jour qui sont remplacées par cette mise à jour

        -   Les langues prises en charge par la mise à jour

        -   L’ID de mise à jour

Notez que vous pouvez effectuer cette procédure sur la mise à jour qu’une seule à la fois. Si vous sélectionnez plusieurs mises à jour, uniquement la première mise à jour dans la liste s’affichera dans le **propriétés** volet.

## <a name="managing-updates-with-wsus"></a>La gestion des mises à jour avec WSUS
Mises à jour sont utilisés pour la mise à jour ou en fournissant un remplacement de fichier complet pour les logiciels installés sur un ordinateur. Chaque mise à jour est disponible sur Microsoft Update est constitué de deux composants :

-   Métadonnées : Fournit des informations sur la mise à jour. Par exemple, les métadonnées fournissent des informations pour les propriétés d’une mise à jour, ce qui vous permet de savoir ce que la mise à jour est utile. Les métadonnées incluent également les termes du contrat de licence de logiciel Microsoft. Le package de métadonnées téléchargé pour une mise à jour est généralement plus petit que le package de fichiers de mise à jour réelle.

-   Fichiers de mises à jour : Les fichiers réels requis pour installer une mise à jour sur un ordinateur.

Lorsque les mises à jour sont synchronisées sur votre serveur WSUS, les fichiers de mise à jour et les métadonnées sont stockés à deux endroits différents. Les métadonnées sont stockées dans la base de données WSUS. Fichiers de mise à jour peuvent être stockés sur votre serveur WSUS ou sur des serveurs de Microsoft Update, en fonction de la manière dont vous avez configuré vos options de synchronisation. Si vous choisissez de stocker les fichiers de mise à jour sur les serveurs Microsoft Update, seules les métadonnées sont téléchargée au moment de la synchronisation ; vous approuvez les mises à jour via la console WSUS et les ordinateurs clients obtiennent les fichiers de mise à jour directement à partir de Microsoft Update au moment de l’installation. Pour plus d’informations sur les options de stockage des mises à jour, consultez la section [1.3. Choisir une stratégie de stockage WSUS](../plan/plan-your-wsus-deployment.md#BKMK_1.3.) de l’étape 1 : Préparer votre déploiement de WSUS, dans le guide de déploiement de WSUS.

Vous devez configurer et exécuter des synchronisations, ajout d’ordinateurs et des groupes d’ordinateurs et le déploiement des mises à jour régulièrement. La liste suivante donne des exemples de tâches générales que vous pouvez entreprendre dans la mise à jour des ordinateurs avec WSUS.

-   Déterminer un plan de gestion de mise à jour globale en fonction de votre topologie de réseau et la bande passante, des besoins de l’entreprise et structure organisationnelle.

    -   Si vous souhaitez définir une hiérarchie de serveurs WSUS et comment la hiérarchie doit être structurée.

    -   Pour créer des groupes quel ordinateur et comment leur attribuer des ordinateurs (ciblage côté serveur ou côté client).

    -   Base de données à utiliser pour les métadonnées de mise à jour (par exemple, base de données interne Windows, SQL Server).

    -   Détermine si les mises à jour doivent être synchronisés automatiquement et à quel moment.

-   Définir les options de synchronisation, telles que la source de mise à jour, la classification de produit et de mise à jour, language, paramètres de connexion, emplacement de stockage et planification de la synchronisation.

-   Obtenir les mises à jour et les métadonnées associées sur votre serveur WSUS via la synchronisation à partir de Microsoft Update ou un serveur WSUS en amont.

-   Approuver ou refuser des mises à jour. Vous avez la possibilité de permettre aux utilisateurs d’installer les mises à jour eux-mêmes (s’ils sont administrateurs locaux sur leurs ordinateurs clients).

-   Configurer des approbations automatiques. Vous pouvez également configurer si vous souhaitez activer l’approbation automatique des révisions de mises à jour existantes ou approuver les révisions manuellement. Si vous choisissez d’approuver les révisions manuellement, puis votre serveur WSUS continueront à l’aide de l’ancienne version jusqu'à ce que vous approuvez manuellement la nouvelle révision.

-   Vérifiez l’état des mises à jour. Vous pouvez afficher l’état de mise à jour, imprimer un rapport d’état ou configurer la messagerie électronique pour les rapports d’état régulières.

## <a name="update-products-and-classifications"></a>Mise à jour produits et Classifications
Mises à jour disponibles sur Microsoft Update sont différenciées par produit (ou famille de produits) et la classification.

Un produit est une édition spécifique d’un système d’exploitation ou une application, par exemple, Windows Server 2012. Une famille de produits est le système d’exploitation ou l’application à partir de laquelle sont dérivés les produits. Un exemple d’une famille de produit est Microsoft Windows, dont Windows Server 2012 est un membre. Vous pouvez sélectionner les produits ou les familles de produits pour laquelle vous souhaitez que votre serveur pour synchroniser les mises à jour. Vous pouvez spécifier une famille de produits ou des produits individuels au sein de la famille. Sélection d’un produit ou une famille de produits obtiendra les mises à jour pour les versions actuelles et futures du produit.

Classifications de mise à jour représentent le type de mise à jour. Pour tout produit ou une famille de produits, mises à jour peuvent être proposées selon différentes classifications de mise à jour (par exemple, Windows 7 famille mises à jour critiques et mises à jour de sécurité). Le tableau suivant répertorie les classifications de mise à jour.

| Classifications des mises à jour  | Description   |
|--|--|
|Mises à jour critiques|Répondant correctifs pour des problèmes spécifiques critiques, pas la sécurité des bogues associés.|
|Mises à jour de définition|Mises à jour pour des virus ou autres fichiers de définition.|
|Pilotes|Composants logiciels conçus pour prendre en charge le nouveau matériel.|
|Packs de fonctionnalités|Généralement, les nouvelles versions des fonctionnalités, intégré dans les produits de la nouvelle version.|
|Mises à jour de sécurité|Correctifs largement publiés pour résoudre des problèmes de sécurité des produits spécifiques.|
|Service Packs|Ensembles cumulatifs de tous les correctifs, mises à jour de sécurité, les mises à jour critiques et les mises à jour créées depuis la version du produit. Les service packs peuvent également contenir un nombre limité de fonctionnalités ou des modifications de conception demandées par les clients.|
|Outils|Utilitaires ou des fonctionnalités qui aident à accomplir une tâche ou un ensemble de tâches.|
|Mises à jour cumulatives|Ensembles cumulés de correctifs, les mises à jour de sécurité, les mises à jour critiques et les autres mises à jour qui sont réunis pour faciliter leur déploiement. Un correctif cumulatif cible généralement un domaine spécifique, telles que la sécurité, ou un composant spécifique, telles que les Services Internet (IIS).|
|Mises à jour|Répondant correctifs pour des problèmes spécifiques non critique et non à la sécurité des bogues associés.|

## <a name="icons-used-for-updates-in-windows-server-update-services"></a>Icônes utilisées pour les mises à jour dans Windows Server Update Services
 Mises à jour dans WSUS sont représentées par une des icônes suivantes.  
 Pour afficher ces icônes, vous devez activer la colonne de remplacement dans la console Services de mise à jour.
 
### <a name="no-icon"></a>Aucune icône
 Il n’y a aucune relation de remplacement avec n’importe quel autre mise à jour pour la mise à jour.

 **Problèmes liés au fonctionnement :**  

 Il n'existe aucun problème opérationnel.  
 
### <a name="superseding-icon"></a>Icône de remplacement
 ![icon](../../media/wsus/wsus-superseding.png) Cette mise à jour remplace les autres mises à jour.

 **Problèmes liés au fonctionnement :**  

 Il n'existe aucun problème opérationnel.  

### <a name="superseded--superseding-icon"></a>Icône remplacée & remplacement
 ![icon](../../media/wsus/wsus-superseded.png) Cette mise à jour est remplacée par une autre mise à jour et remplace les autres mises à jour.

 **Problèmes liés au fonctionnement :**  

 Remplacez ces mises à jour avec les mises à jour de remplacement lorsque cela est possible.
 
### <a name="superseded-icon"></a>Icône Remplacé
 ![icon](../../media/wsus/wsus-superseded-leaf.png) Cette mise à jour est remplacée par une autre mise à jour.

 **Problèmes liés au fonctionnement :**  

 Remplacez ces mises à jour avec les mises à jour de remplacement lorsque cela est possible.
