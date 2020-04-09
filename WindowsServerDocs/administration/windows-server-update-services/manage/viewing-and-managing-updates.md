---
title: Affichage et gestion des mises à jour
description: Rubrique Windows Server Update Service (WSUS)-comment afficher et gérer les mises à jour dans la console WSUS
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: article
ms.assetid: ac70192b-0309-4385-b697-2e8eda51911c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a2a9f7e1f1f3f648a0cba22d599ccc64e7b424d8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828442"
---
# <a name="viewing-and-managing-updates"></a>Affichage et gestion des mises à jour

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez utiliser la console WSUS pour afficher et gérer les mises à jour.

## <a name="viewing-updates"></a>Affichage des mises à jour
Sur la page **mises à jour** , vous pouvez effectuer les opérations suivantes :

-   Affichez les mises à jour. La vue d’ensemble de la mise à jour affiche les mises à jour qui ont été synchronisées à partir de la source de mise à jour sur votre serveur WSUS et qui sont disponibles pour approbation.

-   Filtrer les mises à jour. Dans l’affichage par défaut, vous pouvez filtrer les mises à jour par État d’approbation et état de l’installation. Le paramètre par défaut est pour les mises à jour non approuvées qui sont nécessaires à certains clients ou qui ont subi des échecs d’installation sur certains clients. Vous pouvez modifier cet affichage en modifiant les filtres état d’approbation et état de l’installation, puis en cliquant sur **Actualiser**.

-   Créez de nouvelles vues de mise à jour. Dans le volet **actions** , cliquez sur **nouvelle vue de mise à jour**. Vous pouvez filtrer les mises à jour par classification, produit, groupe pour lequel elles ont été approuvées et date de synchronisation. Vous pouvez trier la liste en cliquant sur l’en-tête de colonne approprié dans la barre de titre.

-   Recherchez des mises à jour. Vous pouvez rechercher une mise à jour individuelle ou un ensemble de mises à jour par titre, description, article de la base de connaissances ou le numéro du centre de réponse de sécurité Microsoft pour la mise à jour.

-   Affichez les détails, l’État et l’historique des révisions pour chaque mise à jour.

-   Approuvez et refusez les mises à jour.

#### <a name="to-view-updates"></a>Pour afficher les mises à jour

1.  Dans la console d’administration WSUS, développez le nœud mises à jour, puis cliquez sur toutes les mises à jour.

2.  Par défaut, les mises à jour sont affichées avec leur titre, leur classification, le pourcentage d’installation/non applicable et l’état d’approbation. Si vous souhaitez afficher des propriétés de mise à jour plus ou moins nombreuses, cliquez avec le bouton droit sur la barre d’en-tête de colonne et sélectionnez les colonnes appropriées.

3.  Pour effectuer un tri selon différents critères, tels que le statut du téléchargement, le titre, la classification, la date de publication ou l’état d’approbation, cliquez sur l’en-tête de colonne approprié.

#### <a name="to-filter-the-list-of-updates-displayed-on-the-updates-page"></a>Pour filtrer la liste des mises à jour affichées sur la page mises à jour

1.  Dans la console d’administration WSUS, développez **mises à jour**, puis cliquez sur **toutes les mises à jour**.

2.  Dans le volet central en regard d' **approbation**, sélectionnez l’état d’approbation souhaité et en regard de **État** , sélectionnez l’état d’installation souhaité. Cliquez sur **Actualiser**.

#### <a name="to-create-a-new-update-view-on-wsus"></a>Pour créer une vue de mise à jour sur WSUS

1.  Dans la console d’administration WSUS, développez **mises à jour**, puis cliquez sur **toutes les mises à jour**.

2.  Dans le volet **actions** , cliquez sur **nouvelle vue de mise à jour**.

3.  Dans la fenêtre **Ajouter une vue de mise à jour** , sous **étape 1 : sélectionnez des propriétés**, sélectionnez les propriétés dont vous avez besoin pour filtrer l’affichage des mises à jour :

    -   Sélectionnez les mises à jour dans une classification spécifique pour filtrer les mises à jour appartenant à une ou plusieurs classifications de mise à jour.

    -   Sélectionnez les mises à jour pour un produit spécifique afin de filtrer les mises à jour pour un ou plusieurs produits ou familles de produits.

    -   Sélectionnez les mises à jour sont approuvées pour un groupe spécifique afin de filtrer les mises à jour approuvées pour un ou plusieurs groupes d’ordinateurs.

    -   Sélectionnez les mises à jour ont été synchronisées dans une période de temps spécifique pour filtrer les mises à jour synchronisées à un moment donné.

    -   Sélectionner les mises à jour sont des mises à jour WSUS pour filtrer les mises à jour WSUS.

4.  Sous **étape 2 : modifiez les propriétés**, cliquez sur les mots soulignés pour sélectionner les valeurs souhaitées.

5.  Sous **étape 3 : spécifiez un nom**, donnez un nom à votre nouvelle vue.

6.  Cliquez sur **OK**.

Votre nouvel affichage apparaît dans le volet de l’arborescence sous mises à jour. Elle s’affiche, comme les vues standard, dans le volet central lorsque vous la sélectionnez.

#### <a name="to-search-for-an-update"></a>Pour rechercher une mise à jour

1.  Sélectionnez le nœud **mises à jour** (ou n’importe quel nœud situé sous celui-ci).

2.  Dans le volet **actions** , cliquez sur **Rechercher**.

3.  Dans la fenêtre de **recherche** , sous l’onglet **mises à jour** , entrez vos critères de recherche. Vous pouvez utiliser du texte du **titre**, de la **Description**et des champs de numéro d’article de la **base de connaissances Microsoft** . Chacun de ces éléments est une propriété figurant dans l’onglet **Détails** des propriétés de mise à jour.

#### <a name="to-view-the-properties-for-an-update"></a>Pour afficher les propriétés d’une mise à jour

1.  Dans la console d’administration WSUS, développez **mises à jour**, puis cliquez sur **toutes les mises à jour**.

2.  Dans la liste des mises à jour, cliquez sur la mise à jour que vous souhaitez afficher.

3.  Dans le volet inférieur, les différentes sections de propriétés s’affichent :

    -   La barre de titre affiche le titre de la mise à jour ; par exemple, la mise à jour de sécurité pour Windows Media Player 9 (KB911565).

    -   La section état affiche l’état d’installation de la mise à jour (les ordinateurs sur lesquels elle doit être installée, les ordinateurs sur lesquels elle a été installée avec des erreurs, les ordinateurs sur lesquels elle a été installée ou non) et les ordinateurs qui n’ont pas signalé d’État pour la mise à jour), ainsi que les informations générales (date de publication des numéros KB et MSRC).

    -   La section Description affiche une brève description de la mise à jour.

    -   La section détails supplémentaires affiche les informations suivantes :

        -   Le comportement d’installation de la mise à jour (qu’elle soit amovible ou non, demande un redémarrage, requiert une entrée de l’utilisateur ou doit être installé exclusivement).

        -   Indique si la mise à jour a des termes du contrat de licence logiciel Microsoft

        -   Produits auxquels s’applique la mise à jour

        -   Les mises à jour qui remplacent cette mise à jour

        -   Mises à jour remplacées par cette mise à jour

        -   Langues prises en charge par la mise à jour

        -   ID de mise à jour

Notez que vous ne pouvez effectuer cette procédure que sur une seule mise à jour à la fois. Si vous sélectionnez plusieurs mises à jour, seule la première mise à jour de la liste s’affiche dans le volet **Propriétés** .

## <a name="managing-updates-with-wsus"></a>Gestion des mises à jour avec WSUS
Les mises à jour sont utilisées pour la mise à jour ou la fourniture d’un remplacement de fichier complet pour les logiciels installés sur un ordinateur. Chaque mise à jour disponible sur Microsoft Update est constituée de deux composants :

-   Métadonnées : fournit des informations sur la mise à jour. Par exemple, les métadonnées fournissent des informations sur les propriétés d’une mise à jour, qui vous permet de savoir l’utilité de la mise à jour. Les métadonnées incluent également les Termes du contrat de licence logiciel Microsoft. Le package de métadonnées téléchargé pour une mise à jour est généralement plus petit que le package des fichiers de mise à jour.

-   Fichiers de mise à jour : fichiers réels requis pour installer une mise à jour sur un ordinateur.

Lorsque les mises à jour sont synchronisées sur votre serveur WSUS, les fichiers de mise à jour et les métadonnées sont stockés à deux endroits différents. Les métadonnées sont stockées dans la base de données WSUS. Les fichiers de mise à jour peuvent être stockés sur votre serveur WSUS ou sur des serveurs de Microsoft Update, en fonction de la façon dont vous avez configuré vos options de synchronisation. Si vous choisissez de stocker les fichiers de mise à jour sur les serveurs Microsoft Update, seules les métadonnées sont téléchargées au moment de la synchronisation ; vous approuvez les mises à jour via la console WSUS, puis les ordinateurs clients obtiennent les fichiers de mise à jour directement à partir de Microsoft Update au moment de l’installation. Pour plus d’informations sur les options de stockage des mises à jour, consultez la section [1,3. Choisissez une stratégie de stockage WSUS](../plan/plan-your-wsus-deployment.md#13-choose-a-wsus-storage-strategy) de l’étape 1 : préparer votre déploiement de WSUS dans le Guide de déploiement de WSUS.

Vous allez configurer et exécuter des synchronisations, ajouter des ordinateurs et des groupes d’ordinateurs, et déployer des mises à jour sur une base régulière. La liste suivante fournit des exemples de tâches générales que vous pouvez entreprendre pour mettre à jour des ordinateurs avec WSUS.

-   Déterminez un plan de gestion des mises à jour global basé sur la topologie et la bande passante de votre réseau, les besoins de votre entreprise et la structure organisationnelle.

    -   Indique s’il faut configurer une hiérarchie de serveurs WSUS et comment la hiérarchie doit être structurée.

    -   Les groupes d’ordinateurs à créer, ainsi que la manière de leur attribuer des ordinateurs (ciblage côté serveur ou côté client).

    -   La base de données à utiliser pour les métadonnées de mise à jour (par exemple, base de données interne Windows, SQL Server).

    -   Si les mises à jour doivent être synchronisées automatiquement et à quel moment.

-   Définir les options de synchronisation, telles que la source des mises à jour, la classification des produits et des mises à jour, la langue, les paramètres de connexion, l’emplacement de stockage et le calendrier de synchronisation.

-   Récupérez les mises à jour et les métadonnées associées sur votre serveur WSUS par le biais de la synchronisation à partir de Microsoft Update ou d’un serveur WSUS en amont.

-   Approuver ou refuser des mises à jour. Vous avez la possibilité d’autoriser les utilisateurs à installer les mises à jour elles-mêmes (s’il s’agit d’administrateurs locaux sur leurs ordinateurs clients).

-   Configurer les approbations automatiques. Vous pouvez également configurer si vous voulez activer l’approbation automatique des révisions des mises à jour existantes ou approuver les révisions manuellement. Si vous choisissez d’approuver les révisions manuellement, votre serveur WSUS continuera à utiliser l’ancienne version jusqu’à ce que vous approuviez manuellement la nouvelle révision.

-   Vérifiez l’état des mises à jour. Vous pouvez consulter l’état de la mise à jour, imprimer un rapport d’État ou configurer la messagerie électronique pour les rapports d’état standard.

## <a name="update-products-and-classifications"></a>Mettre à jour des produits et des classifications
Les mises à jour disponibles sur Microsoft Update sont différenciées par produit (ou famille de produits) et par classification.

Un produit est une édition spécifique d’un système d’exploitation ou d’une application, par exemple, Windows Server 2012. Une famille de produits est le système d’exploitation ou l’application de base à partir duquel les produits individuels sont dérivés. Par exemple, Microsoft Windows est une famille de produits dont Windows Server 2012 est membre. Vous pouvez sélectionner les produits ou les familles de produits pour lesquels vous souhaitez que votre serveur synchronise les mises à jour. Vous pouvez spécifier une famille de produits ou des produits individuels au sein de la famille. La sélection d’un produit ou d’une famille de produits recevra des mises à jour pour les versions actuelles et futures du produit.

Les classifications de mise à jour représentent le type de mise à jour. Pour un produit ou une famille de produits donné, les mises à jour peuvent être disponibles parmi plusieurs classifications de mise à jour (par exemple, mises à jour critiques et mises à jour de sécurité de la famille Windows 7). Le tableau suivant répertorie les classifications de mise à jour.

| Classifications des mises à jour  | Description   |
|--|--|
|Mises à jour critiques|Correctifs étendus pour des problèmes spécifiques relatifs à des bogues critiques non liés à la sécurité.|
|Mises à jour de définitions|Mises à jour de virus ou autres fichiers de définition.|
|Pilotes|Composants logiciels conçus pour prendre en charge le nouveau matériel.|
|Packs de fonctionnalités|Nouvelles versions de fonctionnalités, généralement intégrées aux produits de la prochaine version.|
|Mises à jour de sécurité|Correctifs étendus pour des produits spécifiques, en résolvant les problèmes de sécurité.|
|Service Packs|Ensembles cumulatifs de tous les correctifs, mises à jour de sécurité, mises à jour critiques et mises à jour créées depuis la sortie du produit. Les service packs peuvent également contenir un nombre limité de modifications de conception ou de fonctionnalités demandées par les clients.|
|Outils|Utilitaires ou fonctionnalités qui facilitent l’accomplissement d’une tâche ou d’un ensemble de tâches.|
|Correctifs cumulatifs|Ensemble cumulé de correctifs, de mises à jour de sécurité, de mises à jour critiques et d’autres mises à jour qui sont regroupées pour faciliter leur déploiement. Un cumul cible généralement un domaine spécifique, tel que la sécurité, ou un composant spécifique, tel que Internet Information Services (IIS).|
|Mises à jour|Correctifs étendus pour des problèmes spécifiques concernant des bogues non critiques liés à la sécurité.|

## <a name="icons-used-for-updates-in-windows-server-update-services"></a>Icônes utilisées pour les mises à jour dans Windows Server Update Services
 Les mises à jour dans WSUS sont représentées par l’une des icônes suivantes.  
 Pour afficher ces icônes, vous devez activer la colonne de remplacement dans la console des services de mise à jour.
 
### <a name="no-icon"></a>Aucune icône
 La mise à jour n’a pas de relation de remplacement avec une autre mise à jour.

 **Problèmes opérationnels :**  

 Il n'existe aucun problème opérationnel.  
 
### <a name="superseding-icon"></a>Icône de remplacement
 ![icon](../../media/wsus/wsus-superseding.png) Cette mise à jour remplace d’autres mises à jour.

 **Problèmes opérationnels :**  

 Il n'existe aucun problème opérationnel.  

### <a name="superseded--superseding-icon"></a>Icône de remplacement & remplacée
 ![icon](../../media/wsus/wsus-superseded.png) Cette mise à jour est remplacée par une autre mise à jour et remplace les autres mises à jour.

 **Problèmes opérationnels :**  

 Remplacez ces mises à jour par les mises à jour de remplacement lorsque cela est possible.
 
### <a name="superseded-icon"></a>Icône Remplacé
 ![icon](../../media/wsus/wsus-superseded-leaf.png) Cette mise à jour est remplacée par une autre mise à jour.

 **Problèmes opérationnels :**  

 Remplacez ces mises à jour par les mises à jour de remplacement lorsque cela est possible.
